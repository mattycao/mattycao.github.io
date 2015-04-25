<!doctype html><html><head><meta charset="utf-8">
<link rel="stylesheet" href="http://mdp.tylingsoft.com/bower_components/github-markdown-css/github-markdown.css"/>
<link rel="stylesheet" href="http://mdp.tylingsoft.com/bower_components/font-awesome-min/css/font-awesome.min.css"/>
<link rel="stylesheet" href="http://mdp.tylingsoft.com/bower_components/ionicons-min/css/ionicons.min.css"/>
<link rel="stylesheet" href="http://mdp.tylingsoft.com/bower_components/katex-build/katex.min.css"/>
<link rel="stylesheet" href="http://mdp.tylingsoft.com/markdown-plus.css"/>
<script src="http://mdp.tylingsoft.com/bower_components/jquery-dist/jquery.min.js"></script>
<script src="http://mdp.tylingsoft.com/bower_components/ace-min-noconflict/ace.js" charset="utf-8"></script>
<script src="http://mdp.tylingsoft.com/bower_components/ace-min-noconflict/ext-static_highlight.js"></script>
<script src="http://mdp.tylingsoft.com/bower_components/katex-build/katex.min.js"></script>
<script>$(function(){ $('img[src^="bower_components/emoji-icons/"]').each(function(){ $(this).attr('src', 'http://mdp.tylingsoft.com/' + $(this).attr('src')); }); });</script>
</head><body><article class="markdown-body" id="preview" data-open-title="Hide Preview" data-closed-title="Show Preview" style="padding-bottom: 848px;"><h2 id="chapter1-jquery" data-line="1">Chapter1 认识JQuery</h2>
<h3 id="jquery-js-" data-line="2">JQuery和JS的认识</h3>
<ol data-line="3">
<li>Javascript的弊端<ul data-line="2">
<li>复杂的DOM</li>
<li>不一致的浏览器实现和便捷的开发</li>
<li>调试工具的缺乏</li>
</ul>
</li>
<li>AJAX: asynchronous Javascript And XML</li>
<li>JQuery: 轻量级的库，拥有强的选择器，出色的DOM操作，可靠的时间处理，和完善的兼容性和链式操作等功能。</li>
<li>JQuery的优势：<ul data-line="2">
<li>轻量级 采用uglifyJS压缩</li>
<li>强大的选择器 支持CSS1到CSS3几乎所有的选择器， 以及JQuery自己的高级的选择器。</li>
<li>出色的DOM封装</li>
<li>可靠的事件处理机制，有graceful degradation, 循序渐进和非入侵式(unobtrusive)编程思想。</li>
<li>完善的AJAX</li>
<li>不污染顶级变量</li>
<li>出色的浏览器兼容性</li>
<li>链式操作</li>
<li>隐式迭代</li>
<li>行为层和结构层的分离</li>
<li>丰富的插件支持</li>
<li>完善的文档</li>
<li>开源</li>
</ul>
</li>
</ol>
<h3 id="jquery-" data-line="24">JQuery代码的编写</h3>
<ol data-line="25">
<li>JQuery分为生产版和开发版</li>
<li>在JQuery库中，$就是JQuery的一种简写形式</li>
<li>First Code:<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_lparen">(</span><span class="ace_variable ace_language">document</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">ready</span><span class="ace_paren ace_lparen">(</span><span class="ace_storage ace_type">function</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span> <span class="ace_paren ace_lparen">{</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_support ace_function">alert</span><span class="ace_paren ace_lparen">(</span><span class="ace_string">'JQuery'</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_paren ace_rparen">})</span><span class="ace_punctuation ace_operator">;</span>
</div></div></div></code></pre></li>
</ol>
<hr data-line="31">
<table data-line="33">
<thead>
<tr>
<th></th>
<th>window.onload</th>
<th>$(document).ready(function(){})</th>
</tr>
</thead>
<tbody>
<tr>
<td>执行时机</td>
<td>必须等待网页中所有的内容加载完毕后(包括图片)才能执行</td>
<td>网页中所有的DOM结构绘制完毕后就执行 可能DOM元素相关联的东西并没有加载完</td>
</tr>
<tr>
<td>编写个数</td>
<td>不能写多个</td>
<td>可以写多个</td>
</tr>
<tr>
<td>简化写法</td>
<td>无</td>
<td><code>$(function(){})</code></td>
</tr>
</tbody>
</table>
<h3 id="jquery-" data-line="39">JQuery的代码风格：</h3>
<ul data-line="40">
<li>对于同一个对象不超过三个操作，可以直接写成一行。</li>
<li>对于同一个对象的较多操作，建议每行写一个操作</li>
<li>对于多个对象的少量操作，可以每个对象写一行，如果涉及子元素，可以考虑适当的缩进。<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_lparen">(</span><span class="ace_variable ace_language">this</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">addClass</span><span class="ace_paren ace_lparen">(</span><span class="ace_string">'a'</span><span class="ace_paren ace_rparen">)</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span><span class="ace_indent-guide">    </span> <span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">children</span><span class="ace_paren ace_lparen">(</span><span class="ace_string">'li'</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">show</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">end</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">siblings</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">removeClass</span><span class="ace_paren ace_lparen">(</span><span class="ace_string">'high'</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div></div></div></code></pre></li>
</ul>
<h3 id="jquery-dom-" data-line="47">JQuery对象和DOM对象</h3>
<ol data-line="48">
<li>DOM对象
Document Object Model by using the document method.</li>
<li>JQuery对象
就是通过JQuery包装DOM对象后产生的对象。
如果他是一个JQuery对象，那么就可以使用JQuery里面的方法。
因此，在JQuery对象中，无法使用DOM对象的任何方法。</li>
</ol>
<h3 id="jquery-dom-" data-line="55">JQuery对象和DOM对象之间的相互转换</h3>
<ol data-line="56">
<li>首先， 我们先约定好获取的对象如果为JQuery对象，那么我们要在变量前面加上$。<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_storage ace_type">var</span> <span class="ace_identifier">$v</span> <span class="ace_keyword ace_operator">=</span> <span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_lparen">(</span><span class="ace_string">'#id'</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div></div></div></code></pre></li>
<li>JQuery对象转成DOM对象<ul data-line="2">
<li><code>[index]</code></li>
<li><code>get(index)</code><blockquote data-line="2">
<p data-line="undefined">JQuery对象是一个类似数组的对象，可以通过<code>index</code>的方法来获得相应的DOM对象：</p>
<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_storage ace_type">var</span> <span class="ace_identifier">c</span> <span class="ace_keyword ace_operator">=</span> <span class="ace_identifier">$v</span><span class="ace_paren ace_lparen">[</span><span class="ace_constant ace_numeric">0</span><span class="ace_paren ace_rparen">]</span><span class="ace_punctuation ace_operator">;</span> <span class="ace_comment">// the dom element</span>
</div></div></div></code></pre><p data-line="undefined">另一种方法是JQuery本身提供的，通过<code>get(index)</code>得到相应的DOM对象；</p>
<pre data-line="4"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_storage ace_type">var</span> <span class="ace_identifier">c</span> <span class="ace_keyword ace_operator">=</span> <span class="ace_identifier">$v</span><span class="ace_punctuation ace_operator">.</span><span class="ace_keyword">get</span><span class="ace_paren ace_lparen">(</span><span class="ace_constant ace_numeric">0</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div></div></div></code></pre></blockquote>
</li>
</ul>
</li>
<li>DOM对象转成JQuery对象<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_storage ace_type">var</span> <span class="ace_identifier">$v</span> <span class="ace_keyword ace_operator">=</span> <span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_lparen">(</span><span class="ace_identifier">c</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div></div></div></code></pre><blockquote data-line="3">
<p data-line="undefined">然后我们就可以随意使用JQuery所提供的相关方法了。</p>
</blockquote>
</li>
<li>例子</li>
<li>解决JQuery和其他库的冲突<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_identifier">JQuery</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">noConflict</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span> <span class="ace_comment">// <span class="ace_cjk" style="width:20px">释</span><span class="ace_cjk" style="width:20px">放</span>$<span class="ace_cjk" style="width:20px">的</span><span class="ace_cjk" style="width:20px">控</span><span class="ace_cjk" style="width:20px">制</span><span class="ace_cjk" style="width:20px">权</span><span class="ace_cjk" style="width:20px">给</span><span class="ace_cjk" style="width:20px">其</span><span class="ace_cjk" style="width:20px">他</span><span class="ace_cjk" style="width:20px">库</span><span class="ace_cjk" style="width:20px">，</span>JQuery<span class="ace_cjk" style="width:20px">仍</span><span class="ace_cjk" style="width:20px">然</span><span class="ace_cjk" style="width:20px">可</span><span class="ace_cjk" style="width:20px">以</span><span class="ace_cjk" style="width:20px">用</span>JQuery<span class="ace_cjk" style="width:20px">来</span><span class="ace_cjk" style="width:20px">使</span><span class="ace_cjk" style="width:20px">用</span>JQuery<span class="ace_cjk" style="width:20px">的</span><span class="ace_cjk" style="width:20px">东</span><span class="ace_cjk" style="width:20px">西</span><span class="ace_cjk" style="width:20px">。</span></span>
</div><div class="ace_line"> <span class="ace_comment">//<span class="ace_cjk" style="width:20px">此</span><span class="ace_cjk" style="width:20px">外</span><span class="ace_cjk" style="width:20px">，</span><span class="ace_cjk" style="width:20px">我</span><span class="ace_cjk" style="width:20px">们</span><span class="ace_cjk" style="width:20px">可</span><span class="ace_cjk" style="width:20px">以</span><span class="ace_cjk" style="width:20px">自</span><span class="ace_cjk" style="width:20px">定</span><span class="ace_cjk" style="width:20px">义</span><span class="ace_cjk" style="width:20px">一</span><span class="ace_cjk" style="width:20px">个</span><span class="ace_cjk" style="width:20px">备</span><span class="ace_cjk" style="width:20px">用</span><span class="ace_cjk" style="width:20px">名</span><span class="ace_cjk" style="width:20px">称</span><span class="ace_cjk" style="width:20px">来</span>access</span>
</div><div class="ace_line"> <span class="ace_storage ace_type">var</span> <span class="ace_identifier">$j</span> <span class="ace_keyword ace_operator">=</span> <span class="ace_identifier">JQuery</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">noConflict</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_comment">// it can be a, $a, awesome <span class="ace_cjk" style="width:20px">等</span></span>
</div></div></div></code></pre></li>
<li>如果不想给备用名，而同时使用$符号，我们可以：<pre data-line="2"><code><div class="ace-github"><div class="ace_static_highlight" style="counter-reset:ace_line 0"><div class="ace_line"><span class="ace_comment">//method 1</span>
</div><div class="ace_line"> <span class="ace_identifier">JQuery</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">noConflict</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_identifier">JQuery</span><span class="ace_paren ace_lparen">(</span><span class="ace_storage ace_type">function</span><span class="ace_paren ace_lparen">(</span><span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_rparen">)</span> <span class="ace_paren ace_lparen">{</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_comment">//here we can still using the $ as the JQuery</span>
</div><div class="ace_line"> <span class="ace_paren ace_rparen">})</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_comment">// outside the scope, the $ will be released</span>
</div><div class="ace_line"> <span class="ace_comment">//method 2</span>
</div><div class="ace_line"> <span class="ace_identifier">JQuery</span><span class="ace_punctuation ace_operator">.</span><span class="ace_identifier">noConflict</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_paren ace_lparen">(</span><span class="ace_storage ace_type">function</span><span class="ace_paren ace_lparen">(</span><span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_rparen">)</span> <span class="ace_paren ace_lparen">{</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_keyword ace_operator">$</span><span class="ace_paren ace_lparen">(</span><span class="ace_storage ace_type">function</span><span class="ace_paren ace_lparen">(</span><span class="ace_paren ace_rparen">)</span> <span class="ace_paren ace_lparen">{</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_comment">//here we can still using the $ as the JQuery</span>
</div><div class="ace_line"><span class="ace_indent-guide">    </span> <span class="ace_paren ace_rparen">})</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_paren ace_rparen">})</span><span class="ace_paren ace_lparen">(</span><span class="ace_identifier">JQuery</span><span class="ace_paren ace_rparen">)</span><span class="ace_punctuation ace_operator">;</span>
</div><div class="ace_line"> <span class="ace_comment">// outside the scope, the $ will be released</span>
</div></div></div></code></pre></li>
<li>JQuery库在其他库之前导入<br>如果JQuery库在其他库之前就导入，那么可以直接使用JQuery来做JQuery一些的工作，而无需调用<code>JQuery.noConflict()</code>.</li>
</ol>
</article></body></html>