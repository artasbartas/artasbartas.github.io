---
layout: post
date: 2016-06-12
categories: tutorials presentations reveal.js
author_name : Artas Bartas
author_url : /author/artas
author_avatar: artas
show_avatar : true
read_time : 12
feature_image: feature-revealjs
show_related_posts: true
square_related: recommend-woods
---

Working on presentations can be a huge time sink, especially if you care about how your presentation looks like. Which is why the first time I came across the presentations by [David Khourshid](http://slides.com/davidkhourshid/deck-17#/), [Gregor Aisch](http://slides.com/drivenbydata/nicar16#/) or [Joan Leon](http://slides.com/joanleon/flexbox-webcat#/), I was flabbergasted. Not only did their decks look a-m-a-z-i-n-g and worked directly in the browser, but they also came with myriads of cool features like background videos, vertical slides, and interactive elements.

It turns out that the three guys I mentioned as well as thousands of other developers, educators and journalists dumped their Powerpoints and Keynotes for the open-source HTML presentation framework [reveal.js](http://lab.hakim.se/reveal-js/#/). Invest a few days into picking up some coding skills like them and soon you will be able to churn out next-level decks. Heck, you can even skip the coding part and go straight for slides.com, a nifty online service built on top of the reveal.js library.

In case you want to persevere and learn things by doing, I have put together this basic guide to help you master the essentials of working with the reveal.js. You would do yourself a huge favor by going through the Read Me manual put together by the framework author [Hakim El Hattab](https://github.com/hakimel), but that is not strictly necessary. 

## Getting started

Begin by installing [Node.js](https://howtonode.org/how-to-install-nodejs) and [Grunt](http://gruntjs.com/getting-started#installing-the-cli) on your computer. Then clone the reveal.js repository and install dependencies:

{% highlight bash %}
$ git clone https://github.com/hakimel/reveal.js.git
$ cd reveal.js
$ npm install
{% endhighlight %}

It's useful to know that reveal.js library takes up less than 5 MB of space, while node modules occupy a whopping 136 MB. If you are OK with manually refreshing the presentation and don't need more advanced functions, you skip the `npm install` altogether and instead use the Python local server - SimpleHTTPServer, which is preinstalled on OS X. 

The repository comes with a demo presentation that you can view by switching into the root folder and running:

{% highlight bash %}
$ grunt serve
{% endhighlight %}

If you are using SimpleHTTPServer, first run:

{% highlight bash %}
$ python -m SimpleHTTPServer 8001
{% endhighlight %}

Then open [http://localhost:8001](http://localhost:8001) in your browser. By default, reveal.js will display the contents of the `index.html` file. If you want to view a different presentation, simply append the name of the corresponding file to the base URL. 

Reveal.js assumes that your presentations will live in the same folder as its source code. If that is not the case, you will need to modify the original `Gruntfile.js` to let it know where to look for presentations and working files. Locate the `watch` task in the `Gruntfile.js`, the original configuration looks like this:

{% highlight grunt %}
html: {
	files: [ '/*.html']
},
markdown: {
	files: [ '/*.md']
},
{% endhighlight %}

Modify it by adding sub-folders to the original scope:

{% highlight grunt %}
html: {
	files: [ '../*.html', '../*/*.html']
},
markdown: {
	files: [ '../*.md', '../*/*.md']
},
{% endhighlight %}

Now you can store all your markdown files in the `md` folder or have an individual folder for each presentation. 

## Presentation structure

Let's take a look at the outline of the presentation structure: slides are enclosed by the `<section>` tags, helper libraries and presentation settings - by the `<script>` tags. You can also add vertical slides to your presentations by nesting them within a given section as shown below. 

```html
	<body>
			<div class="reveal">
				<!-- Any section element inside of this container is displayed as a slide -->
				<div class="slides">
					<section>
						<p>Slide #1</p>
					</section>
					<section>
						<section>
							<p>Nested slide #1	
						</section>
						<section>
							<p>Nested slide #2	
						</section>	
					</section>
					<section>
						<p>Slide #N</p>
					</section>
				</div>
			</div>
			<script src="lib/js/head.min.js"></script>
			<script src="js/reveal.js"></script>
			<script> 
				// Configuration settings
				Reveal.initialize({
					controls: true,
					progress: true,
					history: true,
					center: true,
					transition: 'slide', 
					dependencies: [
						{ src: 'lib/js/classList.js', condition: function() { return !document.body.classList; } },
						{ src: 'plugin/markdown/marked.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } },
						{ src: 'plugin/markdown/markdown.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } },
						{ src: 'plugin/highlight/highlight.js', async: true, callback: function() { hljs.initHighlightingOnLoad(); } },
						{ src: 'plugin/zoom-js/zoom.js', async: true },
						{ src: 'plugin/notes/notes.js', async: true }
					]
				});
			</script>
	</body>
```

You can see some default settings under the `Reveal.initialize` function, but you can add much more - things like slide numbers, timed auto slide, looping a presentation, etc. The [Read me](https://github.com/hakimel/reveal.js/#configuration) file lists all the options - read it. 

By default, reveal.js presentations are styled with the **Black** theme, but don't let that fool you - `reveal.js` also has [10 other themes](https://github.com/hakimel/reveal.js/#theming) to choose from. Typically, you would edit your *.html file load the desired theme from the css/theme folder:

{% highlight HTML %}
// Loading the Serif theme
<link rel="stylesheet" href="css/theme/serif.css" id="theme">
{% endhighlight %}

However, sometimes you want to keep your styling options open. For such situations, include the snippet below at the very end of your presentation. The hack allows you to switch between themes by clicking on a link in the slide. The only downside of the hack is that the theme is reset every time a browser is reloaded.

{% highlight HTML %}
<section id="themes">
	<h2>Themes</h2>
		<p>
		reveal.js comes with a few themes built in: <br>
		<!-- Hacks to swap themes after the page has loaded. Not flexible and only intended for the reveal.js demo deck. -->
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/black.css'); return false;">Black (default)</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/white.css'); return false;">White</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/league.css'); return false;">League</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/sky.css'); return false;">Sky</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/beige.css'); return false;">Beige</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/simple.css'); return false;">Simple</a> <br>
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/serif.css'); return false;">Serif</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/blood.css'); return false;">Blood</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/night.css'); return false;">Night</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/moon.css'); return false;">Moon</a> -
			<a href="#" onclick="document.getElementById('theme').setAttribute('href','css/theme/solarized.css'); return false;">Solarized</a>
		</p>
</section>
{% endhighlight %}

## Harnessing Markdown

The great thing about the reveal.js framework is its support for markdown: you can compose your presentation as inline markdown or reference an external file (for the knowledgeable ones, the framework implements [Github Flavored Markdow](https://help.github.com/enterprise/11.10.340/user/articles/github-flavored-markdown/)). However, if you plan to make your presentation available on Github Pages, stick to inline markdown.

To get started, first you need to configure the data attributes - this will ensure that your markdown content is parsed correctly. Here is how a section referencing an external markdown file looks like:

{% highlight HTML %}
<section 	data-markdown="\presentation.md" 
			data-separator="^\n\n\n"
			data-separator-vertical="^\n\n"
			data-separator-notes="^Notes:"
			data-attributes="\\\.slide:\\\s*?(\\\S.+?)$"
			data-element-attributes="{_\s*?([^}]+?)}">
{% endhighlight %}  

As you can see, reveal.js expects the attribute values to be passed as regex statements, so remember to escape any characters with special meanings. Here is a quick explanation of the attributes above:

- `data-markdown` specifies the path to your markdown source file. Leave it blank if you plan to compose inline markdown.
- `data-separator` specifies how slides are separated, *"^\n\n\n"* stands for three new lines.
- `data-separator-vertical` vertical slides are separated by two new lines.
- `data-separator-notes` presenter notes are preceded by the word "Notes:".
- `data-attributes` specifies slide attributes, expects a line that begins with " .slide:"
- `data-element-attributes` specifies fragment attributes, expects a line that contains " {_ }" 

For inline markdown, we skip the file reference and enclose the content in an additional script tag:

{% highlight HTML %}
<section 	data-markdown 
			data-separator="^\n\n\n"
			data-separator-vertical="^\n\n"
			data-separator-notes="^Notes:"
			data-attributes="\\\.slide:\\\s*?(\\\S.+?)$"
			data-element-attributes="{_\s*?([^}]+?)}">
	<script type="text/template">		
		... 
	</script>
</section>
{% endhighlight %} 

Now you can start writing your presentation in markdown. Just remember that you need to enclose slide and fragment attributes in an HTML comment tag or reveal.js will fail to parse them. Here is a sample presentation that follows the syntax we defined above:

{% highlight Markdown %}
## Slide 1
This is the *first* slide in the presentation
It includes **bold** words and [links](http://google.com)



## Slide 2
This slide is separated by three lines, it will appear as a new slide
Note: This will only appear in the speaker notes window.


## Slide 2.1
This slide is separated by two lines, it will appear as a vertical slide



<!-- .slide: data-background="#417203" -->
## Slide attributes
A slide with a green background



## Element attributes
A slide with colored fragment
- A blue item 1 <!-- {_class="fragment highlight-blue" data-fragment-index="2"} -->
- A red item 2 <!-- {_class="fragment highlight-red" data-fragment-index="1"} -->
{% endhighlight %}


## The Art of PDF

There will inevitably come a time when you want to export your presentation to a PDF. To do that, first append a `?print-pdf` tag to the URL and then hit "Print" remembering to set margins at zero and enable the background graphics.

If you are very precise about how you like your PDS, venture beyond the default option and add [DeckTape](https://github.com/astefanutti/decktape) to your arsenal of tools. DeckTape supports such advanced features as font embedding, preserving of hyperlinks, support for SVG graphics and, naturally, file compression.

To get started, follow the installation instructions in the [Github repo](https://github.com/astefanutti/decktape). Once done, change into the `decktape` directory and use the following command:

{% highlight HTTP %}
$ ./bin/phantomjs decktape.js [options] [command] <url> <filename>
{% endhighlight %}

By default, decktape uses the `automatic` command, which picks the plugin matching your presentation framework and produces a PDF file from the provided URL. If you'd rather capture the presentation as it unfolds one click at a time, use the `generic` command. Here is a more advanced example of using decktape to produce a PDF of selected slides in two different resolutions:

{% highlight HTTP %}
$ ./bin/phantomjs decktape.js --screenshots --screenshots-size=400x300 --screenshots-size=800x600 --slides 1,3,5 automatic http://example.com/slides/demo.html deck.pdf
{% endhighlight %}

## Advanced techniques

### Embedding the presentation

Now that you mastered the basics, let's add some mojo to your presentations by exploring a few cool tricks you can do. How about embedding a presentation on your website? Just use an iframe:

{% highlight HTML %}
<iframe 	width="640" 
			height="427" 
			marginheight="0" 
			marginwidth="0" 
			src="http://example.com/slides/demo.html">
  				Include a fallback text for unsupported browsers here.
</iframe>
{% endhighlight %}

While you do that, also remember to set the *embedded* flag to true in the Reveal.initialize settings. 

### Synchronized presentations (Multiplexing)

Say you want to score some points by allowing the audience to view the presentation on their phones and tablets. Multiplexing allows you to control the flow of the presentation by changing slides on individual devices in sync with the master presentation. 

Reveal.js uses websockets to make this magic happen, so you need to begin by generatic an access token on the public websocket server provided by reveal.js at [https://reveal-js-multiplex-ccjbegmaii.now.sh](https://reveal-js-multiplex-ccjbegmaii.now.sh). You should get something that looks like this:

{% highlight JSON %}
{"secret":"12345678901234567890","socketId":"12a34b56c78d90e1"} 
{% endhighlight %}

Now configure the settings in the _master_ presentation by adding dependencies and the websockets information:

{% highlight HTML %}
Reveal.initialize({
    // other options...

    multiplex: {
        secret: '12345678901234567890', 
        id: '12a34b56c78d90e1',
        url: 'https://reveal-js-multiplex-ccjbegmaii.now.sh'
    },

    // Don't forget to add the dependencies
    dependencies: [
        { src: '//cdn.socket.io/socket.io-1.3.5.js', async: true },
        { src: 'plugin/multiplex/master.js', async: true },

        // other dependencies...
    ]
});
{% endhighlight %}

Now do the same for the _client_ presentation:

{% highlight HTML %}
Reveal.initialize({
    // other options...

    multiplex: {
        secret: null,
        id: '12a34b56c78d90e1',
        url: 'https://reveal-js-multiplex-ccjbegmaii.now.sh'
    },

    // Don't forget to add the dependencies
    dependencies: [
        { src: '//cdn.socket.io/socket.io-1.3.5.js', async: true },
        { src: 'plugin/multiplex/client.js', async: true }

        // other dependencies...
    ]
});
{% endhighlight %}

The `secret` variable enables control over your presentation, so you want to make sure it is included only in the master presentation and does not get accidentally deployed to the Github pages. 

Ideally, you want to run your master presentation on a localhost, while your client presentation is hosted at a publicly accessible URL. In case it does not work as expected, check the Console tools to make sure the websocket connection is successfully established.

### Interactive forms

Reveal.js allows you to execute some simple javascript code, which means that you can embed a post-presentation survey form or take some input from the audience during the talk and show what happens with that input. Simply insert the snippet of code between the `<section>` tags and you are good to go. Here is an example of a Wufoo survey embedded as an iframe:

{% highlight HTML %}
<section>
	<iframe height="377" 
			allowTransparency="true" 
			frameborder="0" 
			scrolling="no" 
			style="width:100%;border:none"  
			src="https://abartas.wufoo.com/embed/zj8m58u1xartnq/">
		<a href="https://abartas.wufoo.com/forms/zj8m58u1xartnq/">Fill out my Wufoo form!</a>
	</iframe>
	<div 	id="wuf-adv" 
			style="font-family:inherit;font-size: small;color:#a7a7a7;text-align:center;display:block;">
		<span class="notranslate">HTML Forms powered by <a href="http://www.wufoo.com">Wufoo</a>.
		</span>
	</div>
</section>
{% endhighlight %}

If you read all the way to this point, then nothing can stop you. I hope you will create only great presentations from now on. 