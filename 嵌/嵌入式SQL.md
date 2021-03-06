'''嵌入式SQL'''（英文: '''Embedded SQL'''）是一种将[[SQL|SQL语句]]直接写入[[C语言|C语言]]、[[COBOL|COBOL]]、[[FORTRAN|FORTRAN]]及[[Ada|Ada]]等[[编程语言|编程语言]][[源代码|源代码]]中的方法。藉此方法，可使得应用程序拥有了访问数据以及处理数据的能力。在这一方法中，将SQL文嵌入的目标源码的语言称为'''宿主语言'''。

在1986年发布的'''SQL86'''标准中定义了对于COBOL、FORTRAN及PI/L等语言的嵌入式SQL的规范。在1989年发布的'''SQL89'''规范中，定义了对于C语言的嵌入式SQL的规范。一些大型的[[数据库|数据库]]厂商发布的数据库产品中，都提供了对于嵌入式SQL的支持。比如[[Oracle|Oracle]]、[[DB2|DB2]]等。

== 嵌入式SQL的工作原理 ==
提供对于嵌入式SQL的支持，需要数据库厂商除了提供[[DBMS|DBMS]]之外，还必须提供一些工具。为了实现对于嵌入式SQL的支持，技术上必须解决以下4个问题<ref> {{cite book|title=《数据库系统教程(上册)》|author=王能斌|publisher=电子工业出版社|isbn=7-5053-7827-9|pages=77页|date=2002年8月}}</ref>：

#宿主语言的[[编译器|编译器]]不可能识别和接受SQL文，需要解决如何将SQL的宿主语言源代码编译成可执行码。
#宿主语言的应用程序如何与DBMS之间传递数据和消息。
#如何把对数据的查询结果逐次赋值给宿主语言程序中的[[变量|变量]]以供其处理。
#数据库的数据类型与宿主语言的数据类型有时不完全对应或等价，如何解决必要的数据类型转换问题。

[[File:EmbeddedSQLWorkFlow.PNG|500px]]
为了解决上述这些问题，数据库厂商需要提供一个嵌入式SQL的[[预编译器|预编译器]]，把包含有嵌入式SQL文的宿主语言源码转换成纯宿主语言的代码。这样一来，源码即可使用宿主语言对应的编译器进行编译。通常情况下，经过嵌入式SQL的预编译之后，原有的嵌入式SQL会被转换成一系列函数调用。因此，数据库厂商还需要提供一些列函数库，以确保[[链接器|链接器]]能够把代码中的函数调用与对应的实现链接起来。

== 嵌入式SQL的扩展语法 ==
嵌入式SQL中除了可以执行标准SQL文之外，为了对应嵌入的需要，还增加了一些额外的语法成分。主要包含以下内容：
* 宿主变量使用声明的语法
* 数据库访问的语法
* [[事务|事务]]控制的语法
* [[游标|游标]]操作的语法

=== 示例代码 ===
以下展示了用于连接[[PostgreSQL|PostgreSQL]]数据库并执行一次数据查询的嵌入式SQL源码（宿主语言为[[C语言|C语言]]）：

<source lang="c">
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

EXEC SQL BEGIN DECLARE SECTION;
int var_c1;
char var_c2[21] = { 0x00 };
EXEC SQL END DECLARE SECTION;

int main(int argc, char* argv[])
{
    /* 步骤1: 建立连接 */
    EXEC SQL WHENEVER SQLERROR SQLPRINT;   /* 声明异常发生时的处理动作统一为打印消息 */
    EXEC SQL CONNECT TO postgres@localhost:5432 USER postgres/xxxx;   /* 指定连接的目标数据库，用户名，密码 */  
     
    /* 步骤2: 利用游标执行查询 */
    EXEC SQL DECLARE foo_bar CURSOR FOR SELECT c1, c2 FROM tb1;    /* 声明一个游标使之用于执行SELECT文 */
    EXEC SQL OPEN foo_bar;                                         /* 打开游标从而使SELECT文被执行 */
    EXEC SQL FETCH foo_bar INTO :var_c1, :var_c2;                  /* 获取一行数据 */
    
    printf("C1: %d, C2: %s\n", var_c1, var_c2);
 
    EXEC SQL CLOSE foo_bar;                                        /* 关闭所打开的游标 */

    /* 步骤3: 关闭连接 */
    EXEC SQL DISCONNECT CURRENT;                   

    return 0;
}
</source>



在[[Cygwin|Cygwin]]平台上，利用PostgreSQL 7.4.5所自带的嵌入式SQL的预编译器[[ECPG|ECPG]]（版本号 3.1.1）进行预编译，生成的纯C语言代码如下所示：
<source lang="c">
/* Processed by ecpg (3.1.1) */
/* These include files are added by the preprocessor */
#include <ecpgtype.h>
#include <ecpglib.h>
#include <ecpgerrno.h>
#include <sqlca.h>
#line 1 "ESQL_CONNECT.pgc"
/* End of automatic include section */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/* exec sql begin declare section */

#line 6 "ESQL_CONNECT.pgc"
 int  var_c1   ;
 
#line 7 "ESQL_CONNECT.pgc"
 char  var_c2 [ 21 ]  = { 0x00 } ;
