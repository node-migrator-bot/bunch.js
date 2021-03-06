# bunch.js

Bunch.js is a manifest-driven CSS and Javascript packaging utility. Here's what makes bunch.js awesome:

- It's a stand-alone program. No server-side code or framework required. 
- It packages AND minifies your JS and CSS files for you. 
- It's manifest driven. With one configuration file, he makes it easy to see what JS and CSS files are in use on your site, and what needs retired.
- It runs all your CSS through [LESS](http://lesscss.org/) for you.
- It lets you provide global variables to your CSS and JS.
- It's got a monitor mode that will automatically rebuild your bundles when they change. 

## Installation

You can install bunch.js from npm: 

	npm install bunch --global
	
Or if you wish to build from source, checkout the repository, and run:

	npm install --global

## Usage

To generate a new Bunch file for your project, run:

	bunch init

To pack your bundles, run:

	bunch pack

To pack your bundles and compress them, include the compress (x) flag when running pack:

	bunch pack --compress
	bunch pack -x

To start monitoring the source files for changes, run:

	bunch monitor
	
## The Bunchfile

As stated above, bunch.js is manifest driven. All your instructions to bunch.js should live in a Bunchfile. A Bunchfile is simply a JSON document with a few specific variables that allow you to control how bunch.js will bundle your code. 

	{
		"jsDir": "js", // This is a relative path to where your JS files live
		"cssDir": "css", // This is a relative path to where your CSS files live
		"sourceDir": "src", // This is the name of the dir inside jsDir and cssDir where your source files will live.
		"buildDir": "bin", // This is the name of the dir inside jsDir and cssDir where your built files will live.
		"bundles": { // List each bundle in here like so...
			"example_base.js": [
				"vendor/jquery.js",
				"app.js"
			],
			"example_base.css": [
				"reset.css",
				"app.css"
			]
		},
		"variables": { // You can specify variables here.
			"baseURL": "/staging/bunchShop/",
			"mainColor": "#385903",
			"bunches": ["Bananas", "Flowers", "Grapes", "Carrots", "Bradys"]
		}
	}
	
	
# Variables 

Occasionally you may want to define a variable at compile time for your CSS/JS. This is especially handy when dealing with URLs to external resource that may change between your staging and production environments. When you define variables in your Bunchfile, you can use strings, numbers, arrays or objects, however objects are not supported for CSS and will simply be left out of your CSS files. 

Using the above Bunchfile as our example, here is how you would access the variables in your CSS files: 

	#example {
		background-image: url( "@{baseURL}images/common/bg.gif" );
		color: @mainColor;
	}
	
We handle variables a bit differently for javascript. They're simply rendered as a global object at the at the top of each of your JS bundles. Here's how you would access them in your JS files: 

	$.getJSON( Bunch.variables["baseURL"] + "api/data.json", function( data ) {} );
	
	for( var i = 0; i < Bunch.variables["bunches"].length; i++ ) {
		console.log( Bunch.variables["bunches"][i] );
	}
	

# Upgrading an existing project using a Pickler file

Bunch used to be called pickle.js. If you're upgrading from pickle.js, simply rename your Pickler file to Bunchfile and you should be good to go. 


# Running the tests

Bunch.js is tested with [Vows](http://vowsjs.org/), and each path expects to be run in it's own process. This much change in the future, but for now it means passing the isolate parameter to vows when running the test suite. 

	vows test/test_bunch* -i


# License

Bunch.js is released under the [Modified BSD License](https://github.com/thebarbariangroup/bunch.js/blob/master/LICENSE).