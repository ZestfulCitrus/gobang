# gobang
基于Java开发的基础版五子棋！希望能帮助到大家！您的每个star对我都十分珍贵！

[开发过程及演示:博客园](https://www.cnblogs.com/godoforange/p/10935821.html)

<h1>五子棋手把手教你写：</h1>
<h2>写在前面的话：</h2>
<p>回想起从前初学代码的五子棋简直写的不像样子。今天闲来无事就写了个五子棋的小程序。</p>
<p>如果有需要可以从github上下载：github地址：<a href="https://github.com/GodofOrange/gobang.git" target="_blank">https://github.com/GodofOrange/gobang.git</a></p>
<p>一来呢回忆一下很久以前写代码时的感觉。</p>
<p>二来呢顺便帮下诸位有需求的学生，顺利的Ctrl+C。</p>
<p>五子棋的运行效果如下。</p>
<p><img src="https://img2018.cnblogs.com/blog/1590876/201905/1590876-20190528102841828-1891851591.gif" alt="" /></p>
<p>&nbsp;</p>
<h2>开发环境：</h2>
<p>这个小程序是基于Java实现的。因此呢需要提前安装JDK环境。(老油条忽略此条信息)</p>
<p>开发环境jdk1.8 + eclipse</p>
<p>eclipse 目录结构如下所示,就三个类啊。</p>
<p><img src="https://img2018.cnblogs.com/blog/1590876/201905/1590876-20190528103032056-924111687.png" alt="" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>棋盘数据结构核心：</h2>
<p>无论你做数据库开发还是做一些小程序，第一时间考虑的必须是需求+建模。把核心设计出来。</p>
<p>此次我们用一个二维数组作为棋盘，每条线交叉的地方设为二维数组的值，并约定：</p>
<p>0=空</p>
<p>1=白棋</p>
<p>2=黑棋</p>
<p>然后对应的把下棋，悔棋，判断输赢(横竖斜)和清盘的算法都实现出来。</p>
<p>具体展现如下：</p>
<p>悔棋时候我们需要用一个栈来保存我们之前下棋的信息：</p>
<div class="cnblogs_code">
<pre><span style="color: #008000;">/*</span><span style="color: #008000;">*
     * 在该位置下棋  1:white 2:black 
     * @param x 横坐标
     * @param y 纵坐标
     * @param var 棋子种类
     * @return 1:white 赢   2:black赢
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> ChessIt(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span> y,<span style="color: #0000ff;">int</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">) {
        </span><span style="color: #0000ff;">if</span><span style="color: #000000;">(__CanInput(x,y)) {
            core[x][y] </span>=<span style="color: #0000ff;">var</span><span style="color: #000000;">;
            Chess chess </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> Chess(x,y);
            stack.push(chess);
            </span><span style="color: #0000ff;">return</span> checkVictory(x, y, <span style="color: #0000ff;">var</span><span style="color: #000000;">);
        }
        </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">return</span> -<span style="color: #800080;">1</span><span style="color: #000000;">;
    }

</span><span style="color: #008000;">//</span><span style="color: #008000;">悔棋</span>
    <span style="color: #0000ff;">public</span><span style="color: #000000;"> boolean RetChess() {
        </span><span style="color: #0000ff;">if</span>(stack.isEmpty()) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        Chess chess </span>=<span style="color: #000000;"> stack.pop();
        core[chess.x][chess.y]</span>= <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">true</span><span style="color: #000000;">;
    }</span></pre>
</div>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>总体：Core.java 的代码如下&middot;:</p>
<p>&nbsp;</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">package main;

import java.util.Stack;

</span><span style="color: #008000;">/*</span><span style="color: #008000;">*
 * @author GodofOrange
 *    棋盘数据结构
 </span><span style="color: #008000;">*/</span>
<span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Core {
    </span><span style="color: #008000;">//</span><span style="color: #008000;">棋盘大小</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[][] core;
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> x;
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> y;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">记录下棋的类</span>
    <span style="color: #0000ff;">class</span><span style="color: #000000;"> Chess{
        </span><span style="color: #0000ff;">int</span><span style="color: #000000;"> x;
        </span><span style="color: #0000ff;">int</span><span style="color: #000000;"> y;
        </span><span style="color: #0000ff;">public</span> Chess(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span><span style="color: #000000;"> y) {
            </span><span style="color: #0000ff;">this</span>.x=<span style="color: #000000;">x;
            </span><span style="color: #0000ff;">this</span>.y=<span style="color: #000000;">y;
        }
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">栈</span>
    Stack&lt;Chess&gt;<span style="color: #000000;"> stack;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">构造方法</span>
    <span style="color: #0000ff;">public</span> Core(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span><span style="color: #000000;"> y) {
        stack </span>= <span style="color: #0000ff;">new</span> Stack&lt;&gt;<span style="color: #000000;">();
        core </span>= <span style="color: #0000ff;">new</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[x][y];
        </span><span style="color: #0000ff;">this</span>.x=<span style="color: #000000;">x;
        </span><span style="color: #0000ff;">this</span>.y=<span style="color: #000000;">y;
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">检查该地是否有空位置</span>
    <span style="color: #0000ff;">private</span> boolean __CanInput(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span><span style="color: #000000;"> y) {
        </span><span style="color: #0000ff;">if</span>(core[x][y]==<span style="color: #800080;">0</span>) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">true</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">false</span><span style="color: #000000;">;
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">判断输赢</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> checkVictory(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span> y,<span style="color: #0000ff;">int</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;">横向判断</span>
        <span style="color: #0000ff;">int</span> trans = <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i=x-<span style="color: #800080;">4</span>;i&lt;x+<span style="color: #800080;">5</span>;i++<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">if</span>(i&lt;<span style="color: #800080;">0</span>||i&gt;=<span style="color: #0000ff;">this</span>.x) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(core[i][y]==<span style="color: #0000ff;">var</span><span style="color: #000000;">) {
                trans</span>++<span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                trans</span>=<span style="color: #800080;">0</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">if</span>(trans==<span style="color: #800080;">5</span>) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">;
        }
        </span><span style="color: #008000;">//</span><span style="color: #008000;">纵向判断</span>
        <span style="color: #0000ff;">int</span> longitudinal = <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i=y-<span style="color: #800080;">4</span>;i&lt;y+<span style="color: #800080;">5</span>;i++<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">if</span>(i&lt;<span style="color: #800080;">0</span>||i&gt;=<span style="color: #0000ff;">this</span>.y) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(core[x][i]==<span style="color: #0000ff;">var</span><span style="color: #000000;">) {
                longitudinal</span>++<span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                longitudinal</span>=<span style="color: #800080;">0</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">if</span>(longitudinal==<span style="color: #800080;">5</span>) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">;
        }
        </span><span style="color: #008000;">//</span><span style="color: #008000;">从左上到右下</span>
        <span style="color: #0000ff;">int</span> leftUPToDown = <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i=x-<span style="color: #800080;">4</span>,j=y+<span style="color: #800080;">4</span>;i&lt;x+<span style="color: #800080;">5</span>&amp;&amp;j&gt;y-<span style="color: #800080;">5</span>;i++,j--<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">if</span>(i&lt;<span style="color: #800080;">0</span>||i&gt;=<span style="color: #0000ff;">this</span>.x||j&lt;<span style="color: #800080;">0</span>||j&gt;=<span style="color: #0000ff;">this</span>.y) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(core[i][j]==<span style="color: #0000ff;">var</span><span style="color: #000000;">) {
                leftUPToDown</span>++<span style="color: #000000;">;
            }</span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                leftUPToDown</span>=<span style="color: #800080;">0</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">if</span>(leftUPToDown==<span style="color: #800080;">5</span>) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">;
        }
        </span><span style="color: #008000;">//</span><span style="color: #008000;">从左下到右上</span>
        <span style="color: #0000ff;">int</span> leftDownToUP = <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i=x+<span style="color: #800080;">4</span>,j=y+<span style="color: #800080;">4</span>;i&gt;x-<span style="color: #800080;">5</span>&amp;&amp;j&gt;y-<span style="color: #800080;">5</span>;i--,j--<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">if</span>(i&lt;<span style="color: #800080;">0</span>||i&gt;=<span style="color: #0000ff;">this</span>.x||j&lt;<span style="color: #800080;">0</span>||j&gt;=<span style="color: #0000ff;">this</span>.y) <span style="color: #0000ff;">continue</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(core[i][j]==<span style="color: #0000ff;">var</span><span style="color: #000000;">) {
                leftDownToUP</span>++<span style="color: #000000;">;
            }</span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                leftDownToUP</span>=<span style="color: #800080;">0</span><span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">if</span>(leftDownToUP==<span style="color: #800080;">5</span>) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">;
        }
        </span><span style="color: #0000ff;">return</span> <span style="color: #800080;">0</span><span style="color: #000000;">;
    }
    </span><span style="color: #008000;">/*</span><span style="color: #008000;">*
     * 在该位置下棋  1:white 2:black 
     * @param x 横坐标
     * @param y 纵坐标
     * @param var 棋子种类
     * @return 1:white 赢   2:black赢
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span> ChessIt(<span style="color: #0000ff;">int</span> x,<span style="color: #0000ff;">int</span> y,<span style="color: #0000ff;">int</span> <span style="color: #0000ff;">var</span><span style="color: #000000;">) {
        </span><span style="color: #0000ff;">if</span><span style="color: #000000;">(__CanInput(x,y)) {
            core[x][y] </span>=<span style="color: #0000ff;">var</span><span style="color: #000000;">;
            Chess chess </span>= <span style="color: #0000ff;">new</span><span style="color: #000000;"> Chess(x,y);
            stack.push(chess);
            </span><span style="color: #0000ff;">return</span> checkVictory(x, y, <span style="color: #0000ff;">var</span><span style="color: #000000;">);
        }
        </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">return</span> -<span style="color: #800080;">1</span><span style="color: #000000;">;
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">悔棋</span>
    <span style="color: #0000ff;">public</span><span style="color: #000000;"> boolean RetChess() {
        </span><span style="color: #0000ff;">if</span>(stack.isEmpty()) <span style="color: #0000ff;">return</span> <span style="color: #0000ff;">false</span><span style="color: #000000;">;
        Chess chess </span>=<span style="color: #000000;"> stack.pop();
        core[chess.x][chess.y]</span>= <span style="color: #800080;">0</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">true</span><span style="color: #000000;">;
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">获得棋盘状态</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[][] getCore(){
        </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">this</span><span style="color: #000000;">.core;
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;">重新开始</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> Restart() {
        </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> i=<span style="color: #800080;">0</span>;i&lt;<span style="color: #0000ff;">this</span>.x;i++<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">for</span>(<span style="color: #0000ff;">int</span> j=<span style="color: #800080;">0</span>;j&lt;<span style="color: #0000ff;">this</span>.y;j++<span style="color: #000000;">) {
                </span><span style="color: #0000ff;">this</span>.core[i][j]=<span style="color: #800080;">0</span><span style="color: #000000;">;
            }
        }
        </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.stack.clear();
    }
}</span></pre>
</div>
<h2>Windows的前端代码</h2>
<h2>&nbsp;</h2>
<p>在上一步我们把一个五子棋的数据结构实现了之后，我们下一步就需要用JavaSwing的知识来画前端。</p>
<p>首先我们定义一个类来继承JFrame,从而包含JFrame的所有功能。</p>
<p>以下是JFrame常用的方法。</p>
<div class="cnblogs_code">
<pre>jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);<span style="color: #008000;">//</span><span style="color: #008000;">关闭JFrame时运行System.exit(0)</span>
jFrame.setLocationRelativeTo(<span style="color: #0000ff;">null</span>);<span style="color: #008000;">//</span><span style="color: #008000;">屏幕中央显示</span>
jFrame.setVisible(<span style="color: #0000ff;">true</span>);<span style="color: #008000;">//</span><span style="color: #008000;">可见</span></pre>
</div>
<p>&nbsp;</p>
<p>其次我们需要单击屏幕进行下棋，所以我们需要符合鼠标单击事件的接口。因此我们去接上MouseListener的接口。</p>
<p>再然后我们重写JFrame里的paint方法来画画。</p>
<p>具体体现：如下</p>
<p>其中横线和竖线都是调用的Graphics中的drawLine方法。</p>
<p>画圈圈用的是drawOval和fillOval分别是画空心圆和画实心圆。</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">@Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> paint(Graphics g) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
<span style="color: #000000;">        super.paint(g);
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 横</span>
        <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">)
            g.drawLine(</span><span style="color: #800080;">30</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">570</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span><span style="color: #000000;">);
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 竖线</span>
        <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">)
            g.drawLine(</span><span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">60</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">570</span><span style="color: #000000;">);

        </span><span style="color: #0000ff;">int</span>[][] board =<span style="color: #000000;"> core.getCore();
        </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> j = <span style="color: #800080;">0</span>; j &lt; <span style="color: #800080;">19</span>; j++<span style="color: #000000;">) {
                </span><span style="color: #0000ff;">if</span> (board[i][j] == <span style="color: #800080;">1</span><span style="color: #000000;">)
                    g.drawOval(</span><span style="color: #800080;">20</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">50</span> + j * <span style="color: #800080;">30</span>, <span style="color: #800080;">20</span>, <span style="color: #800080;">20</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">if</span>(board[i][j]==<span style="color: #800080;">2</span><span style="color: #000000;">)
                    g.fillOval(</span><span style="color: #800080;">20</span>+i*<span style="color: #800080;">30</span>, <span style="color: #800080;">50</span>+j*<span style="color: #800080;">30</span>, <span style="color: #800080;">20</span>, <span style="color: #800080;">20</span><span style="color: #000000;">);
            }
        }
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">60</span>, <span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">悔棋</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">80</span><span style="color: #000000;">);
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">120</span>,<span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">开始</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">140</span><span style="color: #000000;">);
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">180</span>,<span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">设置</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">200</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">Code by 秃桔子 QQ:1243137612</span><span style="color: #800000;">"</span>, <span style="color: #800080;">600</span>,<span style="color: #800080;">260</span><span style="color: #000000;">);
    }</span></pre>
</div>
<p>&nbsp;</p>
<p>再然后我们需要确定每次鼠标单击的事件和信息。</p>
<p>具体实现如下：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">@Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mousePressed(MouseEvent e) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
        <span style="color: #0000ff;">if</span> (e.getX() &lt; <span style="color: #800080;">570</span> &amp;&amp; e.getY() &lt; <span style="color: #800080;">570</span><span style="color: #000000;">) {
            </span><span style="color: #0000ff;">int</span> a = core.ChessIt(_CgetX(e.getX()), (_CgetY(e.getY())), <span style="color: #0000ff;">var</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
            </span><span style="color: #0000ff;">if</span> (a == <span style="color: #800080;">1</span><span style="color: #000000;">) {
                JOptionPane.showMessageDialog(</span><span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">白的赢了</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">恭喜</span><span style="color: #800000;">"</span><span style="color: #000000;">, JOptionPane.DEFAULT_OPTION);;
            }
            </span><span style="color: #0000ff;">if</span>(a==<span style="color: #800080;">2</span><span style="color: #000000;">) {
                JOptionPane.showMessageDialog(</span><span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">黑的赢了</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">恭喜</span><span style="color: #800000;">"</span><span style="color: #000000;">, JOptionPane.DEFAULT_OPTION);;
            }
            </span><span style="color: #0000ff;">if</span>(a!=-<span style="color: #800080;">1</span><span style="color: #000000;">) {
                </span><span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">2</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            }
        }
        </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">60</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">90</span><span style="color: #000000;">) {
            core.RetChess();
            </span><span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">2</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
        </span><span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">120</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">150</span><span style="color: #000000;">) {
            core.Restart();
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
        </span><span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">180</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">210</span><span style="color: #000000;">) {
            Object[] options </span>= {<span style="color: #800000;">"</span><span style="color: #800000;">白先</span><span style="color: #800000;">"</span>,<span style="color: #800000;">"</span><span style="color: #800000;">黑先</span><span style="color: #800000;">"</span><span style="color: #000000;">};
            </span><span style="color: #0000ff;">int</span> n = JOptionPane.showOptionDialog(<span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">红先还是黑先？</span><span style="color: #800000;">"</span>,<span style="color: #800000;">"</span><span style="color: #800000;">游戏设置</span><span style="color: #800000;">"</span>,JOptionPane.YES_NO_OPTION,JOptionPane.QUESTION_MESSAGE, <span style="color: #0000ff;">null</span>,options,options[<span style="color: #800080;">0</span><span style="color: #000000;">]);
            </span><span style="color: #0000ff;">if</span>(n==<span style="color: #800080;">0</span>) <span style="color: #0000ff;">this</span>.<span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(n==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">this</span>.<span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.core.Restart();
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
    }</span></pre>
</div>
<p>再然后每次单击的时候进行repaint重绘将代码重写出来。</p>
<p>&nbsp;</p>
<p>这些东西我也不记得，看api就好了。</p>
<p>下面是总体源码：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">package main;

import java.awt.Graphics;
import java.awt.</span><span style="color: #0000ff;">event</span><span style="color: #000000;">.MouseEvent;
import java.awt.</span><span style="color: #0000ff;">event</span><span style="color: #000000;">.MouseListener;

import javax.swing.JFrame;
import javax.swing.JOptionPane;
</span><span style="color: #008000;">/*</span><span style="color: #008000;">*
 * 
 * @author GodofOrange
 * @see 图形界面
 </span><span style="color: #008000;">*/</span>