/* exec sql end declare section */
#line 8 "ESQL_CONNECT.pgc"

int main(int argc, char* argv[])
{
    /* 步骤1: 建立连接 */
    /* exec sql whenever sqlerror  sqlprint ; */

#line 13 "ESQL_CONNECT.pgc"
 
    { ECPGconnect(__LINE__, 0, "postgres@localhost:5432" , "postgres" , "asdf1234" , NULL, 0); 
#line 14 "ESQL_CONNECT.pgc"

if (sqlca.sqlcode < 0) sqlprint();}
#line 14 "ESQL_CONNECT.pgc"    
    /* 步骤2: 利用游标执行查询 */
    /* declare foo_bar  cursor  for select  c1  , c2   from tb1    */
#line 17 "ESQL_CONNECT.pgc"

    { ECPGdo(__LINE__, 0, 1, NULL, "declare foo_bar  cursor  for select  c1  , c2   from tb1   ", ECPGt_EOIT, ECPGt_EORT);
#line 18 "ESQL_CONNECT.pgc"
if (sqlca.sqlcode < 0) sqlprint();}
#line 18 "ESQL_CONNECT.pgc"

    { ECPGdo(__LINE__, 0, 1, NULL, "fetch foo_bar", ECPGt_EOIT, 
    ECPGt_int,&(var_c1),(long)1,(long)1,sizeof(int), 
    ECPGt_NO_INDICATOR, NULL , 0L, 0L, 0L, 
    ECPGt_char,(var_c2),(long)21,(long)1,21*sizeof(char), 
    ECPGt_NO_INDICATOR, NULL , 0L, 0L, 0L, ECPGt_EORT);
#line 19 "ESQL_CONNECT.pgc"

if (sqlca.sqlcode < 0) sqlprint();}
#line 19 "ESQL_CONNECT.pgc"

    printf("C1: %d, C2: %s\n", var_c1, var_c2);
 
    { ECPGdo(__LINE__, 0, 1, NULL, "close foo_bar", ECPGt_EOIT, ECPGt_EORT);
#line 23 "ESQL_CONNECT.pgc"

if (sqlca.sqlcode < 0) sqlprint();}
#line 23 "ESQL_CONNECT.pgc"


    /* 步骤3: 关闭连接 */
    { ECPGdisconnect(__LINE__, "CURRENT");
#line 26 "ESQL_CONNECT.pgc"

if (sqlca.sqlcode < 0) sqlprint();}
#line 26 "ESQL_CONNECT.pgc"
    return 0;
}
</source>


生成后的代码中所调用的'''ECPGconnect'''等函数，由安装PostgreSQL所自带的ECPG[[函数库|库]]（本例中为<code>libecpg.so</code>）提供具体的实现，供[[链接器|链接器]]将其与上述C语言代码编译生成的[[目标文件|目标文件]]进行链接。

== 支持嵌入式SQL的数据库产品 ==
以下列出支持嵌入式SQL的数据库产品以及各自支持的宿主语言
=== Oracle Database ===
; Ada : Pro*Ada在Oracle 7.3的版本中被加入产品族，并且在Oracle 8中被替换为SQL*Module。但在此之后就一直没有更新<ref>{{cite web
|url=http://download.oracle.com/docs/cd/B10501_01/server.920/a96530/migcompa.htm#1010868
|title=Ada Support in Version 8
|work=Oracle9i Database Migration, Release 2 (9.2), Chapter 5. Compatibility and Interoperability
|publisher=[[Oracle|Oracle]]
|accessdate=2008-07-14
}}</ref>。SQL*Module支持Ada 83.
; C/C++ : Pro*C 在Oracle 8 时被替换成了Pro*C/C++。之后Pro*C/C++ 到Oracle Database 11g仍都在被支持。
; COBOL : Pro*COBOL到Oracle Database 11g仍都在被支持。
; Fortran : Pro*FORTRAN 在Oracle 8之后的Oracle版本中就不再被更新，但Bug修正仍在维护中<ref name="langalts"/>。
; Pascal : Pro*Pascal在Oracle 8之后的Oracle版本中就不再被更新<ref name="langalts"/>。
; PI/L : Pro*PL/I 自Oracle 8之后就不再被更新，但文档中仍然有记述<ref name="langalts">{{cite web
|url=http://download.oracle.com/docs/cd/A64702_01/doc/server.805/a58232/ch01.htm#505
|title=Language Alternatives
|work=Pro*COBOL Precompiler Programmer's Guide, Release 8.0, Chapter 1. Introduction
|publisher=[[Oracle|Oracle]]
|accessdate=2008-07-14
}}</ref>。

=== IBM DB2 ===
IBM DB2的版本9中提供了对于C/C++，COBOL，Java等宿主语言的嵌入式SQL的支持。

=== [[PostgreSQL|PostgreSQL]] ===
; C/C++ : PostgreSQL 自版本6.3起就提供了对于C/C++的嵌入式SQL的支持，以ECPG组件的形式存在。

== 参见 ==
* [[SQL|SQL文]]

== 注释 ==
{{reflist}}

{{Databases}}

[[Category:数据库|Category:数据库]]