# WebGL：部署压缩构建

在发布模式下构建 WebGL 项目（请参阅[发布构建](PublishingBuilds.html)）时，Unity 会压缩构建输出文件以减少构建的下载大小。可从 Publishing Settings（菜单：__Edit__ > __Project Settings__ > __Player__ > __Publishing Settings__）中的 Compression Format 选项中选择其使用的压缩类型：

* __gzip__：这是默认选项。gzip 文件比 Brotli 文件更大，但构建速度更快，且所有浏览器都基于 http 和 https 实现此格式的本机支持。

* __Brotli__：Brotli 压缩提供最佳压缩比。Brotli 压缩文件明显小于 gzip，但需要很长的压缩时间，因此增加了发布版本的迭代时间。Chrome 和 Firefox 基于 https 实现对 Brotli 压缩的本机支持（请参阅 [WebGL 浏览器兼容性](webgl-browsercompatibility.html)以了解更多信息）。

* __Disabled__：此选项禁用压缩。如果要在后期处理脚本中实现您自己的压缩，请使用此选项。

压缩构建的 Unity 构建可在任何浏览器上工作。Unity 包含一个用 JavaScript 编写的软件解压缩程序；当服务器未启用 http 传输级别的压缩时，可回退到此解压缩程序。

## 高级：本机浏览器解压缩

浏览器可在下载构建数据时在本机处理 Unity 构建的解压缩。这样做的好处是可避免因在 JavaScript 中解压缩文件而导致的额外延迟，从而缩短启动时间。要让浏览器在本机处理解压缩，必须配置 Web 服务器以使用适当的 http 标头提供压缩文件：这些头告诉浏览器该数据是使用 gzip 或 Brotli 压缩的，因此浏览器在传输数据时对数据进行解压缩。Brotli 压缩仅在基于 https 的情况下受到 Firefox 和 Chrome 的支持，而 gzip 压缩受到所有浏览器的支持。请参阅 [WebGL 浏览器兼容性](webgl-browsercompatibility.html)以了解更多信息。

## 设置 Web 服务器

本机浏览器解压缩的设置过程取决于 Web 服务器。本页面上的说明适用于两种最常见的 Web 服务器：__Apache__ 和 __IIS__。请注意，这些说明应该适用于默认设置，但可能需要进行调整以符合具体配置。具体而言，如果已使用其他服务器端配置来压缩托管的文件（这可能会干扰此设置），可能会出现问题。基本思想是在服务器响应中附加 *Content-Encoding* 标头（对应于构建时使用的压缩类型）。这将允许浏览器在下载期间在本机和异步执行解压缩。

### Apache

Apache 服务器使用不可见的 *.htaccess* 文件实现服务器配置。下面的代码显示了可用于强制执行本机浏览器解压缩的 *.htaccess* 文件的示例。请注意，Apache 服务器配置设置是可选的。

对于 gzip 压缩的构建，请将以下 *.htaccess* 文件放入 *Build* 子文件夹：

```
<IfModule mod_mime.c>
  AddEncoding gzip .unityweb
</IfModule>
```


对于 brotli 压缩的构建，请将以下 *.htaccess* 文件放入 *Build* 子文件夹：

```
<IfModule mod_mime.c>
  AddEncoding br .unityweb
</IfModule>
```

## IIS

__必要的 IIS 服务器配置步骤：__

默认情况下，IIS 服务器不提供未知 MIME 类型的静态内容。为了使 Unity 构建能够在 IIS 上工作，首先需要将 *.unityweb* 扩展名与 *application/octet-stream* 内容类型相关联。有两种方法可以实现该目的：

*使用 IIS Manager 界面：*
在 IIS Manager 面板中选择您的网站，打开 *MIME Types* 功能，然后选择 *Add…* 操作。将 *.unityweb* 设置为文件扩展名，并将 *application/octet-stream* 设置为 MIME 类型，然后单击 *OK*。

*使用服务器配置文件：*
IIS 使用 *web.config* 文件实现服务器配置。这些配置文件反映了 IIS Manager 中针对特定文件夹所做的所有更改。为了将 *application/octet-stream* MIME 类型与 *.unityweb* 扩展名相关联，可使用以下 *web.config* 文件：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<system.webServer>
		<staticContent>
	<remove fileExtension=".unityweb" />
<mimeMap fileExtension=".unityweb" mimeType="application/octet-stream" />
		</staticContent>
	</system.webServer>
</configuration>
```

请注意，配置文件会影响所有服务器子文件夹，因此只需在服务器根文件夹中设置 *.unityweb* 扩展名的 MIME 类型即可。

可选的 IIS 服务器配置步骤：


为了加快构建启动速度，可选择性使用以下配置文件。请注意，为了使用此设置，需要安装 Microsoft 的 [IIS URL 重写 IIS 模块](http://www.iis.net/downloads/microsoft/url-rewrite)；否则，浏览器将抛出 500 内部服务器错误。

对于 gzip 压缩的构建，请将以下 *web.config* 文件放入 *Build* 子文件夹：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<system.webServer>
		<staticContent>
			<remove fileExtension=".unityweb" />
			<mimeMap fileExtension=".unityweb" mimeType="application/octet-stream" />
		</staticContent>
		<rewrite>
			<outboundRules>
			<rule name="Append gzip Content-Encoding header">
			<match serverVariable="RESPONSE_Content-Encoding" pattern=".*" />
			<conditions>
				<add input="{REQUEST_FILENAME}" pattern="\.unityweb$" />
			</conditions>
			<action type="Rewrite" value="gzip" />
			</rule>
			</outboundRules>
		</rewrite>
	</system.webServer>
</configuration>
```

对于 brotli 压缩的构建，请将以下 *web.config* 文件放入 *Build* 子文件夹：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<system.webServer>
		<staticContent>
			<remove fileExtension=".unityweb" />
			<mimeMap fileExtension=".unityweb" mimeType="application/octet-stream" />
		</staticContent>
		<rewrite>
			<outboundRules>
			<rule name="Append br Content-Encoding header">
			<match serverVariable="RESPONSE_Content-Encoding" pattern=".*" />
			<conditions>
				<add input="{REQUEST_FILENAME}" pattern="\.unityweb$" />
			</conditions>
			<action type="Rewrite" value="br" />
			</rule>
			</outboundRules>
		</rewrite>
	</system.webServer>
</configuration>

```

请注意，当已在服务器目录的层级视图中的较高级别覆盖内容类型时，需要使用 `<remove fileExtension=".unityweb" />` 行对此进行处理，否则可能导致服务器异常。

----
*  <span class="page-edit">2017-05-24  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 版更新</span>


