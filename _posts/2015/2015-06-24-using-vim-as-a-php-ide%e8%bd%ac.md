---
layout: post
title: Using Vim as a PHP IDE[转]
date: 2015-06-24 11:12
author: admin
comments: true
categories: []
---
<article class="post">This isn't a blog about why you should use Vim. That's obvious. If it isn't, read <a href="http://www.viemu.com/a-why-vi-vim.html">Why do those nutheads use Vi?</a> Instead, this a tutorial about using Vim as a fully-fledged IDE, and specifically for PHP projects. Many Vim users would only use it for dipping in and out of the odd file on their computer, or maybe just on a remote server during an SSH session. I'm here to tell you that it can be configured to be more efficient and productive than your favourite IDE.
<ol>
	<li>Why bother?</li>
	<li>Starting out with Vim</li>
	<li>PHP syntax highlighting</li>
	<li>PHP syntax checking/linting</li>
	<li>Tags and auto-completion</li>
	<li>Codesniffer and mess detector support</li>
	<li>PHPUnit support</li>
	<li>Xdebug integration</li>
	<li>Everything in a neat package</li>
</ol>
If you're impatient and want to start tinkering straight away, jump to section 9 to install my Vim configuration.
<h2>1. Why bother?</h2>
I've used a number of IDEs for PHP projects over the past few years: Eclipse, Aptana, Komodo Edit/IDE, Netbeans and PHPStorm. The problem with many of them is speed and resource usage: they eat away at your memory and hog your CPU. Another problem is keyboard shortcuts and commands: it is impossible to do everything using the keyboard, and many things end up being quicker with the mouse, requiring you to constantly swap between keyboard and mouse. This may not sound like a big deal, but spend a few months learning Vim and it will be a big source of frustration.

So, the answer to that question is this: in the long run, you will be a lot more efficient. Jump to exactly the right place in a file in just a couple of key strokes; open a horizontal split window comparing two files with a command as simple as <code>&lt;Ctrl-w&gt; + s</code>; swap between open files by number (the order of opening); and my favourite - view and navigate a directory tree with all the usual Vim functionality.
<h2>2. Starting out with Vim</h2>
If you've never used Vim before, try<a href="http://www.viemu.com/a-why-vi-vim.html"> the tutorial</a> I mentioned at the top of this post. Also, take a look at the <a href="http://www.tuxfiles.org/linuxhelp/vimcheat.html">Vim cheat sheet</a>.

The rest of this tutorial assumes that you have a working knowledge of Vim and know how to configure it on at least a basic level (i.e. you know where your <em>vimrc</em> file is, and you know how to install vim scripts). It also assumes that you're running at least version 7.0 of Vim.

If you don't know how to install Vim scripts I'd recommend using <a href="https://github.com/gmarik/vundle">Vundle</a>, which will manage all your plugins for you and make installing/removing a breeze.
<h2>3. PHP syntax highlighting</h2>
Vim understands PHP by itself, but you can get improved highlighting and basic syntax checking with the <a href="http://www.vim.org/scripts/script.php?script_id=1571">php.vim</a> plugin. This will add better support for certain PHP keywords like <strong>define</strong> and <strong>static</strong>, etc., and will allow you to apply custom colours to these keywords. Also, it puts some basic checks in place for invalid syntax, and will highlight the line if you don't close your brackets, for instance.
<h2>4. PHP syntax checking/linting</h2>
Most IDEs will run the PHP parser on the file periodically, and warn you of any errors such as invalid syntax and multiple declarations of classes/functions within the same file. One of the beautiful things about Vim is that you can run files through command line programs with total ease, so to check your PHP file you could just run:
<div class="highlight">
<pre><code class="vim"><span class="p">:!</span>php <span class="p">-</span><span class="k">l</span> %
</code></pre>
</div>
This prints out any error message to the screen. However, we can do better. Vim has a feature called <em>signs</em>, which are used to mark and highlight lines in a file. Wouldn't it be nice if we could take the PHP parser's error message and highlight the right line in the file?

It would be nice. And it <em>is</em> nice, because it's been done.  I have a plugin that does it, called <a href="https://github.com/joonty/vim-phpqa">PHP QA tools</a> . Install this, and every time you write a file it will check it for syntax errors and highlight any offending lines in the file. It will also open a quickfix window that displays the error. This window is specially made for listing errors, and you can read about how to use it in <a href="http://vimdoc.sourceforge.net/htmldoc/quickfix.html">the documentation</a>.

