<p>为了方便处理一些有用的信息，预处理器定义了一些预处理标识符，也就是预定义宏。预定义宏的名称都是以&ldquo;__&rdquo;（两条下划线）开头和结尾的，如果宏名是由两个单词组成，那么中间以&ldquo;_&rdquo;（一条下划线）进行连接。并且，宏名称一般都由大写字符组成。<br />在日常项目编程中，预定义宏尤其对多目标平台代码的编写通常具有重大意义。通过预定义宏，程序员使用&ldquo;#ifdef&rdquo;与&ldquo;#endif&rdquo;等预处理指令，就可使平台相关代码只在适合于当前平台的代码上编译，从而在同一套代码中完成对多平台的支持。从这个意义上讲，平台信息相关的宏越丰富，代码的多平台支持越准确。<br />标准 C 语言提供的一些标准预定义宏如表 1 所示。</p>
<table><caption>表 1 常用的标准预定义宏</caption>
<tbody>
<tr>
<th>宏</th>
<th>描&nbsp;述</th>
</tr>
<tr>
<td>__DATE__</td>
<td>丐前源文件的编泽口期，用&nbsp;&ldquo;Mmm dd yyy&rdquo;形式的字符串常量表示</td>
</tr>
<tr>
<td>__FILE__</td>
<td>当前源文件的名称，用字符串常量表示</td>
</tr>
<tr>
<td>__LINE__</td>
<td>当前源义件中的行号，用十进制整数常量表示，它可以随#line指令改变</td>
</tr>
<tr>
<td>__TIME__</td>
<td>当前源文件的最新编译吋间，用&ldquo;hh:mm:ss&rdquo;形式的宁符串常量表示</td>
</tr>
<tr>
<td>__STDC__</td>
<td>如果今前编泽器符合ISO标准，那么该宏的值为1，否则未定义</td>
</tr>
<tr>
<td>__STDC_VERSION__</td>
<td>如果当前编译器符合C89，那么它被定义为199409L;如果符合C99，那么它被定义为199901L:在其他情况下，该宏为宋定义</td>
</tr>
<tr>
<td>__STDC_HOSTED__</td>
<td>(C99)如果当前是宿主系统，则该宏的值为1;如果当前是独立系统，则该宏的值为0</td>
</tr>
<tr>
<td>__STDC_IEC_559_</td>
<td>(C99)如果浮点数的实现符合IEC 60559标准时，则该宏的值为1，否则为未定义</td>
</tr>
<tr>
<td>__STDC_IEC_559_COMPLEX__</td>
<td>(C99)如果复数运算实现符合IEC60559标准时，则该宏的伉为1,否则为未定义</td>
</tr>
<tr>
<td>__STDC_ISO_10646__</td>
<td>(C99 )定义为长整型常量，yyyymmL表示wchai_t值遵循ISO 10646标准及其指定年月的修订补充，否则该宏为未定义</td>
</tr>
</tbody>
</table>
<p>除标准 C 语言提供的标准宏之外，各种编译器也都提供了自己的自定义预定义宏。可以通过表 2 所示的指令来查看不同编译器对预定义宏的支持情况。</p>
<table><caption>表 2 不同编译器的预定义宏查看指令</caption>
<tbody>
<tr>
<th>编译器</th>
<th>宏指令（c)</th>
<th>宏指令（<a href="http://c.biancheng.net/cplus/" target="_blank" rel="noopener">C++</a>)</th>
</tr>
<tr>
<td>Clang/LLVM</td>
<td>clang -dM -E -x c /dev/null</td>
<td>clang++ -dM -E -x C++ /dev/null</td>
</tr>
<tr>
<td>GNU&nbsp;<a href="http://c.biancheng.net/gcc/" target="_blank" rel="noopener">GCC</a>/G++</td>
<td>gcc -dM -E -x c /dev/null</td>
<td>g++ -dM -E -x&nbsp;C++&nbsp;/dev/null</td>
</tr>
<tr>
<td>Hewlett-Packard C/aC++</td>
<td>cc -dM -E -x c /clev/null</td>
<td>aCC -dM -E -x&nbsp;C++&nbsp;/dev/null</td>
</tr>
<tr>
<td>IBM XL C/C++</td>
<td>xlc -qshowmacros -E /dev/null</td>
<td>xlc++ -qshowmacros -E /dev/null</td>
</tr>
<tr>
<td>Intel ICC7ICPC</td>
<td>icc -dM -E -x c /dev/null</td>
<td>icpc -dM -E -x&nbsp;C++&nbsp;/dev/null</td>
</tr>
<tr>
<td>Oracle Solaris Studio</td>
<td>cc -xduinpmacros -E /dev/null</td>
<td>CC -xduinpmacros -E /clev/null</td>
</tr>
<tr>
<td>Portland Group PGCC/PGCPP</td>
<td>pgcc -dM -E</td>
<td>&nbsp;</td>
</tr>
</tbody>
</table>
<p>对于这些预定义宏的应用，基本上随处可见，下面举例介绍。<br />利用&ldquo;__DATE__&rdquo;和&ldquo;__TIME__&rdquo;宏可以用来确定程序编译的时间。如下面的示例代码所示：</p>
<div class="snippet-container">
<div class="sh_bright snippet-wrap">
<pre class="language-c"><code>int main (void)
{
  printf("Copyright (c) Powered by www.develhome.com\n");
  printf("Compiled on %s at %s\n", __DATE__,__TIME__);
  return 0;
}</code></pre>
</div>
</div>
<p>利用&ldquo;__STDC__&rdquo;与&ldquo;__STDC_VERSION__&rdquo;宏可以编写那些需要兼容标准 C 和非标准 C 编译器的程序，如下面的示例代码所示：</p>
<div class="snippet-container">
<div class="sh_bright snippet-wrap">
<pre class="language-c"><code>#ifdef __STDC__
/* Some version of standard C */
#if defined(__STDC__VERSION__)&amp;&amp;__STDC_VERSION__&gt;=199901L
/* C99 */
#elif defined(__STDC_VERSION__)&amp;&amp;__STDC_VERSION__&gt;=199409L
/* C89 and amendment 1 */
#else
/* C89 but not amendment 1*/
#endif
#else /* __STDC__not defined */
/*Not Standard C*/
#endif</code></pre>
</div>
</div>
<p>利用__FILE__、__LINE__与__FUNCTION__（或者__func__）预定义宏的组合，在调试程序的时候可以很简单地在程序运行期进行异常跟踪。如下面的示例代码所示：</p>
<div class="snippet-container">
<div class="sh_bright snippet-wrap">
<pre class="language-c"><code>#include &lt;stdio.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;sys/stat.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;unistd.h&gt;
#include &lt;stdlib.h&gt;
#define MESSAGE(message,assertion) \
do{\
  if(!(assertion)){\
    printf("line %d in %s(%s)", __LINE__, __FILE__,__FUNCTION__);\
    if(message){\
      printf(":%s",message);\
    }\
    printf("\n");\
    abort();\
  }\
}while(0)
int OpenFile(const char *filename)
{
  int fd;
  MESSAGE("文件名称不能够为空",filename);
  MESSAGE("文件不存在",0==access(filename,F_OK));
  fd = open(filename,O_RDONLY);
  close(fd);
  return 0;
}
int main(int argc,char **argv)
{
  MESSAGE("命令参数不能够为空",argc==2);
  OpenFile(argv[1]);
  return 0;
}</code></pre>
</div>
</div>
<p>最后还需要注意的是，如果用户重定义&ldquo;#define&ldquo;或取消了&ldquo;#undef&rdquo;预定义宏，那么其结果是&ldquo;未定义&rdquo;的。因此，在代码编写中，应该尽量避免自定义宏与预定义宏名称相同的情况发生。</p>
