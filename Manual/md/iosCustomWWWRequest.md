#Customising WWW requests on iOS

You can use a plugin to replace the default code that Unity uses when handling requests with the [WWW](../ScriptReference/WWW.html) class. You might do this, for example, to change the default caching behaviour, which caches on the basis of the URL alone.

##示例

The following code (described in more detail below) disables the default caching method and adds a "secret" header field to the request in addition to the headers specified from the game's script code.

````
	// Code placed at Plugins/iOS/CustomConnection.mm within the Assets folder.

	#include "Unity/WWWConnection.h"

	@interface UnityWWWCustomRequestProvider : UnityWWWRequestDefaultProvider
	{
	}
	+ (NSMutableURLRequest*)allocRequestForHTTPMethod:(NSString*)method url:(NSURL*)url headers:(NSDictionary*)headers;
	@end

	@implementation UnityWWWCustomRequestProvider
	+ (NSMutableURLRequest*)allocRequestForHTTPMethod:(NSString*)method url:(NSURL*)url headers:(NSDictionary*)headers
	{
		NSMutableURLRequest* request = [super allocRequestForHTTPMethod:method url:url headers:headers];

		// let's pretend for security reasons we dont want ANY cache nor cookies
		request.cachePolicy = NSURLRequestReloadIgnoringLocalCacheData;
		[request setHTTPShouldHandleCookies:NO];

		// let's pretend we want special secret info in header
		[request setValue:@"123456789"forHTTPHeaderField:@"Secret"];

		return request;
	}
	@end

	@interface UnityWWWCustomConnectionDelegate : UnityWWWConnectionDelegate
	{
	}
	@end

	@implementation UnityWWWCustomConnectionDelegate
	- (NSCachedURLResponse*)connection:(NSURLConnection*)connection willCacheResponse:(NSCachedURLResponse*)cachedResponse
	{
		// we dont want caching
		return nil;
	}
	- (void)connection:(NSURLConnection*)connection didReceiveResponse:(NSURLResponse*)response
	{
		// let's just print something here for test
		[super connection:connection didReceiveResponse:response];
		if([response isMemberOfClass:[NSHTTPURLResponse class]])
			::printf_console("We've got response with status: %d\n", [(NSHTTPURLResponse*)response statusCode]);
	}
	@end

	IMPL_WWW_DELEGATE_SUBCLASS(UnityWWWCustomConnectionDelegate);
	IMPL_WWW_REQUEST_PROVIDER(UnityWWWCustomRequestProvider);
````

The code breaks down as follows. Firstly, we create a subclass of UnityWWWRequestDefaultProvider to provide a modified NSURLRequest object for the connection. Another option is simply to implement the UnityWWWRequestProvider protocol in the class from scratch. In this case, however, we want to make use of Unity's existing code.

````
	@interface UnityWWWCustomRequestProvider : UnityWWWRequestDefaultProvider
	{
	}
	+ (NSMutableURLRequest*)allocRequestForHTTPMethod:(NSString*)method url:(NSURL*)url headers:(NSDictionary*)headers;
	@end
````

The "action" happens in `allocRequestForHTTPMethod` which starts by calling the same method on the superclass:

````
	@implementation UnityWWWCustomRequestProvider
	+ (NSMutableURLRequest*)allocRequestForHTTPMethod:(NSString*)method url:(NSURL*)url headers:(NSDictionary*)headers
	{
		NSMutableURLRequest* request = [super allocRequestForHTTPMethod:method url:url headers:headers];
````

From there, we disable the default data caching done by iOS and also disable cookies:

````
	request.cachePolicy = NSURLRequestReloadIgnoringLocalCacheData;
	[request setHTTPShouldHandleCookies:NO];
````

Next, we add a header field called "Secret" that provides some additional data. In a real game, this might be a value checked by the server to verify the source of the connection.

````
	[request setValue:@"123456789"forHTTPHeaderField:@"Secret"];
````

Having created the custom request handling class, we also need to create a subclass of `UnityWWWConnectionDelegate` to customise the handling of the connection:

````
	@interface UnityWWWCustomConnectionDelegate : UnityWWWConnectionDelegate
	{
	}
	@end
````

We can disable data caching by returning a nil value from the `connection:willCacheResponse:` method:

````
	- (NSCachedURLResponse*)connection:(NSURLConnection*)connection willCacheResponse:(NSCachedURLResponse*)cachedResponse
	{
		// we dont want caching
		return nil;
	}
````

We can handle the connection itself from `connection:didReceiveResponse:`, which should call the same method on the superclass to actually get the data. Here, we simply print a message to the console when the connection happens but you could implement anything you like here:

````
	- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response
	{
		// let's just print something here for test
		[super connection:connection didReceiveResponse:response];
		if([response isMemberOfClass:[NSHTTPURLResponse class]])
			::printf_console("We've got response with status: %d\n", [(NSHTTPURLResponse*)response statusCode]);
	}
````

Finally, we register our new connection delegate and request provider with Unity so they will be called whenever a WWW request is issued from a script:

````
	IMPL_WWW_DELEGATE_SUBCLASS(UnityWWWCustomConnectionDelegate);
	IMPL_WWW_REQUEST_PROVIDER(UnityWWWCustomRequestProvider);
````

The custom code is transparent to the use of the WWW class within Unity - no extra code needs to be added to the C# script.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
