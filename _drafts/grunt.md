# What the heck is Gruntjs?
Grunt is a “task runner” written on top of [nodejs](https://nodejs.org/en/).

Wait. What the heck is a task runner? A task runner is a script (a file) that runs one or many tasks, where a task is some unit of work.  The tasks that are generally run by Grunt (and other task runners like Gulp or Brocolli) revolve around local development, testing, and deployment.

## Setup
In order to setup grunt, you must download grunt-cli via `npm install -g grunt-cli` and then `npm install —save-dev grunt` in the directory where you will be writing your grunt tasks. You must also create an empty `Gruntfile.js` file which contains all the grunt tasks.

So, in your directory, you should have a `package.json` file and a `Gruntfile.js` file.


## First Function
Let’s start by writing a completely random Grunt task to get a basic idea of its API. First, we need to set up the Gruntfile.js.

```js
module.exports = function(grunt) {
	// your code here.
};
```
The grunt object is what you will use to set up, load, and create grunt tasks. For our first simple task, add the following within the `module.exports` function:

```js
grunt.registerTask(‘helloworld’, ‘A basic task’,  function () {
	grunt.log.writeln('hello world');
	grunt.log.error('I\'m an error');
	grunt.verbose.ok('I\'m an ok message only in --verbose mode');
	grunt.log.debug('I\'m a debug message');
});
```
Run with grunt `helloworld --debug  —verbose`

`grunt.registerTask` allows you to create tasks within the Gruntfile, while `grunt.log` and `grunt.verbose` provide log messaging for the user.

Of course, the above task isn’t that useful. However, many useful tasks have already been made and exist in the npm registry.

One of the simplest tasks is `grunt-contrib-copy`, a task that provides the ability to copy files from one place to another. In order to use this task, run `npm install --save-dev grunt-contrib-copy` to install the grunt plugin.

To use the grunt plugin, you must load the task. The normal way of loading tasks is to do the following within the Gruntfile.js:

```js
module.exports = function(grunt) {
	grunt.loadNpmTasks('grunt-contrib-copy')
};
```

However, this method is slightly cumbersome because every time you want to use a new task, you have to install it via npm and then once again add the `grunt.loadNpmTasks` line. Instead, you can install a very useful package `load-grunt-tasks` using `npm install --save-dev load-grunt-tasks`. With this package, you simply need to do the following:

```js
module.exports = function(grunt) {
  	require('load-grunt-tasks')(grunt);
};
```

`require('load-grunt-tasks')(grunt);` will just load all tasks within package.json. See the [github repository](https://github.com/sindresorhus/load-grunt-tasks) for more details.


Now that we’ve "loaded" the task, we need to actually use it. Right now, we're not telling the copy task what to copy. The way you do that is through `grunt.initConfig` .

```js
grunt.initConfig({
  copy: {
      dev: {
        files: [
          {expand: true, src: ['**/*.js'], cwd: 'src/js', dest: '<%=pkg.bin %>/dev'},
          {expand: true, src: ['**/*.js'], cwd: 'node_modules/courage/src/js', dest: '<%=pkg.bin %>/dev'},
          {expand: true, src: ['require-2.1.18.min.js'], cwd: 'node_modules/courage/src/js/vendor/lib/', dest: '<%=pkg.bin %>/dev',
          rename: function (dest, src) {
            return dest + '/require.min.js';
          }}
        ]
      },
      prod: {
        files: [
          {
            expand: true, src: ['require.min.js'], cwd: '<%=pkg.bin %>/dev', dest: '<%=pkg.bin %>/<%=pkg.version%>'
          },
          {expand: true, src: ['reports.css'], cwd: '<%=pkg.bin %>/dev', dest: '<%=pkg.bin %>/<%=pkg.version%>'}
        ]
      }
    }
});
```
This provides the grunt-contrib-copy task with two “profiles”, dev and prod. These tasks can be run via `
grunt:copy:dev` or `grunt:copy:prod`. What’s really neat about the dev and prod configs is that the `files` argument passed is a common grunt config variable that is used across grunt task configs. This [files](http://gruntjs.com/configuring-tasks#files) argument is very powerful. You can do everything from renaming the destination file to dynamically	setting the source files base on globbing patterns.

The other cool thing about grunt config is that you c

## Topics
- Rename is cool
- Using package.json
