# grunt-wiwo-express

> Start an ~~connect~~ Express web server.



## Getting Started
This plugin requires Grunt `~0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-wiwo-express --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-wiwo-express');
```




## Express task
_Run this task with the `grunt express` command._

This task is based on the grunt-contrib-connect plugin and is designed to be a drop-in replacement for the default Connect server.

Note that this server only runs as long as grunt is running. Once grunt's tasks have completed, the web server stops. This behavior can be changed with the [keepalive](#keepalive) option, and can be enabled ad-hoc by running the task like `grunt express:keepalive`.

This task was designed to be used in conjunction with another task that is run immediately afterwards, like the [grunt-contrib-qunit plugin](https://github.com/gruntjs/grunt-contrib-qunit) `qunit` task.
### Options

#### port
Type: `Integer`
Default: `8000`

The port on which the webserver will respond. The task will fail if the specified port is already in use.

#### hostname
Type: `String`
Default: `'localhost'`

The hostname the webserver will use.

Setting it to * will make the server accessible from anywhere.

#### base
Type: `String`
Default: `'.'`

The base (or root) directory from which files will be served. Defaults to the project Gruntfile's directory.

#### keepalive
Type: `Boolean`
Default: `false`

Keep the server alive indefinitely. Note that if this option is enabled, any tasks specified after this task will _never run_. By default, once grunt's tasks have completed, the web server stops. This option changes that behavior.

This option can also be enabled ad-hoc by running the task like `grunt express:targetname:keepalive`

#### middleware
Type: `Function`
Default:

```js
function(express, options) {
  return [
    // Serve static files.
    express.static(options.base),
    // Make empty directories browsable.
    express.directory(options.base),
  ];
}
```

Lets you add in your own Express/Connect middlewares. This option expects a function that returns an array of middlewares. See the [project Gruntfile][] and [project unit tests][] for a usage example.

Express is built on Connect and should support most middleware projects built for Connect (but check with the plugin author to be sure). 

[project Gruntfile]: Gruntfile.js
[project unit tests]: test/express_test.js

### Usage examples

#### Basic Use
In this example, `grunt express` (or more verbosely, `grunt express:server`) will start a static web server at `http://localhost:9001/`, with its base path set to the `www-root` directory relative to the gruntfile, and any tasks run afterwards will be able to access it.

```javascript
// Project configuration.
grunt.initConfig({
  express: {
    server: {
      options: {
        port: 9001,
        base: 'www-root'
      }
    }
  }
});
```

If you want your web server to use the default options, just omit the `options` object. You still need to specify a target (`uses_defaults` in this example), but the target's configuration object can otherwise be empty or nonexistent. In this example, `grunt express` (or more verbosely, `grunt express:uses_defaults`) will start a static web server using the default options.

```javascript
// Project configuration.
grunt.initConfig({
  express: {
    uses_defaults: {}
  }
});
```

#### Multiple Servers
You can specify multiple servers to be run alone or simultaneously by creating a target for each server. In this example, running either `grunt express:site1` or `grunt express:site2` will  start the appropriate web server, but running `grunt express` will run _both_. Note that any server for which the [keepalive](#keepalive) option is specified will prevent _any_ task or target from running after it.

```javascript
// Project configuration.
grunt.initConfig({
  express: {
    site1: {
      options: {
        port: 9000,
        base: 'www-roots/site1'
      }
    },
    site2: {
      options: {
        port: 9001,
        base: 'www-roots/site2'
      }
    }
  }
});
```