<a href="http://joncairns.com/wp-content/uploads/2012/05/vim-php-lint.png"><img src="http://joncairns.com/wp-content/uploads/2012/05/vim-php-lint-273x300.png" alt="PHP lint error in Vim window" /></a><span class="caption">PHP syntax errors are highlighted and shown in the quickfix window</span>

The same plugin will be useful for some things that appear later, so read on...

<em>An alternative to my own plugin for syntax checking is <a href="https://github.com/scrooloose/syntastic">syntastic</a>. This has support for many languages, so I use it for everything except PHP.</em>
<h2>5. Tags and auto-completion</h2>
Code-completion is a feature that most large IDEs provide, along with the ability to jump to class and function definitions within multiple files. All IDEs do this by creating a database of the definitions, and updating this database as you change the code. Many people think that this is a reason not to change to using Vim, as the support isn't there; this isn't true.

Omnicompletion is a feature of Vim 7+, and it works by reading a tag file generated by a command-line program called <em>ctags</em>. This file contains definitions for all the classes, functions and variables in your project. You then use <code>&lt;c-x&gt;&lt;c-o&gt;</code> (read that as: <code>Ctrl+x</code> followed by <code>Ctrl+o</code> - that's Vim's way of showing a Ctrl modifier shortcut) to bring up a list of completions when you are in insert mode.
<h3>A quick note</h3>
Before we go on, I'd just like to say that I don't use omnicompletion. Instead, I use Vim's local keyword completion. There are several reasons for this:
<ol>
	<li>There's a delay as the context for omnicompletion is determined, and the tags are collected together. This is the case in every IDE I've used - it makes logical sense that it as a project gets bigger, the impact on performance of autocompletion gets greater.</li>
	<li>Local keyword completion is lightning fast, and in many cases saves me valuable seconds (or at least milliseconds!) in typing longer words. This is even more the case with the SuperTab plugin, which I'll mention soon.</li>
	<li>It only works for words defined in the current file, but it encourages me to be a better programmer by using consistent naming methods for functions and variables, for instance.</li>
</ol>
Even so, the tags files are extremely useful even if not using omnicompletion, as they are used for jumping to definitions quickly. I'll explain how to set-up the environment and start generating tag files.
<h3>How to generate Ctags</h3>
You'll want to use exuberant-ctags (<a href="http://ctags.sourceforge.net/">http://ctags.sourceforge.net/</a>), which has support for over 40 programming languages, including PHP, of course. Installation is a doddle on Ubuntu (<code>sudo apt-get install exuberant-ctags</code>), otherwise follow the installation instructions on the website for your OS. After installation you should be able to run ctags-exuberant from the command line. Go to the top-level directory of a project containing PHP files and run:
<div class="highlight">
<pre><code class="bash">ctags-exuberant -f php.tags --languages<span class="o">=</span>PHP -R
</code></pre>
</div>
This will create a file named php.tags, which contains a summary of all the definitions of PHP constructs in your project. You may need to tweak the parameters in this command: you may want to exclude certain files (e.g. build artefacts, documentation, etc.), or exclude whole languages like JavaScript from being parsed (<code>--languages=+PHP,-JavaScript</code>). When you're happy with your tags file, you need to tell Vim to use it:
<div class="highlight">
<pre><code class="vim"><span class="p">:</span><span class="k">set</span> <span class="k">tags</span><span class="p">=~</span><span class="sr">/path/</span><span class="k">to</span>/php.<span class="k">tags</span>
</code></pre>
</div>
You can use more than one tag file at a time: I generate a tag file for my entire PHP include library (i.e. PEAR stuff), a tag file for the framework I'm using and then a tag file for each project, and set all three to be used with a comma separated list.

Now that you've set up your tags, try it out. Open up a new PHP file, set the tag file and start typing the name of a class that you know exists. After a few characters type <code>&lt;c-x&gt;&lt;c-o&gt;</code> (insert mode) and the omnicompletion will kick in. Then, on the completed class name in command mode, type <code>&lt;c-]&gt;</code>. You will be taken to the class definition, or given a list of definitions if it matches more than one.

