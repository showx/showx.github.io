---
layout: post
title: desarrolla2/cache缓存的使用
date: 2017-09-20 14:57
author: admin
comments: true
categories: [Uncategorized]
---
<h2>Installation</h2>
<h3><a id="user-content-with-composer" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-with-composer" rel="nofollow noopener external"></a>With Composer</h3>
It is best installed it through <a href="http://packagist.org/packages/desarrolla2/cache" rel="nofollow noopener external">packagist</a> by including <code>desarrolla2/cache</code> in your project composer.json require:
<pre>    <span class="pl-s"><span class="pl-pds">"</span>require<span class="pl-pds">"</span></span>: {
        <span class="pl-ii">//</span> <span class="pl-ii">...</span>
        <span class="pl-s"><span class="pl-pds">"</span>desarrolla2/cache<span class="pl-pds">"</span></span>:  <span class="pl-s"><span class="pl-pds">"</span>~2.0<span class="pl-pds">"</span></span>
    }</pre>
<h3><a id="user-content-without-composer" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-without-composer" rel="nofollow noopener external"></a>Without Composer</h3>
You can also download it from [Github] (<a href="https://github.com/desarrolla2/Cache" rel="nofollow noopener external">https://github.com/desarrolla2/Cache</a>), but no autoloader is provided so you'll need to register it with your own PSR-0 compatible autoloader.
<h2><a id="user-content-usage" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-usage" rel="nofollow noopener external"></a>Usage</h2>
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\NotCache</span>;</span>

<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-k">new</span> <span class="pl-c1">NotCache</span>());</span>

<span class="pl-s1"><span class="pl-smi">$cache</span><span class="pl-k">-&gt;</span>set(<span class="pl-s"><span class="pl-pds">'</span>key<span class="pl-pds">'</span></span>, <span class="pl-s"><span class="pl-pds">'</span>myKeyValue<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>

<span class="pl-s1"><span class="pl-c">// later ...</span></span>

<span class="pl-s1"><span class="pl-c1">echo</span> <span class="pl-smi">$cache</span><span class="pl-k">-&gt;</span>get(<span class="pl-s"><span class="pl-pds">'</span>key<span class="pl-pds">'</span></span>);</span>
</pre>
<h2><a id="user-content-adapters" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-adapters" rel="nofollow noopener external"></a>Adapters</h2>
<h3><a id="user-content-apcu" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-apcu" rel="nofollow noopener external"></a>Apcu</h3>
Use it if you will you have APC cache available in your system.
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Apcu</span>;</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Apcu</span>();</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<h3><a id="user-content-file" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-file" rel="nofollow noopener external"></a>File</h3>
Use it if you will you have dont have other cache system available in your system or if you like to do your code more portable.
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\File</span>;</span>

<span class="pl-s1"><span class="pl-smi">$cacheDir</span> <span class="pl-k">=</span> <span class="pl-s"><span class="pl-pds">'</span>/tmp<span class="pl-pds">'</span></span>;</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">File</span>(<span class="pl-smi">$cacheDir</span>);</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<h3><a id="user-content-memcache" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-memcache" rel="nofollow noopener external"></a>Memcache</h3>
Use it if you will you have mencache available in your system.
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Memcache</span>;</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Memcache</span>();</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
You can config your connection before
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Memcache</span>;    </span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">\Memcache</span> <span class="pl-k">as</span> <span class="pl-c1">Backend</span></span>

<span class="pl-s1"><span class="pl-smi">$backend</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Backend</span>();</span>
<span class="pl-s1"><span class="pl-c">// configure it here</span></span>

<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-k">new</span> <span class="pl-c1">Memcache</span>(<span class="pl-smi">$backend</span>));</span></pre>
<h3><a id="user-content-memcached" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-memcached" rel="nofollow noopener external"></a>Memcached</h3>
Is the same like mencache adapter.
<h3><a id="user-content-memory" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-memory" rel="nofollow noopener external"></a>Memory</h3>
This is the fastest cache type, since the elements are stored in memory. Cache Memory such is very volatile and is removed when the process terminates. Also it is not shared between different processes.

Memory cache have a option "limit", that limit the max items in cache.
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Memory</span>;</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Memory</span>();</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>limit<span class="pl-pds">'</span></span>, <span class="pl-c1">200</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<h3><a id="user-content-mongo" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-mongo" rel="nofollow noopener external"></a>Mongo</h3>
Use it to store the cache in a Mongo database. Requires the <a href="http://php.net/mongo" rel="nofollow noopener external">(legacy) mongo extension</a> or the <a href="https://github.com/mongodb/mongo-php-library" rel="nofollow noopener external">mongodb/mongodb</a> library.

You may pass either a database or collection object to the constructor. If a database object is passed, the <code>items</code> collection within that DB is used.
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Mongo</span>;</span>

<span class="pl-s1"><span class="pl-smi">$client</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">MongoClient</span>(<span class="pl-smi">$dsn</span>);</span>
<span class="pl-s1"><span class="pl-smi">$database</span> <span class="pl-k">=</span> <span class="pl-smi">$client</span><span class="pl-k">-&gt;</span>selectDatabase(<span class="pl-smi">$dbname</span>);</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Mongo</span>(<span class="pl-smi">$database</span>);</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Mongo</span>;</span>

<span class="pl-s1"><span class="pl-smi">$client</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">MongoClient</span>(<span class="pl-smi">$dsn</span>);</span>
<span class="pl-s1"><span class="pl-smi">$database</span> <span class="pl-k">=</span> <span class="pl-smi">$client</span><span class="pl-k">-&gt;</span>selectDatabase(<span class="pl-smi">$dbName</span>);</span>
<span class="pl-s1"><span class="pl-smi">$collection</span> <span class="pl-k">=</span> <span class="pl-smi">$database</span><span class="pl-k">-&gt;</span>selectCollection(<span class="pl-smi">$collectionName</span>);</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Mongo</span>(<span class="pl-smi">$collection</span>);</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<em>Note that expired cache items aren't automatically deleted. To keep your database clean, you should create a <a href="https://docs.mongodb.org/manual/core/index-ttl/" rel="nofollow noopener external">ttl index</a>.</em>
<pre><code>db.items.createIndex( { "ttl": 1 }, { expireAfterSeconds: 30 } )
</code></pre>
<h3><a id="user-content-mysqli" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-mysqli" rel="nofollow noopener external"></a>Mysqli</h3>
Use it if you will you have mysqlnd available in your system.
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Mysqli</span>;</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Mysqli</span>();</span>
<span class="pl-s1"><span class="pl-smi">$adapter</span><span class="pl-k">-&gt;</span>setOption(<span class="pl-s"><span class="pl-pds">'</span>ttl<span class="pl-pds">'</span></span>, <span class="pl-c1">3600</span>);</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<pre><span class="pl-pse">&lt;?php</span>
    
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Mysqli</span>;    </span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">\mysqli</span> <span class="pl-k">as</span> <span class="pl-c1">Backend</span></span>

<span class="pl-s1"><span class="pl-smi">$backend</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Backend</span>();</span>
<span class="pl-s1"><span class="pl-c">// configure it here</span></span>

<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-k">new</span> <span class="pl-c1">Mysqli</span>(<span class="pl-smi">$backend</span>));</span></pre>
<h3><a id="user-content-notcache" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-notcache" rel="nofollow noopener external"></a>NotCache</h3>
Use it if you will not implement any cache adapter is an adapter that will serve to fool the test environments.
<h3><a id="user-content-predis" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-predis" rel="nofollow noopener external"></a>Predis</h3>
Use it if you will you have redis available in your system.

You need to add predis as dependency in your composer file.
<pre><span class="pl-s"><span class="pl-pds">"</span>require<span class="pl-pds">"</span></span>: {
    <span class="pl-ii">//...</span>
    <span class="pl-s"><span class="pl-pds">"</span>predis/predis<span class="pl-pds">"</span></span>: <span class="pl-s"><span class="pl-pds">"</span>~1.0.0<span class="pl-pds">"</span></span>
}</pre>
other version will have compatibility issues.
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Predis</span>;</span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Predis</span>();</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
If you need to configure your predis client, you will instantiate it and pass it to constructor.
<pre><span class="pl-pse">&lt;?php</span>

<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Cache</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Desarrolla2\Cache\Adapter\Predis</span>;</span>
<span class="pl-s1"><span class="pl-k">use</span> <span class="pl-c1">Predis\Client</span> <span class="pl-k">as</span> <span class="pl-c1">Backend</span></span>

<span class="pl-s1"><span class="pl-smi">$adapter</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Predis</span>(<span class="pl-k">new</span> <span class="pl-c1">Backend</span>(<span class="pl-smi">$options</span>));</span>
<span class="pl-s1"><span class="pl-smi">$cache</span> <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-c1">Cache</span>(<span class="pl-smi">$adapter</span>);</span>
</pre>
<h2><a id="user-content-methods" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-methods" rel="nofollow noopener external"></a>Methods</h2>
A <code>Desarrolla2\Cache\Cache</code> object has the following methods:
<h5><a id="user-content-deletestring-key" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-deletestring-key" rel="nofollow noopener external"></a><code>delete(string $key)</code></h5>
Delete a value from the cache
<h5><a id="user-content-getstring-key" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-getstring-key" rel="nofollow noopener external"></a><code>get(string $key)</code></h5>
Retrieve the value corresponding to a provided key
<h5><a id="user-content-hasstring-key" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-hasstring-key" rel="nofollow noopener external"></a><code>has(string $key)</code></h5>
Retrieve the if value corresponding to a provided key exist
<h5><a id="user-content-setstring-key--mixed-value--int-ttl" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-setstring-key--mixed-value--int-ttl" rel="nofollow noopener external"></a><code>set(string $key , mixed $value [, int $ttl])</code></h5>
Add a value to the cache under a unique key
<h5><a id="user-content-setoptionstring-key-string-value" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-setoptionstring-key-string-value" rel="nofollow noopener external"></a><code>setOption(string $key, string $value)</code></h5>
Set option for Adapter
<h5><a id="user-content-clearcache" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-clearcache" rel="nofollow noopener external"></a><code>clearCache()</code></h5>
Clean all expired records from cache
<h5><a id="user-content-dropcache" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-dropcache" rel="nofollow noopener external"></a><code>dropCache()</code></h5>
Clear all cache
<h2><a id="user-content-coming-soon" class="anchor" href="https://packagist.org/packages/desarrolla2/cache#user-content-coming-soon" rel="nofollow noopener external"></a>Coming soon</h2>
This library implements other adapters as soon as possible, feel free to send new adapters if you think it appropriate.

This can be a list of pending tasks.
<ul>
 	<li>Cleaning cache</li>
 	<li>MemcachedAdapter</li>
 	<li>Other Adapters</li>
</ul>
