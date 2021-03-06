{{NoteTA
|G1=Windows
|G2=IT
|1=zh-tw:指標;zh-cn:指针;}}
[[Windows操作系统|Windows操作系统]]的'''资源'''(resources)是指嵌入可执行程序([[EXE|EXE]], [[Dynamic-Link_Library|DLL]], [[控制面板|CPL]], {{tsl|en|Multilingual User Interface|多语言用户界面|MUI}}等)的只读数据。<ref>{{cite web|url=http://msdn.microsoft.com/en-us/library/windows/desktop/aa380599%28v=vs.85%29.aspx|title=About Resource Files|publisher=[[Microsoft|Microsoft]]|accessdate=24 Feb 2014}}</ref><ref>{{cite web|url=http://msdn.microsoft.com/en-us/library/ms648009%28v=vs.85%29.aspx|title=Resource Types|publisher=[[Microsoft|Microsoft]]|accessdate=24 Feb 2014}}</ref><ref>{{cite web|url=http://msdn.microsoft.com/en-us/library/cc194804.aspx|title=Windows Resource Files|publisher=[[Microsoft|Microsoft]]|accessdate=24 Feb 2014}}</ref>

[[Windows_API|Windows API]]提供了便捷访问应用程序资源的方法。 

==类型==
每种资源有类型及名字，它们是数值标识符或字符串。 

Windows预定义的资源类型：
* [[游标|鼠标指標]]与动画指標
* [[圖示|圖示]] 
* [[點陣圖|點陣圖]]
* [[對話方塊|對話方塊]]模板
* [[字型|字型]]
* [[HTML|HTML]] 文档
* [[字符串|字串]] 与消息模板
* EXE/DLL檔案版本資料

程序员也可以自行定义资源中的数据类型。

==使用==
Windows为一个程序显示的图标实际上是它的EXE文件中的第一个圖示资源。如果EXE文件没有圖示资源，则显示一个标准圖示。

EXE或DLL文件的版本资源显示在它们的属性页的''Version'' tab中。

一个资源总是附加了某种语言。Windows自动使用最适合的可行的语言。这使得程序适合于用户的locale的语言。

编辑工具可以修改嵌入在EXE或DLL文件中的资源。这常用于把应用程序中的字符串翻译为另一种语言，或者修改图标或位图。
==开发==
 
# 为cursors, icons, bitmaps, dialog boxes, fonts创建单独的文件；
# 创建一个资源定义脚本(.rc)文件来描述应用程序用到的资源；
# 使用预处理器RC.exe编译该脚本:<ref>[https://msdn.microsoft.com/en-us/library/aa381055(v=vs.85).aspx MSDN:Using RC (The RC Command Line)]</ref>  <tt>RC [options] script-file</tt>
# 使用[[链接器|链接器]]把编译后的资源(.res)文件加入到要生成的可执行程序中。

==参考文献==
{{reflist}}

==外部链接==
* [http://msdn.microsoft.com/en-us/library/cc194804.aspx MSDN: Windows Resource Files Guide]
* [http://msdn.microsoft.com/en-us/library/aa380599%28v=vs.85%29.aspx MSDN: Better Resource File Guide with reference]

{{Wikibooks|Windows Programming|Resource Script Reference}}

[[Category:Microsoft_Windows|Category:Microsoft Windows]]