<span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Windows extends JFrame implements MouseListener {
    </span><span style="color: #0000ff;">public</span><span style="color: #000000;"> Core core;
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> final <span style="color: #0000ff;">long</span> serialVersionUID = <span style="color: #800080;">1L</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> <span style="color: #0000ff;">var</span> = <span style="color: #800080;">1</span><span style="color: #000000;">;
    </span><span style="color: #0000ff;">public</span><span style="color: #000000;"> Windows(String title) {
        super(title);
        core </span>= <span style="color: #0000ff;">new</span> Core(<span style="color: #800080;">19</span>, <span style="color: #800080;">19</span><span style="color: #000000;">);
        </span><span style="color: #0000ff;">this</span>.setSize(<span style="color: #800080;">800</span>, <span style="color: #800080;">600</span><span style="color: #000000;">);
        </span><span style="color: #0000ff;">this</span>.setLocation(<span style="color: #800080;">800</span>, <span style="color: #800080;">300</span><span style="color: #000000;">);
        </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        </span><span style="color: #0000ff;">this</span>.setVisible(<span style="color: #0000ff;">true</span><span style="color: #000000;">);
        </span><span style="color: #0000ff;">this</span>.setResizable(<span style="color: #0000ff;">false</span><span style="color: #000000;">);
        </span><span style="color: #0000ff;">this</span>.addMouseListener(<span style="color: #0000ff;">this</span><span style="color: #000000;">);
    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> paint(Graphics g) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
<span style="color: #000000;">        super.paint(g);
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 横</span>
        <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">)
            g.drawLine(</span><span style="color: #800080;">30</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">570</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span><span style="color: #000000;">);
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 竖线</span>
        <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">)
            g.drawLine(</span><span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">60</span>, <span style="color: #800080;">30</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">570</span><span style="color: #000000;">);

        </span><span style="color: #0000ff;">int</span>[][] board =<span style="color: #000000;"> core.getCore();
        </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = <span style="color: #800080;">0</span>; i &lt; <span style="color: #800080;">19</span>; i++<span style="color: #000000;">) {
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> j = <span style="color: #800080;">0</span>; j &lt; <span style="color: #800080;">19</span>; j++<span style="color: #000000;">) {
                </span><span style="color: #0000ff;">if</span> (board[i][j] == <span style="color: #800080;">1</span><span style="color: #000000;">)
                    g.drawOval(</span><span style="color: #800080;">20</span> + i * <span style="color: #800080;">30</span>, <span style="color: #800080;">50</span> + j * <span style="color: #800080;">30</span>, <span style="color: #800080;">20</span>, <span style="color: #800080;">20</span><span style="color: #000000;">);
                </span><span style="color: #0000ff;">if</span>(board[i][j]==<span style="color: #800080;">2</span><span style="color: #000000;">)
                    g.fillOval(</span><span style="color: #800080;">20</span>+i*<span style="color: #800080;">30</span>, <span style="color: #800080;">50</span>+j*<span style="color: #800080;">30</span>, <span style="color: #800080;">20</span>, <span style="color: #800080;">20</span><span style="color: #000000;">);
            }
        }
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">60</span>, <span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">悔棋</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">80</span><span style="color: #000000;">);
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">120</span>,<span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">开始</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">140</span><span style="color: #000000;">);
        g.drawRect(</span><span style="color: #800080;">690</span>,<span style="color: #800080;">180</span>,<span style="color: #800080;">50</span>, <span style="color: #800080;">30</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">设置</span><span style="color: #800000;">"</span>,<span style="color: #800080;">700</span>,<span style="color: #800080;">200</span><span style="color: #000000;">);
        g.drawString(</span><span style="color: #800000;">"</span><span style="color: #800000;">Code by 秃桔子 QQ:1243137612</span><span style="color: #800000;">"</span>, <span style="color: #800080;">600</span>,<span style="color: #800080;">260</span><span style="color: #000000;">);
    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mouseClicked(MouseEvent arg0) {

    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mouseEntered(MouseEvent e) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
<span style="color: #000000;">
    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mouseExited(MouseEvent e) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
<span style="color: #000000;">
    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mousePressed(MouseEvent e) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
        <span style="color: #0000ff;">if</span> (e.getX() &lt; <span style="color: #800080;">570</span> &amp;&amp; e.getY() &lt; <span style="color: #800080;">570</span><span style="color: #000000;">) {
            </span><span style="color: #0000ff;">int</span> a = core.ChessIt(_CgetX(e.getX()), (_CgetY(e.getY())), <span style="color: #0000ff;">var</span><span style="color: #000000;">);
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
            </span><span style="color: #0000ff;">if</span> (a == <span style="color: #800080;">1</span><span style="color: #000000;">) {
                JOptionPane.showMessageDialog(</span><span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">白的赢了</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">恭喜</span><span style="color: #800000;">"</span><span style="color: #000000;">, JOptionPane.DEFAULT_OPTION);;
            }
            </span><span style="color: #0000ff;">if</span>(a==<span style="color: #800080;">2</span><span style="color: #000000;">) {
                JOptionPane.showMessageDialog(</span><span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">黑的赢了</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">恭喜</span><span style="color: #800000;">"</span><span style="color: #000000;">, JOptionPane.DEFAULT_OPTION);;
            }
            </span><span style="color: #0000ff;">if</span>(a!=-<span style="color: #800080;">1</span><span style="color: #000000;">) {
                </span><span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
                </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">2</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            }
        }
        </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">60</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">90</span><span style="color: #000000;">) {
            core.RetChess();
            </span><span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span>(<span style="color: #0000ff;">var</span>==<span style="color: #800080;">2</span>) <span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
        </span><span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">120</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">150</span><span style="color: #000000;">) {
            core.Restart();
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
        </span><span style="color: #0000ff;">if</span>(e.getX()&gt;<span style="color: #800080;">690</span>&amp;&amp;e.getX()&lt;<span style="color: #800080;">760</span>&amp;&amp;e.getY()&gt;<span style="color: #800080;">180</span>&amp;&amp;e.getY()&lt;<span style="color: #800080;">210</span><span style="color: #000000;">) {
            Object[] options </span>= {<span style="color: #800000;">"</span><span style="color: #800000;">白先</span><span style="color: #800000;">"</span>,<span style="color: #800000;">"</span><span style="color: #800000;">黑先</span><span style="color: #800000;">"</span><span style="color: #000000;">};
            </span><span style="color: #0000ff;">int</span> n = JOptionPane.showOptionDialog(<span style="color: #0000ff;">null</span>,<span style="color: #800000;">"</span><span style="color: #800000;">红先还是黑先？</span><span style="color: #800000;">"</span>,<span style="color: #800000;">"</span><span style="color: #800000;">游戏设置</span><span style="color: #800000;">"</span>,JOptionPane.YES_NO_OPTION,JOptionPane.QUESTION_MESSAGE, <span style="color: #0000ff;">null</span>,options,options[<span style="color: #800080;">0</span><span style="color: #000000;">]);
            </span><span style="color: #0000ff;">if</span>(n==<span style="color: #800080;">0</span>) <span style="color: #0000ff;">this</span>.<span style="color: #0000ff;">var</span>=<span style="color: #800080;">1</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">if</span>(n==<span style="color: #800080;">1</span>) <span style="color: #0000ff;">this</span>.<span style="color: #0000ff;">var</span>=<span style="color: #800080;">2</span><span style="color: #000000;">;
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.core.Restart();
            </span><span style="color: #0000ff;">this</span><span style="color: #000000;">.repaint();
        }
    }

    @Override
    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> mouseReleased(MouseEvent e) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> TODO Auto-generated method stub</span>
<span style="color: #000000;">
    }

    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> _CgetX(<span style="color: #0000ff;">int</span><span style="color: #000000;"> x) {
        x </span>-= <span style="color: #800080;">30</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">if</span> (x % <span style="color: #800080;">15</span> &lt;= <span style="color: #800080;">7</span><span style="color: #000000;">)
            </span><span style="color: #0000ff;">return</span> x / <span style="color: #800080;">30</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">else</span>
            <span style="color: #0000ff;">return</span> x / <span style="color: #800080;">30</span> + <span style="color: #800080;">1</span><span style="color: #000000;">;
    }

    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> _CgetY(<span style="color: #0000ff;">int</span><span style="color: #000000;"> y) {
        y </span>-= <span style="color: #800080;">60</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">if</span> (y % <span style="color: #800080;">15</span> &lt;= <span style="color: #800080;">7</span><span style="color: #000000;">)
            </span><span style="color: #0000ff;">return</span> y / <span style="color: #800080;">30</span><span style="color: #000000;">;
        </span><span style="color: #0000ff;">else</span>
            <span style="color: #0000ff;">return</span> y / <span style="color: #800080;">30</span> + <span style="color: #800080;">1</span><span style="color: #000000;">;
    }
}</span></pre>
</div>
<p>然后就是启动函数了</p>
<p>这个函数放哪都行-.-。。。。。一看就懂吧？</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">package main;

</span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Main {
    </span><span style="color: #008000;">/*</span><span style="color: #008000;">* 启动函数
     * @param args
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> main(String[] args) {
        </span><span style="color: #0000ff;">new</span> Windows(<span style="color: #800000;">"</span><span style="color: #800000;">五子棋</span><span style="color: #800000;">"</span><span style="color: #000000;">);
    }
}</span></pre>
</div>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>总结：</h2>
<p>其实五子棋的小程序对于初学者来说并不简单。不适合做练手项目，不过当代码量积累到一定程度，写这个小程序简直不要太轻松。完成起来分分钟钟。一定要打好数据结构的基础并加大代码量。</p>
<p>&nbsp;</p>