Another neat feature is jumping to a tag from the Vim command line. For example, if you know you need to get to the file containing the class <code>MyLovelyClass</code>, just type:
<div class="highlight">
<pre><code class="vim"><span class="p">:</span><span class="nb">tag</span> MyLovelyClass
</code></pre>
</div>
And hit return. You will be taken straight to it. This is almost always quicker than trying to navigate a file tree to find the file you want. Also, this supports tab completion, so you could just type <code>:tag My&lt;tab&gt;</code> and you would get a list of options.

If you're going to use omnicompletion, definitely install the <a href="http://www.vim.org/scripts/script.php?script_id=3171">phpcomplete plugin</a>, as this adds better context awareness for omnicompletion in PHP. I'd also recommend installing <a href="http://www.vim.org/scripts/script.php?script_id=1643">SuperTab</a>, and adding this to your <em>vimrc</em>:
<div class="highlight">
<pre><code class="vim"><span class="k">let</span> <span class="k">g</span>:SuperTabDefaultCompletionType <span class="p">=</span> <span class="s2">""</span>
</code></pre>
</div>
Now you just press <code>Tab</code> to trigger omnicompletion.
<h3>Automating tag generation</h3>
Without a plugin to manage your tags, you will have to manually update the tag files every time you make changes. As a result, I created a very simple tag manager plugin, <a href="https://github.com/joonty/vim-taggatron">taggatron</a>. This allows you to specify the languages for which you want to generate tags, and then it watches for changes and automatically and incrementally updates the tag files. You might like to give this a try if you plan on using tags heavily.
<h2>6. Codesniffer and mess detector support</h2>
If you're not familiar with these tools then you may want to skip this section, as you will only want this support if you already use them. Support for these comes bundled in the <a href="http://www.vim.org/scripts/script.php?script_id=3980">PHP QA tools</a> plugin (view on <a href="https://github.com/joonty/vim-phpqa">github</a>), which you will already have installed if you're following each step of this tutorial. The plugin parses their output, puts the violations into a quickfix window and puts line markers in the file (uses the marker <strong>S&gt; *<em>for codesniffer and *</em>M&gt;</strong> for mess detector violations). Nice and simple.

<a href="http://joncairns.com/wp-content/uploads/2012/05/vim-qatools.png"><img src="http://joncairns.com/wp-content/uploads/2012/05/vim-qatools-300x259.png" alt="PHP mess detector and codesniffer support in Vim" /></a><span class="caption">Codesniffer and mess detector signs are shown in the margin, and messages in the quickfix window</span>

Everyone who uses these programs has their own set of configuration rules, and happily this plugin allows you to change the configuration to suit. You can set various options in your <em>vimrc</em> file:
<h3>CODESNIFFER</h3>
<div class="highlight">
<pre><code class="vim"><span class="c">" Pass arguments to phpcs binary</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_codesniffer_args <span class="p">=</span> <span class="s2">"--standard=Zend"</span>
<span class="c">" Another example</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_codesniffer_args <span class="p">=</span> <span class="s2">"--standard=/path/to/xml/file.xml --tab-width=4"</span>

<span class="c">" PHP codesniffer binary (default = phpcs)</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_codesniffer_cmd<span class="p">=</span><span class="s1">'/path/to/phpcs'</span>

<span class="c">" Run codesniffer on save (default = 1)</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_codesniffer_autorun <span class="p">=</span> <span class="m">0</span>
</code></pre>
</div>
<h3>Mess detector</h3>
<div class="highlight">
<pre><code class="vim"><span class="k">let</span> <span class="k">g</span>:phpqa_messdetector_ruleset <span class="p">=</span> <span class="s2">"/path/to/phpmd.xml"</span>

<span class="c">" PHP mess detector binary (default = phpmd)</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_messdetector_cmd<span class="p">=</span><span class="s1">'/path/to/phpmd'</span>

<span class="c">" Run mess detector on save (default = 1)</span>
<span class="k">let</span> <span class="k">g</span>:phpqa_messdetector_autorun <span class="p">=</span> <span class="m">0</span>
</code></pre>
</div>
<h2>7. PHPUnit support</h2>
If you use PHPUnit then you can run tests from the Vim command window with another one of my plugins: <a href="http://www.vim.org/scripts/script.php?script_id=4054">PHPUnitQF</a> (view on <a href="https://github.com/joonty/vim-phpunitqf">github</a>). This parses the output from PHPUnit tests, and any failures or errors are put into a quickfix window, allowing you to easily jump to the tests that weren't successful.

