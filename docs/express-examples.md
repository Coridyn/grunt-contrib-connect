# Usage examples

## Basic Use
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

## Multiple Servers
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

