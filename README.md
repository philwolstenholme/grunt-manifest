# grunt-manifest [![Build Status](https://travis-ci.org/gunta/grunt-manifest.png)](http://travis-ci.org/gunta/grunt-manifest)
> Generate HTML5 Cache Manifest files. 


## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-manifest --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-manifest');
```



### HTML5 Cache Manifest task

_Run this task with the `grunt manifest` command._

Visit the [Appcache Facts](http://appcache.offline.technology/) for more information on Cache Manifest files.

Task targets, files and options may be specified according to the grunt [Configuring tasks](http://gruntjs.com/configuring-tasks) guide.



### Parameters

#### options 
Type: `Object`  
Default: `{}`


This controls how this task (and its helpers) operate and should contain key:value pairs, see options below.

#### src
Type: `String` `Array`  
Default: `undefined`   

Sets the input files.

#### dest
Type: `String`	
Default `manifest.appcache`

Sets the name of the Cache Manifest file.	
Remember that `.appcache` is now the W3C recommended file extension. 

### Options

#### basePath
Type: `String`	
Default: `undefined`	

Sets the base path for **input files**. **_It's recommended to set this_**.

#### cache
Type: `String`	
Default: `undefined`	

Adds manually a string to the **CACHE** section. Needed when you have cache buster for example.

#### process
Type: `Function`
Default: `undefined`

A function to process src files path strings before adding them on the appcache
manifest.

#### exclude
Type: `String` `Array`	
Default: `undefined`	

Exclude specific files from the Cache Manifest file.

#### network
Type: `String` `Array`	
Default: `"*"` (By default, an online whitelist wildcard flag is added)		

Adds a string to the **NETWORK** section.

See [here](http://diveintohtml5.info/offline.html#network) for more information.

#### fallback
Type: `String` `Array`	
Default: `undefined`	

Adds a string to the **FALLBACK** section.

See [here](http://diveintohtml5.info/offline.html#fallback) for more information.

#### preferOnline
Type: `Boolean`		
Default: `undefined`

Adds a string to the **SETTINGS** section, specifically the cache mode flag of the ```prefer-online``` state.

See [here](http://www.whatwg.org/specs/web-apps/current-work/multipage/offline.html#concept-appcache-mode-prefer-online) for more information.

#### verbose
Type: `Boolean`		
Default: `true`	

Adds a meta "copyright" comment.

#### timestamp
Type: `Boolean`		
Default: `true`	

Adds a timestamp as a comment for easy versioning.

Note: timestamp will invalidate application cache whenever cache manifest is rebuilt, even if contents of files in `src` have not changed.

#### hash
Type: `Boolean`
Default: `false`

Adds a sha256 hash of all `src` files (actual contents) as a comment.

This will ensure that application cache invalidates whenever actual file contents change (it's recommented to set `timestamp` to `false` when `hash` is used).

#### master
Type: `String` `Array`
Default: `undefined`

Hashes master html files (used with `hash`). Paths must be relative to the 'basePath'. This is useful when there are multiple html pages using one cache manifest and you don't want to explicitly include those pages in the manifest.

### Config Example

```js
// Project configuration
grunt.initConfig({
  manifest: {
    generate: {
      options: {
        basePath: '../',
        cache: ['js/app.js', 'css/style.css'],
        network: ['http://*', 'https://*'],
        fallback: ['/ /offline.html'],
        exclude: ['js/jquery.min.js'],
        preferOnline: true,
        verbose: true,
        timestamp: true,
        hash: true,
        master: ['index.html'],
        process: function(path) {
          return path.substring('build/'.length);
        }
      },
      src: [
      	'build/some_files/*.html',
    	  'build/js/*.min.js',
    	  'build/css/*.css'
      ],
      dest: 'manifest.appcache'
    }
  }
});
```

### Output example

```
CACHE MANIFEST
# This manifest was generated by grunt-manifest HTML5 Cache Manifest Generator
# Time: Mon Jan 01 2155 22:23:24 GMT+0900 (JST)

CACHE:
js/app.js
css/style
css/style.css
js/zepto.min.js
js/script.js
some_files/index.html
some_files/about.html

NETWORK:
*

# hash: 76f0ef591f999871e1dbdf6d5064d1276d80846feeef6b556f74ad87b44ca16a
```

You do need to be fully aware of standard browser caching.
If the files in **CACHE** are in the network cache, they won't actually update,
since the network cache will spit back the same file to the application cache.
Therefore, it's recommended to add a hash to the filenames's, akin to rails or yeoman. See [here](http://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/) why query strings are not recommended.



## Release History

* 2015/01/26 - v0.4.1 - Documented process for renaming files.
* 2012/10/23 - v0.4.0 - Changed package and repository name to grunt-manifest.
* 2012/10/23 - v0.3.0 - Upgraded to Grunt 0.4. Fixed dependencies. Fixed basePath.    
* 2012/10/23 - v0.2.1 - Added possibility to manually specify "CACHE:" files. Made comments optional.
* 2012/09/28 - v0.2.0 - Refactored from grunt-contrib into individual repo.