To run the tests, just execute:
<div class="highlight">
<pre><code class="vim"><span class="p">:</span>Test
</code></pre>
</div>
Where is passed straight to the <strong>phpunit</strong> command. For example, if you would normally run <code>phpunit MyClassTest</code> from a command line, you run <code>:Test MyClassTest</code> from Vim. If you have a standard set of arguments that you pass to phpunit (e.g. a configuration file), you can set a variable in your <em>vimrc</em> file. Here are the available configuration options:
<div class="highlight">
<pre><code class="vim"><span class="c">" Arguments to pass to the phpunit binary</span>
<span class="k">let</span> <span class="k">g</span>:phpunit_args <span class="p">=</span> <span class="s2">"--configuration /path/to/config"</span>

<span class="c">" Location of phpunit</span>
<span class="k">let</span> <span class="k">g</span>:phpunit_cmd <span class="p">=</span> <span class="s2">"/usr/bin/mytest"</span>

<span class="c">" Temporary file used by the plugin</span>
<span class="c">" (shouldn't need to change unless runing on Windows)</span>
<span class="k">let</span> <span class="k">g</span>:phpunit_tmpfile <span class="p">=</span> <span class="s2">"/my/new/tmp/file"</span>
</code></pre>
</div>
You can use any test wrapper whose output is the same format as PHPUnit. For instance, I sometimes use the CakePHP framework which has a wrapper around the PHPUnit command, so I just change <code>g:phpunit_cmd</code> as shown above.

You can also view the full output of the last test run with:
<div class="highlight">
<pre><code class="vim"><span class="p">:</span>TestOutput
</code></pre>
</div>
This opens the output in a split window.
<h2>8. Xdebug integration</h2>
That's right. You can use Vim as a debugger client with Xdebug, so it throws the same punches as the big boy IDEs. In fact, I think that the Xdebug plugin for Vim allows more flexibility and functionality than any other implementation I've found. It's certainly easier to tweak and modify it. It's also great out of the box, and you can get it up and running within a few minutes.

<em>UPDATE: I no longer use this plugin, as I now actively maintain what I consider to be a far superior plugin, Vdebug. It has support for PHP, Python, Ruby and more, so check out my <a href="http://joncairns.com/2012/08/vdebug-a-dbgp-debugger-client-for-vim-supporting-php-python-perl-and-ruby/">blog post</a> on it or just go straight to the <a href="https://github.com/joonty/vdebug">Github repo</a>. For a description on setting up the older plugin, read on.</em>

This plugin has a bit of a legacy: it started <a href="http://www.vim.org/scripts/script.php?script_id=1152">here</a>, a script by Seung Woo Shin, and has been modified and forked like mad. It goes without saying that I have my own version of it, which you can get from the <a href="https://github.com/joonty/vim-xdebug">github repository</a>.

<em>Note: by all means use another version if you wish, but I have added quite a few features that I use all the time, so they might be useful to you too. Look at the README in the repository for a list of changes.</em>

After installing the plugin you will need to set up Xdebug on your machine. To do this, follow step 1 and (optionally) 4 and 5 on my <a href="http://joncairns.com/2012/01/set-up-xdebug-with-netbeans-and-lamp-and-cakephp/">Xdebug, LAMP and Netbeans tutorial</a>.

Now you can set breakpoints in your code with <code>F1</code> or:
<div class="highlight">
<pre><code class="vim"><span class="p">:</span>Bp
</code></pre>
</div>
Press <code>F5</code> to make Vim try and connect to the debugger (on port 9000 by default), where it waits for 30 seconds before timing out. During the 30 seconds, if you load the webpage or run the script then the session will start. One of my changes is to open the session in a new tab, to not disrupt the window configuration in your current tab, so you will see a new tab and a few windows open up. It will look something like this:

