---
layout: post
title: OPCache使用示例
date: 2015-07-31 13:24
author: admin
comments: true
categories: []
---
<a href="http://php.net/manual/zh/ref.opcache.php" target="_blank" rel="nofollow">http://php.net/manual/zh/ref.opcache.php</a>
如果开启了opcache,在不重启PHP的情况下要使修改生效,可以用opcache_reset()清空所有PHP进程的缓存或者用opcache_invalidate()删除指定脚本的缓存.

============================================

OPcache 通过将 PHP 脚本预编译的字节码存储到共享内存中来提升 PHP 的性能， 存储预编译字节码的好处就是 省去了每次加载和解析 PHP 脚本的开销。

OPcache如何开启？

参考手册：<a href="http://php.net/manual/zh/opcache.installation.php">http://php.net/manual/zh/opcache.installation.php</a>

OPcache相关设置？（本文主要针对windows上的配置）

<img src="http://images.cnitblog.com/blog/537027/201504/171355384956666.png" alt="" border="0" />

关于配置的说明和更多的配置项参考：<a href="http://php.net/manual/zh/opcache.configuration.php">http://php.net/manual/zh/opcache.configuration.php</a>

<strong>OPcache使用（OPcache函数）</strong>

1、 opcache_get_configuration ()   获取缓存的配置信息

