---
layout: post
title: group by order by 连用
date: 2015-09-29 16:28
author: admin
comments: true
categories: []
---
<pre class="lang-sql prettyprint prettyprinted"><code><span class="kwd">select</span><span class="pln"> l</span><span class="pun">.*</span> 
<span class="kwd">from</span> <span class="kwd">table</span><span class="pln"> l
</span><span class="kwd">inner</span> <span class="kwd">join</span> <span class="pun">(</span>
  <span class="kwd">select</span><span class="pln"> 
    m_id</span><span class="pun">,</span><span class="pln"> max</span><span class="pun">(</span><span class="pln">timestamp</span><span class="pun">)</span> <span class="kwd">as</span><span class="pln"> latest 
  </span><span class="kwd">from</span> <span class="kwd">table</span> 
  <span class="kwd">group</span> <span class="kwd">by</span><span class="pln"> m_id
</span><span class="pun">)</span><span class="pln"> r
  </span><span class="kwd">on</span><span class="pln"> l</span><span class="pun">.</span><span class="pln">timestamp </span><span class="pun">=</span><span class="pln"> r</span><span class="pun">.</span><span class="pln">latest </span><span class="kwd">and</span><span class="pln"> l</span><span class="pun">.</span><span class="pln">m_id </span><span class="pun">=</span><span class="pln"> r</span><span class="pun">.</span><span class="pln">m_id
</span><span class="kwd">order</span> <span class="kwd">by</span><span class="pln"> timestamp </span><span class="kwd">desc</span></code></pre>
