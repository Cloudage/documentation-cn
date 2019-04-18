Reporting crash bugs on iOS
===========================

Before submitting a bug report, please check the [iOS Troubleshooting](TroubleShootingIPhone.html) page, where you will find solutions to common crashes and other problems.

If your application crashes in the Xcode debugger then you can add valuable information to your bug report as follows:-

1. Click Continue (__Debug__ &gt; __Continue__) twice
1. Open the debugger console (__View__ &gt; __Debug Area__ &gt; __Activate Console__) and enter (in the console): **thread backtrace all**
1. Copy **all** console output and send it together with your bug report.

Similarly, when reporting app freezes, you can click __Pause__ and do the above steps.

If your application crashes on the iOS device, you should retrieve the crash report as described [here](http://developer.apple.com/iphone/library/technotes/tn2008/tn2151.html#ACQUIRING_CRASH_REPORTS) on Apple's website. Please attach the crash report, your built application and console log to your bug report before submitting.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