<a href="http://joncairns.com/wp-content/uploads/2012/05/vim-xdebug.png"><img src="http://joncairns.com/wp-content/uploads/2012/05/vim-xdebug-300x144.png" alt="Snapshot of a Xdebug session in Vim" /></a><span class="caption">Windows from left, going clockwise: source code pane, watch window, current stack trace, command window</span>

The left window shows the source code and typically the current execution point. The watch window is one of the key features, as it shows the result of getting the context (i.e. all the variables and their values at the current execution point) and <code>eval</code> results.

To start stepping through the program line-by-line, press <code>F3</code>. The <strong>-&gt;</strong> marker will move as the program moves on. Execution will pause at any breakpoints that you've set up. To step into a function on the current execution line press <code>F2</code>, and <code>F4</code> to step back out.

To get the current context, press <code>F11</code> - the watch window will show all variables and their values. You can also evaluate expressions, by typing:
<div class="highlight">
<pre><code class="vim"><span class="k">e</span>
</code></pre>
</div>
(The <code>Leader</code> key is usually \ or <code>,</code>) You can then type a PHP expression (e.g. a variable, or any valid expression like <code>sqrt(100)</code>), press enter, and the result will be printed in the watch window.

When you've finished, press <code>F6</code> to end the session and close all the windows.

That's enough to get you started, but you can read the README on the Github repository for more information.
<h2>9. Everything in a neat package</h2>
Maintaining a Vim configuration is a tricky business. Sometimes you make changes that you think will be helpful, find it annoying after a couple of weeks, and you want to revert back to an old version. That's exactly why I keep mine under version control - you can view it <a href="https://github.com/joonty/myvim">here</a>.

If you want to use this configuration, there are a couple of things you'll need to install. You'll need <em>exuberant-ctags</em>, as talked about in section 5. I'd also recommend installing the <a href="http://www.fontsquirrel.com/fonts/Anonymous-Pro">Anonymous Pro</a> font if you're using GVim, as it's the best code font I've come across.

To install, clone the repository to <em>~/.vim</em>, and initialize the submodule. Then, install the Vundle bundles:
<div class="highlight">
<pre><code class="bash">git clone git://github.com/joonty/myvim.git ~/.vim --recurse-submodules
vim +BundleInstall +qall
</code></pre>
</div>
What's in the package? Along with everything mentioned in this tutorial, it has the following plugins:
<ul>
	<li><a href="http://www.vim.org/scripts/script.php?script_id=1658">NERDTree</a>: shows the directory tree, and allows for navigation and modification. Completely essential in my eyes.</li>
	<li><a href="http://www.vim.org/scripts/script.php?script_id=2975">Fugitive</a>: the definitive Git plugin.</li>
	<li><a href="https://github.com/kien/ctrlp.vim">Ctrl-P</a>: super-fast file finding and opening (used to be <a href="https://github.com/wincent/Command-T">Command-T</a>).</li>
	<li><a href="http://www.vim.org/scripts/script.php?script_id=3526">EasyMotion</a>: quickly jump to any word or character in a file.</li>
	<li><a href="https://github.com/joonty/vim-sauce">Vim-Sauce</a>: a very simple project manager</li>
	<li><a href="https://github.com/scrooloose/syntastic">Syntastic</a>: syntax checking for multiple languages</li>
	<li><del><a href="http://www.vim.org/scripts/script.php?script_id=159">MiniBufExpl</a>: (Mini buffer explorer) shows the open buffers in a kind of tab format, allowing you to easily manage multiple files.</del> <em>I had <strong>serious</strong> performance issues with</em> <em>this plugin, so I no longer use it.</em></li>
	<li><del><a href="http://www.vim.org/scripts/script.php?script_id=273">Tag List</a>:  show definition summaries of classes, functions, variables, etc. for all open files.</del><em> I stopped using this plugin, as I found I could do everything with tags and Ctrl-P.</em></li>
</ul>
I swapped from Command-T to Ctrl-P, as Command-T requires you to build a library from source. Plus, Ctrl-P has some great additional features.

Here's a word of warning: this is <em>my</em> configuration, and it's likely to change on a fairly regular basis. I don't guarantee that it will work on your system, and I certainly don't guarantee that you'll like it. Feel free to chop and change it as you please.
<h2>The Final Word</h2>
Hopefully this has been helpful. If you have any suggestions for things I should add to this list then just add a comment.

May the Vim be with you.

</article>
