### 嵌入HTML

要将krpano查看器嵌入到HTML页面中，需要包含核心的"krpano.js"脚本文件（文件名可以不同），并调用embedpano()函数。 embedpano函数检测浏览器支持并确定要使用哪个krpano查看器(krpano HTML5或krpano Flash查看器）。使用krpano Flash查看器时，嵌入脚本还会应用多个Flashplayer和Browser修复程序及解决方法（例如，在不支持此功能的系统上启用Flash中的鼠标滚轮使用）。这使得krpano查看器的嵌入变得容易和简单-仅包含一个脚本和一行嵌入代码。

文档主题：
- 脚本包括
- 查看器嵌入
- 嵌入参数
- 查看器移除


##### 脚本包括
krpano脚本文件需要在html页面的任何位置（但在embedpano（）使用之前）都包含一次：
```html 
<script src="krpano.js"></script>
```
一些细节和注意事项：
- 可以将'krpano.js'文件命名为其他名称-例如使用MAKE PANO或MAKE VTOUR，通常将其命名为“ pano.js”或“ tour.js”。
- 该文件本身包含两部分-krpano嵌入脚本和krpano HTML5查看器。 krpano Flash查看器是一个单独的外部文件（例如“ krpano.swf”文件）。
- 该文件始终是相同的，它不包含任何全景图或游览专用信息，这意味着它可以在多个全景图/游览或项目中共享。例如。为了使以后的更新更容易，只需对krpano查看器和插件文件使用一个全局文件夹，然后将所有其他项目链接到它们。然后，仅通过更新全局krpano文件即可更新所有项目。

##### 查看器嵌入
在html页面上的任意位置创建一个<div>元素，其中应嵌入查看器，为该div元素赋予唯一的“ id”名称，并通过CSS样式定义其大小：
```html
<div id="pano" style="width:100%;height:100%;"></div>
```
定义`<div>`元素后，使用嵌入脚本代码创建一个`<script>`元素。对于嵌入本身，有一个全局embedpano（）函数：
embedpano({...embedding parameters...});
- embedpano（）函数需要具有[嵌入参数](https://krpano.com/docu/html/#embeddingparameters)的对象。
- 完整案例
```js
<script src="krpano.js"></script>

<div id="pano" style="width:600px; height:400px;"></div>

<script>
  embedpano({swf:"krpano.swf", xml:"pano.xml", target:"pano"});
</script>
```

- 嵌入参数
embedpano（）函数需要一个Javascript对象作为参数。该对象用于通过使用parametername：value 对传递所有参数（以随机顺序）。几乎所有参数（目标参数除外）都是可选的-未定义它们时，将使用其默认值。

参数对象提供以下设置：
- xml:"krpano.xml"
   - 启动xml文件的名称和路径（相对于html文件）。
   - 未设置时，将使用.swf文件的文件名（例如，“ krpano.swf”的名称为“ krpano.xml”）。
   - 当启动时不应加载任何xml文件时，可以使用null值。

- target:"...pano-div-id..."
  - 嵌入查看器的html元素的#id。
  - 如果未设置目标，将出现“ alert（）”错误。

- swf:"krpano.swf"
  - 查看器“ .swf”文件的名称和路径（相对于html文件）。
  - 默认值为“ krpano.swf”。
  - 该文件仅用于Flashplayer，而仅使用HTML5时，则不需要此文件。

- id:"krpanoSWFObject"
  - 内部查看器对象的ID。
  - 这将是通过Javascript接口与[查看器接口](https://krpano.com/docu/js/#top)的对象。
  - 默认ID为“ krpanoSWFObject”。
  - 每个观看者都有一个唯一的ID很重要！
  - 如果已经存在具有给定ID的对象，则嵌入脚本将自动在ID的末尾添加数字，直到其唯一为止。

- bgcolor:"#000000"
  - 查看器的背景色（以html颜色格式）。
  - 默认值为“＃000000”（=黑色）。

- html5:"auto"
  - 设置krpano HTML5查看器用法。
  - 可能的设置：
    - auto - 有了该设置，默认情况下将尽可能使用krpano HTML5查看器（因为版本1.19起，在较早的版本中，Flash被用作默认值）！如果无法使用HTML5，则Flash查看器将用作备用。
    - preferred - 尽可能使用krpano HTML5查看器，否则使用Flash（自1.19版开始与auto相同）。
    - fallback - 尽可能使用krpano Flash查看器。当Flash不可用时，只能将krpano HTML5查看器用作备用。
    - only - 仅使用krpano HTML5查看器-切勿使用krpano Flash查看器。通过该设置，将尽可能使用krpano HTML5查看器。当系统/浏览器不兼容HTML5时，将显示[错误消息](https://krpano.com/docu/html/#onerror)。
    - always - 无论系统和浏览器是否支持必要的功能，请始终使用krpano HTML5查看器！
    注意-此设置应仅用于内部测试，请勿用于发布！
    - never - 切勿使用krpano HTML5查看器，强制使用krpano Flash查看器。
  - 其他扩展设置（用于控制渲染方法）：
    - +webgl - 仅在WebGL可用时才使用krpano HTML5查看器。注意-某些功能仅在WebGL可用时才可用-例如全景视频支持，WebVR支持，立体渲染，球形和圆柱形图像，视图失真（鱼眼，littleplanet），...
    - +css3d - 首选使用CSS3D渲染而不是WebGL渲染（默认情况下，如果可用，默认情况下将首选WebGL）。css3d设置应仅用于内部测试，而不能用于发布！
  - 用法示例：
    - prefer+webgl , 仅在WebGL可用时才使用krpano HTML5查看器，否则请使用Flash。
    - only+webgl, 仅在WebGL可用时才使用krpano HTML5查看器，否则显示[错误消息](https://krpano.com/docu/html/#onerror)。

- flash:""
  - 设置krpano Flash Viewer的用法。
  - 这与[html5](https://krpano.com/docu/html/#html5)设置基本相同，只是相反。它可以用于更好的网址，例如通过使用`flash = prefer`而不是`html5 = fallback`。
  - 设置[Flash](https://krpano.com/docu/html/#flash)设置后，它将映射到html5设置并覆盖它。
  - 可能的设置：
    - 自动或不设置-使用[html5](https://krpano.com/docu/html/#html5)设置。
    - prefer - 尽可能使用krpano Flash查看器。仅当没有Flashplayer并且系统/浏览器兼容HTML5时，才使用krpano HTML5 Viewer。此设置将映射到`html5=fallback`
    - fallback - 尽可能使用krpano HTML5查看器。当HTML5不可用时，只能将krpano Flash查看器用作备用。此设置将映射为html5 = prefer
    - only - 仅使用krpano Flash查看器-请勿使用krpano HTML5查看器。通过该设置，将尽可能使用krpano Flash查看器。如果没有Flashplayer，则将显示错误消息。此设置将映射到html5 = never。
    - never - 从不使用krpano Flash查看器，仅使用krpano Flash查看器。
此设置将映射到html5 = only

- wmode:"..."
  - 设置[Flashplayer wmode](https://helpx.adobe.com/flash/kb/flash-object-embed-tag-attributes.html#main_Using_Window_Mode__wmode__values_)设置。
  - 可能的设置：
    - window - Flashplayer默认为系统支持和性能之间的折衷。注意-在某些系统和浏览器上，html元素在此模式下不能与Flashplayer重叠！有关详细信息，请参见此[wmode链接](https://helpx.adobe.com/flash/kb/flash-object-embed-tag-attributes.html#main_Using_Window_Mode__wmode__values_)。
    - opaque - 允许其他html元素与Flashplayer重叠（取决于系统和浏览器，这可能会导致渲染速度变慢和抖动）。
    - opaque-flash - 与opaque相同，但仅适用于Flashplayer（HTML5查看器将忽略它-请参阅下面的HTML5注释）。
    - transparent - 使Flashplayer背景透明，以允许看到Flashplayer后面的html元素，并且还允许其他html元素与Flashplayer重叠（取决于系统和浏览器，这可能会导致显示速度变慢和抖动）。
    - transparent-flash - 与透明相同，但仅适用于Flashplayer（HTML5查看器将忽略它-请参阅下面的HTML5注释）。
    - direct - 最佳性能，硬件加速的演示，在许多系统和浏览器上没有html重叠（这通常是最快的模式，但是在不兼容或较旧的系统和浏览器上，这可能会导致速度降低）。
  - 默认情况下，krpano将使用wmode = direct，但Chrome除外-默认情况下，将使用wmode = window（性能更好，调整大小时不显示黑窗）。
  - HTML5注意：wmode设置通常是Flashplayer设置，但是krpano HTML5查看器也会评估wmode = opaque和wmode = transparent，并使该查看器背景也透明。使用HTML5查看器时，始终可以重叠html元素本身。

- localfallback:"http://localhost:8090"
  - 当使用file：//网址在本地运行HTML5内容时，一些浏览器（尤其是Chrome和Safari）限制了动态加载数据文件！在krpano HTML5查看器中，这会影响xml和插件的加载。
  - 有关这种情况的更多信息，请参见此处-本地使用。
  - 为了避免在这种情况下仅收到xml加载错误，在这种情况下，嵌入脚本会检查是否可以加载，如果不能，则提供一些替代解决方案。
  - 可能的设置：
    - krpano测试服务器的URL（默认情况下为http：// localhost：8090）




















