{{noteta
|G1=IT
}}

在[[计算机|计算机]]领域，'''进程标识符'''（{{lang-en|'''process identifier'''}}，又略称为'''进程ID'''（{{lang-en|'''process ID'''}}）、'''PID'''）是大多数[[操作系统|操作系统]]的[[内核|内核]]用于唯一标识[[进程|进程]]的一个数值。这一数值可以作为许多函数调用的参数，以使调整进程优先级、[[Kill_(命令)|杀死]]进程之类的进程控制行为成为可能。

== 类UNIX系统 ==

在[[类UNIX|类UNIX]]操作系统中，新进程都衍自[[系统调用|系统调用]]<tt>[[fork_(系统调用)|fork()]]</tt>。<tt>fork()</tt>调用会将子进程的PID返回给[[父进程|父进程]]，使其可以之指代子进程，从而在需要时以之为函数参数。例如，若以子进程PID为参数调用<tt>[[waitpid|waitpid()]]</tt>，可使父进程以休眠状态等待子进程结束；若以之为参数调用<tt>[[Kill_(命令)|kill()]]</tt>，便可结束对应子进程。

在各PID中，较为特别的是0号PID和1号PID。PID为0者为交换进程（{{lang-en|swapper}}），属于内核进程，负责[[分页|分页]]任务；PID为1者则常为[[init|init]]进程，主要负责启动与关闭系统。值得一提的是，1号PID本来并非是特意为init进程预留的，而init进程之所以拥有这一PID，则是因为init即是内核创建的第一个进程。不过，现今的许多UNIX/类UNIX系统内核也有以进程形式存在的其他组成部分，而在这种情况下，1号PID则仍为init进程保有，以与之前系统保持一致<ref>{{cite book|title=Basics Of Os Unix And Shell Programming|publisher=Tata McGraw-Hill Education|year=2006|author=ISRD Group}}</ref>。

PID的分配机制则因系统而异，一般从0开始，然后顺序分配，直到达到一个最大值（亦因系统而异），而后又从300开始重新分配；在[[Mac_OS_X|Mac OS X]]和[[HP-UX|HP-UX]]下，则是由100开始重分配。在分配PID时，若遇到已分配的PID，则直接跳过，继续递增查找下一个可分配PID。

== Microsoft Windows ==
[[Microsoft_Windows|Microsoft Windows]]系列操作系统提供了一系列API，以使开发者可以获取相关PID，如用于获取当前进程PID<code>GetCurrentProcessId()</code><ref>{{citation |title=GetCurrentProcessId Function |publisher=Windows Developer Center |url=http://msdn.microsoft.com/en-us/library/ms683180(VS.85).aspx |accessdate=2009-05-20}}</ref>、返回其他进程PID的<code>GetProcessId()</code><ref>{{citation |title=ProcessId Function |publisher=Windows Developer Center |url=http://msdn.microsoft.com/en-us/library/ms683215(v=vs.85).aspx |accessdate=2011-03-05}}</ref>。在操作系统内部，进程ID与线程ID在同一个命名空间中，因此二者不会重合。

== PID文件 ==

有些长时间运行的进程（如[[MySQL|MySQL]]的守护进程）会将自己的PID写入一个文件，以使其他进程可寻获之。

== 参见 ==
* <code>[[pidof|pidof]]</code>
* [[用户标识符|用户标识符]]（UID）
* {{Tsl|en|Group identifier|组标识符}}（GID）

== 参考资料 ==
{{Reflist|30em}}
{{FOLDOC}}

{{DEFAULTSORT:进程标识符}}
[[Category:进程|Category:进程]]