opcache_get_status （）            获取缓存的状态信息
<div id="highlighter_101085" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div>
<div class="line number20 index19 alt1">20</div>
<div class="line number21 index20 alt2">21</div>
<div class="line number22 index21 alt1">22</div>
<div class="line number23 index22 alt2">23</div>
<div class="line number24 index23 alt1">24</div>
<div class="line number25 index24 alt2">25</div>
<div class="line number26 index25 alt1">26</div>
<div class="line number27 index26 alt2">27</div>
<div class="line number28 index27 alt1">28</div>
<div class="line number29 index28 alt2">29</div>
<div class="line number30 index29 alt1">30</div>
<div class="line number31 index30 alt2">31</div>
<div class="line number32 index31 alt1">32</div>
<div class="line number33 index32 alt2">33</div>
<div class="line number34 index33 alt1">34</div>
<div class="line number35 index34 alt2">35</div>
<div class="line number36 index35 alt1">36</div>
<div class="line number37 index36 alt2">37</div>
<div class="line number38 index37 alt1">38</div>
<div class="line number39 index38 alt2">39</div>
<div class="line number40 index39 alt1">40</div>
<div class="line number41 index40 alt2">41</div>
<div class="line number42 index41 alt1">42</div>
<div class="line number43 index42 alt2">43</div>
<div class="line number44 index43 alt1">44</div>
<div class="line number45 index44 alt2">45</div>
<div class="line number46 index45 alt1">46</div>
<div class="line number47 index46 alt2">47</div>
<div class="line number48 index47 alt1">48</div>
<div class="line number49 index48 alt2">49</div>
<div class="line number50 index49 alt1">50</div>
<div class="line number51 index50 alt2">51</div>
<div class="line number52 index51 alt1">52</div>
<div class="line number53 index52 alt2">53</div>
<div class="line number54 index53 alt1">54</div>
<div class="line number55 index54 alt2">55</div>
<div class="line number56 index55 alt1">56</div>
<div class="line number57 index56 alt2">57</div>
<div class="line number58 index57 alt1">58</div>
<div class="line number59 index58 alt2">59</div>
<div class="line number60 index59 alt1">60</div>
<div class="line number61 index60 alt2">61</div>
<div class="line number62 index61 alt1">62</div>
<div class="line number63 index62 alt2">63</div>
<div class="line number64 index63 alt1">64</div>
<div class="line number65 index64 alt2">65</div>
<div class="line number66 index65 alt1">66</div>
<div class="line number67 index66 alt2">67</div>
<div class="line number68 index67 alt1">68</div>
<div class="line number69 index68 alt2">69</div>
<div class="line number70 index69 alt1">70</div>
<div class="line number71 index70 alt2">71</div>
<div class="line number72 index71 alt1">72</div>
<div class="line number73 index72 alt2">73</div>
<div class="line number74 index73 alt1">74</div>
<div class="line number75 index74 alt2">75</div>
<div class="line number76 index75 alt1">76</div>
<div class="line number77 index76 alt2">77</div>
<div class="line number78 index77 alt1">78</div>
<div class="line number79 index78 alt2">79</div>
<div class="line number80 index79 alt1">80</div>
<div class="line number81 index80 alt2">81</div>
<div class="line number82 index81 alt1">82</div>
<div class="line number83 index82 alt2">83</div>
<div class="line number84 index83 alt1">84</div>
<div class="line number85 index84 alt2">85</div>
<div class="line number86 index85 alt1">86</div>
<div class="line number87 index86 alt2">87</div>
<div class="line number88 index87 alt1">88</div>
<div class="line number89 index88 alt2">89</div>
<div class="line number90 index89 alt1">90</div>
<div class="line number91 index90 alt2">91</div>
<div class="line number92 index91 alt1">92</div>
<div class="line number93 index92 alt2">93</div>
<div class="line number94 index93 alt1">94</div>
<div class="line number95 index94 alt2">95</div>
<div class="line number96 index95 alt1">96</div>
<div class="line number97 index96 alt2">97</div>
<div class="line number98 index97 alt1">98</div>
<div class="line number99 index98 alt2">99</div>
<div class="line number100 index99 alt1">100</div>
<div class="line number101 index100 alt2">101</div>
<div class="line number102 index101 alt1">102</div>
<div class="line number103 index102 alt2">103</div>
<div class="line number104 index103 alt1">104</div>
<div class="line number105 index104 alt2">105</div>
<div class="line number106 index105 alt1">106</div>
<div class="line number107 index106 alt2">107</div>
<div class="line number108 index107 alt1">108</div>
<div class="line number109 index108 alt2">109</div>
<div class="line number110 index109 alt1">110</div>
<div class="line number111 index110 alt2">111</div>
<div class="line number112 index111 alt1">112</div>
<div class="line number113 index112 alt2">113</div>
<div class="line number114 index113 alt1">114</div>
<div class="line number115 index114 alt2">115</div>
<div class="line number116 index115 alt1">116</div>
<div class="line number117 index116 alt2">117</div>
<div class="line number118 index117 alt1">118</div>
<div class="line number119 index118 alt2">119</div>
<div class="line number120 index119 alt1">120</div>
<div class="line number121 index120 alt2">121</div>
<div class="line number122 index121 alt1">122</div>
<div class="line number123 index122 alt2">123</div>
<div class="line number124 index123 alt1">124</div>
<div class="line number125 index124 alt2">125</div>
<div class="line number126 index125 alt1">126</div>
<div class="line number127 index126 alt2">127</div>
<div class="line number128 index127 alt1">128</div>
<div class="line number129 index128 alt2">129</div>
<div class="line number130 index129 alt1">130</div>
<div class="line number131 index130 alt2">131</div>
<div class="line number132 index131 alt1">132</div>
<div class="line number133 index132 alt2">133</div>
<div class="line number134 index133 alt1">134</div>
<div class="line number135 index134 alt2">135</div>
<div class="line number136 index135 alt1">136</div>
<div class="line number137 index136 alt2">137</div>
<div class="line number138 index137 alt1">138</div>
<div class="line number139 index138 alt2">139</div>
<div class="line number140 index139 alt1">140</div>
<div class="line number141 index140 alt2">141</div>
<div class="line number142 index141 alt1">142</div>
<div class="line number143 index142 alt2">143</div>
<div class="line number144 index143 alt1">144</div>
<div class="line number145 index144 alt2">145</div>
<div class="line number146 index145 alt1">146</div>
<div class="line number147 index146 alt2">147</div>
<div class="line number148 index147 alt1">148</div>
<div class="line number149 index148 alt2">149</div>
<div class="line number150 index149 alt1">150</div>
<div class="line number151 index150 alt2">151</div>
<div class="line number152 index151 alt1">152</div>
<div class="line number153 index152 alt2">153</div>
<div class="line number154 index153 alt1">154</div>
<div class="line number155 index154 alt2">155</div>
<div class="line number156 index155 alt1">156</div>
<div class="line number157 index156 alt2">157</div>
<div class="line number158 index157 alt1">158</div>
<div class="line number159 index158 alt2">159</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'&lt;pre&gt;'</code><code class="php plain">;</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">    </code><code class="php comments">//  获取缓存的配置信息</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">    </code><code class="php plain">var_dump(opcache_get_configuration());</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'&lt;hr/&gt;'</code><code class="php plain">;</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">    </code><code class="php comments">//  获取缓存的状态信息</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">    </code><code class="php plain">var_dump(opcache_get_status());</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'&lt;/pre&gt;'</code><code class="php plain">;</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php spaces">    </code><code class="php comments">/**</code></div>
<div class="line number11 index10 alt2"><code class="php spaces">     </code><code class="php comments">* opcache_get_configuration和opcache_get_status返回的都是一组有关缓存配置信息</code></div>
<div class="line number12 index11 alt1"><code class="php spaces">     </code><code class="php comments">* 类似于上面的输出</code></div>
<div class="line number13 index12 alt2"><code class="php comments">array(3) {</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">  </code><code class="php comments">["directives"]=&gt;</code></div>
<div class="line number15 index14 alt2"><code class="php spaces">  </code><code class="php comments">array(24) {</code></div>
<div class="line number16 index15 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.enable"]=&gt;</code></div>
<div class="line number17 index16 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number18 index17 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.enable_cli"]=&gt;</code></div>
<div class="line number19 index18 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number20 index19 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.use_cwd"]=&gt;</code></div>
<div class="line number21 index20 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number22 index21 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.validate_timestamps"]=&gt;</code></div>
<div class="line number23 index22 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number24 index23 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.inherited_hack"]=&gt;</code></div>
<div class="line number25 index24 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number26 index25 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.dups_fix"]=&gt;</code></div>
<div class="line number27 index26 alt2"><code class="php spaces">    </code><code class="php comments">bool(false)</code></div>
<div class="line number28 index27 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.revalidate_path"]=&gt;</code></div>
<div class="line number29 index28 alt2"><code class="php spaces">    </code><code class="php comments">bool(false)</code></div>
<div class="line number30 index29 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.log_verbosity_level"]=&gt;</code></div>
<div class="line number31 index30 alt2"><code class="php spaces">    </code><code class="php comments">int(1)</code></div>
<div class="line number32 index31 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.memory_consumption"]=&gt;</code></div>
<div class="line number33 index32 alt2"><code class="php spaces">    </code><code class="php comments">int(134217728)</code></div>
<div class="line number34 index33 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.max_accelerated_files"]=&gt;</code></div>
<div class="line number35 index34 alt2"><code class="php spaces">    </code><code class="php comments">int(4000)</code></div>
<div class="line number36 index35 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.max_wasted_percentage"]=&gt;</code></div>
<div class="line number37 index36 alt2"><code class="php spaces">    </code><code class="php comments">float(0.05)</code></div>
<div class="line number38 index37 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.consistency_checks"]=&gt;</code></div>
<div class="line number39 index38 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number40 index39 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.force_restart_timeout"]=&gt;</code></div>
<div class="line number41 index40 alt2"><code class="php spaces">    </code><code class="php comments">int(180)</code></div>
<div class="line number42 index41 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.revalidate_freq"]=&gt;</code></div>
<div class="line number43 index42 alt2"><code class="php spaces">    </code><code class="php comments">int(60)</code></div>
<div class="line number44 index43 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.preferred_memory_model"]=&gt;</code></div>
<div class="line number45 index44 alt2"><code class="php spaces">    </code><code class="php comments">string(0) ""</code></div>
<div class="line number46 index45 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.blacklist_filename"]=&gt;</code></div>
<div class="line number47 index46 alt2"><code class="php spaces">    </code><code class="php comments">string(0) ""</code></div>
<div class="line number48 index47 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.max_file_size"]=&gt;</code></div>
<div class="line number49 index48 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number50 index49 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.error_log"]=&gt;</code></div>
<div class="line number51 index50 alt2"><code class="php spaces">    </code><code class="php comments">string(0) ""</code></div>
<div class="line number52 index51 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.protect_memory"]=&gt;</code></div>
<div class="line number53 index52 alt2"><code class="php spaces">    </code><code class="php comments">bool(false)</code></div>
<div class="line number54 index53 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.save_comments"]=&gt;</code></div>
<div class="line number55 index54 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number56 index55 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.load_comments"]=&gt;</code></div>
<div class="line number57 index56 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number58 index57 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.fast_shutdown"]=&gt;</code></div>
<div class="line number59 index58 alt2"><code class="php spaces">    </code><code class="php comments">bool(true)</code></div>
<div class="line number60 index59 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.enable_file_override"]=&gt;</code></div>
<div class="line number61 index60 alt2"><code class="php spaces">    </code><code class="php comments">bool(false)</code></div>
<div class="line number62 index61 alt1"><code class="php spaces">    </code><code class="php comments">["opcache.optimization_level"]=&gt;</code></div>
<div class="line number63 index62 alt2"><code class="php spaces">    </code><code class="php comments">int(2147483647)</code></div>
<div class="line number64 index63 alt1"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number65 index64 alt2"><code class="php spaces">  </code><code class="php comments">["version"]=&gt;</code></div>
<div class="line number66 index65 alt1"><code class="php spaces">  </code><code class="php comments">array(2) {</code></div>
<div class="line number67 index66 alt2"><code class="php spaces">    </code><code class="php comments">["version"]=&gt;</code></div>
<div class="line number68 index67 alt1"><code class="php spaces">    </code><code class="php comments">string(5) "7.0.3"</code></div>
<div class="line number69 index68 alt2"><code class="php spaces">    </code><code class="php comments">["opcache_product_name"]=&gt;</code></div>
<div class="line number70 index69 alt1"><code class="php spaces">    </code><code class="php comments">string(12) "Zend OPcache"</code></div>
<div class="line number71 index70 alt2"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number72 index71 alt1"><code class="php spaces">  </code><code class="php comments">["blacklist"]=&gt;</code></div>
<div class="line number73 index72 alt2"><code class="php spaces">  </code><code class="php comments">array(0) {</code></div>
<div class="line number74 index73 alt1"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number75 index74 alt2"><code class="php comments">}</code></div>
<div class="line number76 index75 alt1"><code class="php comments">array(7) {</code></div>
<div class="line number77 index76 alt2"><code class="php spaces">  </code><code class="php comments">["opcache_enabled"]=&gt;</code></div>
<div class="line number78 index77 alt1"><code class="php spaces">  </code><code class="php comments">bool(true)</code></div>
<div class="line number79 index78 alt2"><code class="php spaces">  </code><code class="php comments">["cache_full"]=&gt;</code></div>
<div class="line number80 index79 alt1"><code class="php spaces">  </code><code class="php comments">bool(false)</code></div>
<div class="line number81 index80 alt2"><code class="php spaces">  </code><code class="php comments">["restart_pending"]=&gt;</code></div>
<div class="line number82 index81 alt1"><code class="php spaces">  </code><code class="php comments">bool(false)</code></div>
<div class="line number83 index82 alt2"><code class="php spaces">  </code><code class="php comments">["restart_in_progress"]=&gt;</code></div>
<div class="line number84 index83 alt1"><code class="php spaces">  </code><code class="php comments">bool(false)</code></div>
<div class="line number85 index84 alt2"><code class="php spaces">  </code><code class="php comments">["memory_usage"]=&gt;</code></div>
<div class="line number86 index85 alt1"><code class="php spaces">  </code><code class="php comments">array(4) {</code></div>
<div class="line number87 index86 alt2"><code class="php spaces">    </code><code class="php comments">["used_memory"]=&gt;</code></div>
<div class="line number88 index87 alt1"><code class="php spaces">    </code><code class="php comments">int(231368)</code></div>
<div class="line number89 index88 alt2"><code class="php spaces">    </code><code class="php comments">["free_memory"]=&gt;</code></div>
<div class="line number90 index89 alt1"><code class="php spaces">    </code><code class="php comments">int(133986360)</code></div>
<div class="line number91 index90 alt2"><code class="php spaces">    </code><code class="php comments">["wasted_memory"]=&gt;</code></div>
<div class="line number92 index91 alt1"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number93 index92 alt2"><code class="php spaces">    </code><code class="php comments">["current_wasted_percentage"]=&gt;</code></div>
<div class="line number94 index93 alt1"><code class="php spaces">    </code><code class="php comments">float(0)</code></div>
<div class="line number95 index94 alt2"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number96 index95 alt1"><code class="php spaces">  </code><code class="php comments">["opcache_statistics"]=&gt;</code></div>
<div class="line number97 index96 alt2"><code class="php spaces">  </code><code class="php comments">array(13) {</code></div>
<div class="line number98 index97 alt1"><code class="php spaces">    </code><code class="php comments">["num_cached_scripts"]=&gt;</code></div>
<div class="line number99 index98 alt2"><code class="php spaces">    </code><code class="php comments">int(2)</code></div>
<div class="line number100 index99 alt1"><code class="php spaces">    </code><code class="php comments">["num_cached_keys"]=&gt;</code></div>
<div class="line number101 index100 alt2"><code class="php spaces">    </code><code class="php comments">int(4)</code></div>
<div class="line number102 index101 alt1"><code class="php spaces">    </code><code class="php comments">["max_cached_keys"]=&gt;</code></div>
<div class="line number103 index102 alt2"><code class="php spaces">    </code><code class="php comments">int(7963)</code></div>
<div class="line number104 index103 alt1"><code class="php spaces">    </code><code class="php comments">["hits"]=&gt;</code></div>
<div class="line number105 index104 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number106 index105 alt1"><code class="php spaces">    </code><code class="php comments">["start_time"]=&gt;</code></div>
<div class="line number107 index106 alt2"><code class="php spaces">    </code><code class="php comments">int(1429153705)</code></div>
<div class="line number108 index107 alt1"><code class="php spaces">    </code><code class="php comments">["last_restart_time"]=&gt;</code></div>
<div class="line number109 index108 alt2"><code class="php spaces">    </code><code class="php comments">int(1429161635)</code></div>
<div class="line number110 index109 alt1"><code class="php spaces">    </code><code class="php comments">["oom_restarts"]=&gt;</code></div>
<div class="line number111 index110 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number112 index111 alt1"><code class="php spaces">    </code><code class="php comments">["hash_restarts"]=&gt;</code></div>
<div class="line number113 index112 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number114 index113 alt1"><code class="php spaces">    </code><code class="php comments">["manual_restarts"]=&gt;</code></div>
<div class="line number115 index114 alt2"><code class="php spaces">    </code><code class="php comments">int(81)</code></div>
<div class="line number116 index115 alt1"><code class="php spaces">    </code><code class="php comments">["misses"]=&gt;</code></div>
<div class="line number117 index116 alt2"><code class="php spaces">    </code><code class="php comments">int(2)</code></div>
<div class="line number118 index117 alt1"><code class="php spaces">    </code><code class="php comments">["blacklist_misses"]=&gt;</code></div>
<div class="line number119 index118 alt2"><code class="php spaces">    </code><code class="php comments">int(0)</code></div>
<div class="line number120 index119 alt1"><code class="php spaces">    </code><code class="php comments">["blacklist_miss_ratio"]=&gt;</code></div>
<div class="line number121 index120 alt2"><code class="php spaces">    </code><code class="php comments">float(0)</code></div>
<div class="line number122 index121 alt1"><code class="php spaces">    </code><code class="php comments">["opcache_hit_rate"]=&gt;</code></div>
<div class="line number123 index122 alt2"><code class="php spaces">    </code><code class="php comments">float(0)</code></div>
<div class="line number124 index123 alt1"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number125 index124 alt2"><code class="php spaces">  </code><code class="php comments">["scripts"]=&gt;</code></div>
<div class="line number126 index125 alt1"><code class="php spaces">  </code><code class="php comments">array(2) {</code></div>
<div class="line number127 index126 alt2"><code class="php spaces">    </code><code class="php comments">["D:\WWW\php-code\opcache\optest.php"]=&gt;</code></div>
<div class="line number128 index127 alt1"><code class="php spaces">    </code><code class="php comments">array(6) {</code></div>
<div class="line number129 index128 alt2"><code class="php spaces">      </code><code class="php comments">["full_path"]=&gt;</code></div>
<div class="line number130 index129 alt1"><code class="php spaces">      </code><code class="php comments">string(34) "D:\WWW\php-code\opcache\optest.php"</code></div>
<div class="line number131 index130 alt2"><code class="php spaces">      </code><code class="php comments">["hits"]=&gt;</code></div>
<div class="line number132 index131 alt1"><code class="php spaces">      </code><code class="php comments">int(0)</code></div>
<div class="line number133 index132 alt2"><code class="php spaces">      </code><code class="php comments">["memory_consumption"]=&gt;</code></div>
<div class="line number134 index133 alt1"><code class="php spaces">      </code><code class="php comments">int(4656)</code></div>
<div class="line number135 index134 alt2"><code class="php spaces">      </code><code class="php comments">["last_used"]=&gt;</code></div>
<div class="line number136 index135 alt1"><code class="php spaces">      </code><code class="php comments">string(24) "Thu Apr 16 13:20:35 2015"</code></div>
<div class="line number137 index136 alt2"><code class="php spaces">      </code><code class="php comments">["last_used_timestamp"]=&gt;</code></div>
<div class="line number138 index137 alt1"><code class="php spaces">      </code><code class="php comments">int(1429161635)</code></div>
<div class="line number139 index138 alt2"><code class="php spaces">      </code><code class="php comments">["timestamp"]=&gt;</code></div>
<div class="line number140 index139 alt1"><code class="php spaces">      </code><code class="php comments">int(1429161630)</code></div>
<div class="line number141 index140 alt2"><code class="php spaces">    </code><code class="php comments">}</code></div>
<div class="line number142 index141 alt1"><code class="php spaces">    </code><code class="php comments">["D:\WWW\php-code\opcache\opcache_config.php"]=&gt;</code></div>
<div class="line number143 index142 alt2"><code class="php spaces">    </code><code class="php comments">array(6) {</code></div>
<div class="line number144 index143 alt1"><code class="php spaces">      </code><code class="php comments">["full_path"]=&gt;</code></div>
<div class="line number145 index144 alt2"><code class="php spaces">      </code><code class="php comments">string(42) "D:\WWW\php-code\opcache\opcache_config.php"</code></div>
<div class="line number146 index145 alt1"><code class="php spaces">      </code><code class="php comments">["hits"]=&gt;</code></div>
<div class="line number147 index146 alt2"><code class="php spaces">      </code><code class="php comments">int(0)</code></div>
<div class="line number148 index147 alt1"><code class="php spaces">      </code><code class="php comments">["memory_consumption"]=&gt;</code></div>
<div class="line number149 index148 alt2"><code class="php spaces">      </code><code class="php comments">int(2080)</code></div>
<div class="line number150 index149 alt1"><code class="php spaces">      </code><code class="php comments">["last_used"]=&gt;</code></div>
<div class="line number151 index150 alt2"><code class="php spaces">      </code><code class="php comments">string(24) "Thu Apr 16 14:28:32 2015"</code></div>
<div class="line number152 index151 alt1"><code class="php spaces">      </code><code class="php comments">["last_used_timestamp"]=&gt;</code></div>
<div class="line number153 index152 alt2"><code class="php spaces">      </code><code class="php comments">int(1429165712)</code></div>
<div class="line number154 index153 alt1"><code class="php spaces">      </code><code class="php comments">["timestamp"]=&gt;</code></div>
<div class="line number155 index154 alt2"><code class="php spaces">      </code><code class="php comments">int(1429150438)</code></div>
<div class="line number156 index155 alt1"><code class="php spaces">    </code><code class="php comments">}</code></div>
<div class="line number157 index156 alt2"><code class="php spaces">  </code><code class="php comments">}</code></div>
<div class="line number158 index157 alt1"><code class="php comments">}</code></div>
<div class="line number159 index158 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
&nbsp;

<strong>2、下面我们将进行一些练习示例，注意所有的这一切都需要你确定你已经开启了php的OPcache扩展</strong>

<img src="http://images.cnitblog.com/blog/537027/201504/171355395738236.png" alt="" border="0" />去掉php.ini文件中的OPcache前面的分号

完成后重启Apache服务。

现在你将会发现：当我们像平时一样修改php代码以后刷新页面，页面并不会立即显示出我们的变化，好了来段代码说明：
<div id="highlighter_768927" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php variable">$stime</code> <code class="php plain">= microtime();</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">    </code><code class="php keyword">try</code><code class="php plain">{</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">            </code><code class="php comments">// 这里我们简单的写了一个PDO连接数据库的测试程序</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">            </code><code class="php variable">$dsn</code> <code class="php plain">= </code><code class="php string">"mysql:host=localhost;dbname=test"</code><code class="php plain">;</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">            </code><code class="php variable">$pdo</code> <code class="php plain">= </code><code class="php keyword">new</code> <code class="php plain">PDO(</code><code class="php variable">$dsn</code><code class="php plain">, </code><code class="php string">'root'</code><code class="php plain">, </code><code class="php string">'root'</code><code class="php plain">);</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">            </code><code class="php variable">$sql</code> <code class="php plain">= </code><code class="php string">"SELECT * FROM account"</code><code class="php plain">;</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">            </code><code class="php variable">$stmt</code> <code class="php plain">= </code><code class="php variable">$pdo</code><code class="php plain">-&gt;prepare(</code><code class="php variable">$sql</code><code class="php plain">);</code></div>
<div class="line number9 index8 alt2"><code class="php spaces">            </code><code class="php variable">$stmt</code><code class="php plain">-&gt;execute();</code></div>
<div class="line number10 index9 alt1"><code class="php spaces">            </code><code class="php comments">// 假设第一次我们只输出了有多少行</code></div>
<div class="line number11 index10 alt2"><code class="php spaces">            </code><code class="php functions">echo</code> <code class="php variable">$stmt</code><code class="php plain">-&gt;rowCount();</code></div>
<div class="line number12 index11 alt1"><code class="php spaces">            </code></div>
<div class="line number13 index12 alt2"><code class="php spaces">    </code><code class="php plain">}</code><code class="php keyword">catch</code><code class="php plain">(PDOExecption </code><code class="php variable">$e</code><code class="php plain">){</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">        </code><code class="php variable">$e</code><code class="php plain">-&gt;getMessage();</code></div>
<div class="line number15 index14 alt2"><code class="php spaces">    </code><code class="php plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
<img src="http://images.cnitblog.com/blog/537027/201504/171355401354664.png" alt="" border="0" />

这里显示有 6 条数据，

如果我们此时想打印这 6 条数据，我们像平时那样，简单修改一下代码
<div id="highlighter_2996" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php variable">$stime</code> <code class="php plain">= microtime();</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">    </code><code class="php keyword">try</code><code class="php plain">{</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">            </code><code class="php comments">// 这里我们简单的写了一个PDO连接数据库的测试程序</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">            </code><code class="php variable">$dsn</code> <code class="php plain">= </code><code class="php string">"mysql:host=localhost;dbname=test"</code><code class="php plain">;</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">            </code><code class="php variable">$pdo</code> <code class="php plain">= </code><code class="php keyword">new</code> <code class="php plain">PDO(</code><code class="php variable">$dsn</code><code class="php plain">, </code><code class="php string">'root'</code><code class="php plain">, </code><code class="php string">'root'</code><code class="php plain">);</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">            </code><code class="php variable">$sql</code> <code class="php plain">= </code><code class="php string">"SELECT * FROM account"</code><code class="php plain">;</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">            </code><code class="php variable">$stmt</code> <code class="php plain">= </code><code class="php variable">$pdo</code><code class="php plain">-&gt;prepare(</code><code class="php variable">$sql</code><code class="php plain">);</code></div>
<div class="line number9 index8 alt2"><code class="php spaces">            </code><code class="php variable">$stmt</code><code class="php plain">-&gt;execute();</code></div>
<div class="line number10 index9 alt1"><code class="php spaces">            </code><code class="php comments">// 假设第一次我们只输出了有多少行</code></div>
<div class="line number11 index10 alt2"><code class="php spaces">            </code><code class="php functions">echo</code> <code class="php variable">$stmt</code><code class="php plain">-&gt;rowCount();</code></div>
<div class="line number12 index11 alt1"><code class="php spaces">            </code><code class="php functions">echo</code> <code class="php string">'&lt;hr/&gt;'</code><code class="php plain">;</code></div>
<div class="line number13 index12 alt2"><code class="php spaces">            </code><code class="php comments">// 我们想要打印查询出的所有数据</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">            </code><code class="php plain">print_r(</code><code class="php variable">$stmt</code><code class="php plain">-&gt;fetchAll());</code></div>
<div class="line number15 index14 alt2"><code class="php spaces">    </code><code class="php plain">}</code><code class="php keyword">catch</code><code class="php plain">(PDOExecption </code><code class="php variable">$e</code><code class="php plain">){</code></div>
<div class="line number16 index15 alt1"><code class="php spaces">        </code><code class="php variable">$e</code><code class="php plain">-&gt;getMessage();</code></div>
<div class="line number17 index16 alt2"><code class="php spaces">    </code><code class="php plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
我们现在刷新页面发现，

<img src="http://images.cnitblog.com/blog/537027/201504/171355412141936.png" alt="" border="0" />

咦？ 怎么没有变化，难道是我们写错了吗？ 如果你确定自己的代码没有问题，那么问题来了？这是为什么？是Apache疯了还是php疯了，还是我自己疯了？

其实如果你对自己的代码有信心也有耐心，多刷新几次，就会发现，结果出来了

<img src="http://images.cnitblog.com/blog/537027/201504/171355425102505.png" alt="" border="0" />

好奇怪？有木有，其实不是这样了，由于我们上面修改php.ini文件已经开启OPcache，当我们第一次运行这个脚本的时候，php会检查-&gt;翻译-&gt;运行 我们写的这个脚本，和以前是一样的，但是我们开启了OPcache，这次php会将翻译好的php中间代码：字节码opcode加载到内存中，当再次修改我们的脚本运行时，其实运行的是内存中的php中间代码opcode，我们修改过的脚本并没有运行

那为什么过一会，又显示正确了呢？

还是我们上面 OPcache 的设置 <img src="http://images.cnitblog.com/blog/537027/201504/171355444174890.png" alt="" border="0" />

看看解释：
<div id="highlighter_654779" class="syntaxhighlighter  as3">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="as3 plain">opcache.revalidate_freq integer</code></div>
<div class="line number2 index1 alt1"><code class="as3 plain">检查脚本时间戳是否有更新的周期，以秒为单位。 设置为 </code><code class="as3 value">0</code> <code class="as3 plain">会导致针对每个请求， OPcache 都会检查脚本更新。</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
所以就是每60秒php会检查我们的脚本是否被修改过，如果修改过那就重新执行正常的那一套：检查-&gt;翻译-&gt;运行，然后把中间代码加载到内存中，

所以在上面 我们的修改不会跟往常一样刷新页面就行，需要等待一会。这样是不是对缓存很有感受啊：我们修改了文件，却跟没有修改一样 ，页面没有显示任何变化 0.0 ！

&nbsp;

<strong>3、opcache_reset（）   重置字节码缓存的内容</strong>

对于我们上面出现的这种情况，如果我们不想每次都等待60秒，我们就可以使用这个方法，这个方法的定义是：在调用 opcache_reset() 之后，所有的脚本将会重新载入并且在下次被点击的时候重新解析。

注意一旦调用这个方法，<strong>所有的脚本</strong>都会被重新解析，然后再次载入到内存，记住是所有脚本，如果我们只是针对某个脚本的话，我们可以考虑使用：opcache_invalidate — 废除脚本缓存 （下面再说这个）

下面我们新建两个文件：opecho.php和opreset.php，简单的几行代码做个示例
<div id="highlighter_673777" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opecho.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 02:44:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<code class="php functions">echo</code> <code class="php string">"这是第一次修改，这个脚本会和以前一样去解释执行，然后将翻译好的中间代码opcode载入到内存中，&lt;br&gt;当我们在一分钟之内再次修改文件输出是没有变化的"</code><code class="php plain">;</code>

<code class="php plain"> </code>

</div></td>
</tr>
</tbody>
</table>
</div>
然后假设我们此时，又想echo一些内容
<div id="highlighter_530811" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opecho.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 02:44:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"><code class="php functions">echo</code> <code class="php string">"这是第一次修改，这个脚本会和以前一样去解释执行，然后将翻译好的中间代码opcode载入到内存中，&lt;br&gt;当我们在一分钟之内再次修改文件输出是没有变化的"</code><code class="php plain">;</code></div>
<div class="line number10 index9 alt1"></div>
<div class="line number11 index10 alt2"><code class="php comments">// 假设此时我们想在输出一些内容</code></div>
<div class="line number12 index11 alt1"><code class="php functions">echo</code> <code class="php string">"&lt;hr/&gt;"</code><code class="php plain">;</code></div>
<div class="line number13 index12 alt2"><code class="php functions">echo</code> <code class="php string">"这是第二次输出"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
我们发现，修改代码后没有变化

<img src="http://images.cnitblog.com/blog/537027/201504/171355450103074.png" alt="" border="0" />

然后我们执行 opreset.php

opreset.php
<div id="highlighter_102790" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opreset.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 02:44:25</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<code class="php comments">// 当我们在调用这个文件中 opcache_reset() 方法之后，我们不用等待 60秒，此时 所有脚本会重新走一次php标准的流程，</code>

<code class="php comments">也就相当于我们修改后的脚本重新解释执行 然后将修改后的脚本的中间代码 字节码 opcode加载到内存中，我们就可以看到我们的修改效果了</code>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php plain">opcache_reset() ? </code><code class="php string">'成功重置缓存内容'</code> <code class="php plain">: </code><code class="php string">'重置失败'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
<img src="http://images.cnitblog.com/blog/537027/201504/171355460261132.png" alt="" border="0" />

刷新我们刚才的页面

<img src="http://images.cnitblog.com/blog/537027/201504/171355469323987.png" alt="" border="0" />

修改出现了，如果你觉得你在60内完不成这么多操作，无法出现按我说的这样的情景，可以把 opcache.revalidate_freq 这个参数设置大一些 比如 180 秒，这样的话 会每隔三分钟才去坚持一次文件是否更新过

&nbsp;

<strong>4、opcache_invalidate （）</strong>

现在我们再新建一个opinvalidate.php 文件，再新建一个opecho1.php文件，利用我们上面的opecho.php文件，这次我们只想重新解释执行opecho1.php中的修改，
<div id="highlighter_956049" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opecho1.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 10:44:20</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'我们将使用php中的opcache_invalidate（）方法，让这个文件的字节码opcode手动失效'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
此时我们执行文件页面

<img src="http://images.cnitblog.com/blog/537027/201504/171355477609100.png" alt="" border="0" />

此时我们同时修改opecho.php和opecho1.php这两个文件
<div id="highlighter_778051" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opecho.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 02:44:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"><code class="php functions">echo</code> <code class="php string">"这是第一次修改，这个脚本会和以前一样去解释执行，然后将翻译好的中间代码opcode载入到内存中，&lt;br&gt;当我们在一分钟之内再次修改文件输出是没有变化的"</code><code class="php plain">;</code></div>
<div class="line number10 index9 alt1"></div>
<div class="line number11 index10 alt2"><code class="php comments">// 假设此时我们想在输出一些内容</code></div>
<div class="line number12 index11 alt1"><code class="php functions">echo</code> <code class="php string">"&lt;hr/&gt;"</code><code class="php plain">;</code></div>
<div class="line number13 index12 alt2"><code class="php functions">echo</code> <code class="php string">"这是第二次输出"</code><code class="php plain">;</code></div>
<div class="line number14 index13 alt1"></div>
<div class="line number15 index14 alt2"><code class="php functions">echo</code> <code class="php string">'&lt;hr/&gt;修改了opecho1.php文件'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
<div id="highlighter_978066" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opecho1.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 10:44:20</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'我们将使用php中的opcache_invalidate（）方法，让这个文件的字节码opcode手动失效'</code><code class="php plain">;</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'&lt;hr/&gt;修改了opecho1.php文件'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
然后刷新页面，页面没有变化：

<img src="http://images.cnitblog.com/blog/537027/201504/171355484017972.png" alt="" border="0" />

<img src="http://images.cnitblog.com/blog/537027/201504/171355488396373.png" alt="" border="0" />

当我们执行opinvalidate.php
<div id="highlighter_960628" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">// opcache_invalidate() 方法中有两个参数：</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">    </code><code class="php comments">// args: 1、 缓存需要被作废对应的脚本路径</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">    </code><code class="php comments">//       2、 布尔型，true表示 强制作废， false或者缺省值，表示文件更新后才失效</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php plain">opcache_invalidate(</code><code class="php string">'./opecho1.php'</code><code class="php plain">, TRUE) ? </code><code class="php string">'操作成功'</code> <code class="php plain">: </code><code class="php string">'操作失败'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
<img src="http://images.cnitblog.com/blog/537027/201504/171355496355028.png" alt="" border="0" />

我们就会看到：刷新 opecho1.php 页面

<img src="http://images.cnitblog.com/blog/537027/201504/171355502297913.png" alt="" border="0" />

我们的修改出现了，再来看看opecho.php页面

<img src="http://images.cnitblog.com/blog/537027/201504/171355509796284.png" alt="" border="0" />

没有变化，明白？  我们运行opinvalidate.php页面时，opecho1.php 脚本在缓存中的opcode被废弃了，php会重新执行一次opecho1.php这个脚本，所以我们看到了 我们的修改

&nbsp;

<strong>5、opcache_compile_file（） 方法：无需运行，即可编译并缓存 PHP 脚本</strong>

我们新建两个脚本：opcompile.php和compileScript.php，我们在opcompile.php中执行OPcache_compile_file方法缓存compileScript.php脚本
<div id="highlighter_972190" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opcomplie.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 11:53:30</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php spaces">    </code><code class="php comments">// opcache_compile_file() 方法要求传入待缓存文件的路径，此时不需要运行这个带缓存文件，只需要执行这个方法即可在内存中缓存</code></div>
<div class="line number11 index10 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php plain">opcache_compile_file(</code><code class="php string">'./compileScript.php'</code><code class="php plain">) ? </code><code class="php string">'脚本成功缓存'</code> <code class="php plain">: </code><code class="php string">'脚本缓存失败'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
<div id="highlighter_524938" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: compileScript.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 12:44:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'这是脚本compileScript.php'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
现在执行 compile.php文件，让他去把compileScript.php脚本加入到缓存中

<img src="http://images.cnitblog.com/blog/537027/201504/171355515575941.png" alt="" border="0" />

然后我们修改：compileScript.php
<div id="highlighter_823677" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: compileScript.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 12:44:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'这是脚本compileScript.php'</code><code class="php plain">;</code></div>
<div class="line number10 index9 alt1"></div>
<div class="line number11 index10 alt2"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">'我们第一次修改compileScript.php文件'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
现在来在浏览器中看看这个脚本：

<img src="http://images.cnitblog.com/blog/537027/201504/171355521043841.png" alt="" border="0" />

没有变化，那是因为，这次运行的时候，执行的是我们上面执行compile.php文件时在内存中生成的第一次的compileScript.php的中间代码

&nbsp;

<strong>6、opcache_is_script_cached（） </strong>

这个方法看名字估计都能猜出他是什么意思了：检查一个脚本是否在OPcache缓存中

我们也来一个示例：
<div id="highlighter_750307" class="syntaxhighlighter  php">
<table class="noBorderTable" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php spaces">    </code><code class="php comments">/*</code></div>
<div class="line number3 index2 alt2"><code class="php spaces">     </code><code class="php comments">* filename: opisscriptcache.php</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">     </code><code class="php comments">* author  : wangxb</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">     </code><code class="php comments">* create  : 2015_04_17 13:14:16</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">     </code><code class="php comments">* update  : @update</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">     </code><code class="php comments">*/</code></div>
<div class="line number8 index7 alt1"></div>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php comments">//  我们就以前面已经在加载到内存中的opecho.php文件作为测试对象</code></div>
<div class="line number10 index9 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php plain">opcache_is_script_cached(</code><code class="php string">'./opecho.php'</code><code class="php plain">) ? </code><code class="php string">'opecho.php脚本中间代码已经缓存'</code> <code class="php plain">: </code><code class="php string">'opecho.php没有缓存中间代码opcode在内存中'</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
很不幸，出了一个很严重的错误，

<img src="http://images.cnitblog.com/blog/537027/201504/171355525733998.png" alt="" border="0" />

没有这个方法，难道是我们错了吗？

其实不是了，是因为这个方法要求：

<img src="http://images.cnitblog.com/blog/537027/201504/171355530899141.png" alt="" border="0" />

看看我们的PHPinfo

<img src="http://images.cnitblog.com/blog/537027/201504/171355536827325.png" alt="" border="0" />

所以用不了，其他的为什么都好着？ 其他的方法要求进步都是v7.0.0、v7.0.2  我们这个满足，所以其他方法都可以使用，既然这样，这个方法也就这么说说算了，很简单的一个方法，平时也就是测试使用

切记：最后如果你测试完了，还是应该关闭OPcache，这样才不至于被OPcache捣乱我们平时的开发工作

<strong>  结语</strong>

好了，关于php缓存 OPcache的使用就介绍到这里，php还有一个扩张APC也是针对缓存的，我们下一篇再做说明，只用缓存来说，这个面就很大了，不光是这里说的php的缓存，数据库数据缓存、页面缓存等等。所以目前打算先把php中间OPcache层面的缓存弄清楚

如果你不知道什么是opcode字节码、中间代码，给你推荐一篇博客：<a href="http://blog.linuxeye.com/361.html">http://blog.linuxeye.com/361.html</a>

OK、就先这样吧，这段时间被换工作的事情搞得有点烦，上周面试了一家公司，不是专门做开发的公司，估计是那种做做页面一两个人维护的简单网站或者写一些简单的微信开发什么的，其实我不是开不起这样的公司，这家公司给我还不错的待遇，可是我是过不了心里那一道坎：幻想能找到一个技术氛围好的开发公司，自己能够见识到更多、更深、更广。自己也能在技术上有更大的提高，可是心里又告诉自己，难道一辈子要当个程序员吗？应该为以后想想，不一定要技术成长，工资待遇可以就行了。可是自己还是过不了心里那颗技术的梦，还是放不下在技术道路上狂奔的想法。哎、纠结，只能写写博客缓解一下这么纠结的心情了，让自己静静

&nbsp;
