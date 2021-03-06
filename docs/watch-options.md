# Settings

There are a number of options available. Please review the [minimatch options here](https://github.com/isaacs/minimatch#options). As well as some additional options as follows:

## files
Type: `String|Array`

This defines what file patterns this task will watch. Can be a string or an array of files and/or minimatch patterns.

## tasks
Type: `String|Array`

This defines which tasks to run when a watched file event occurs.

## options.spawn
Type: `Boolean`
Default: true

Whether to spawn task runs in a child process. Setting this option to `false` speeds up the reaction time of the watch (usually 500ms faster for most) and allows subsequent task runs to share the same context. Not spawning task runs can make the watch more prone to failing so please use as needed.

Example:
```js
watch: {
  scripts: {
    files: ['**/*.js'],
    tasks: ['jshint'],
    options: {
      spawn: false,
    },
  },
},
```

*For backwards compatibility the option `nospawn` is still available and will do the opposite of `spawn`.*

## options.interrupt
Type: `Boolean`
Default: false

As files are modified this watch task will spawn tasks in child processes. The default behavior will only spawn a new child process per target when the previous process has finished. Set the `interrupt` option to true to terminate the previous process and spawn a new one upon later changes.

Example:
```js
watch: {
  scripts: {
    files: '**/*.js',
    tasks: ['jshint'],
    options: {
      interrupt: true,
    },
  },
},
```

## options.debounceDelay
Type: `Integer`
Default: 500

How long to wait before emitting events in succession for the same filepath and status. For example if your `Gruntfile.js` file was `changed`, a `changed` event will only fire again after the given milliseconds.

Example:
```js
watch: {
  scripts: {
    files: '**/*.js',
    tasks: ['jshint'],
    options: {
      debounceDelay: 250,
    },
  },
},
```

## options.interval
Type: `Integer`
Default: 100

The `interval` is passed to `fs.watchFile`. Since `interval` is only used by `fs.watchFile` and this watcher also uses `fs.watch`; it is recommended to ignore this option. *Default is 100ms*.

## options.event
Type: `String|Array`
Default: `'all'`

Specify the type watch event that trigger the specified task. This option can be one or many of: `'all'`, `'changed'`, `'added'` and `'deleted'`.

Example:
```js
watch: {
  scripts: {
    files: '**/*.js',
    tasks: ['generateFileManifest'],
    options: {
      event: ['added', 'deleted'],
    },
  },
},
```

## options.reload
Type: `Boolean`
Default: `false`

By default, if `Gruntfile.js` is being watched, then changes to it will trigger the watch task to restart, and reload the `Gruntfile.js` changes.
When `reload` is set to `true`, changes to *any* of the watched files will trigger the watch task to restart.
This is especially useful if your `Gruntfile.js` is dependent on other files.

```js
watch: {
  configFiles: {
    files: [ 'Gruntfile.js', 'config/*.js' ],
    options: {
      reload: true
    }
  }
}
```


## options.forever
Type: `Boolean`
Default: true

This is *only a task level option* and cannot be configured per target. By default the watch task will duck punch `grunt.fatal` and `grunt.warn` to try and prevent them from exiting the watch process. If you don't want `grunt.fatal` and `grunt.warn` to be overridden set the `forever` option to `false`.

## options.dateFormat
Type: `Function`

This is *only a task level option* and cannot be configured per target. By default when the watch has finished running tasks it will display the message `Completed in 1.301s at Thu Jul 18 2013 14:58:21 GMT-0700 (PDT) - Waiting...`. You can override this message by supplying your own function:

```js
watch: {
  options: {
    dateFormat: function(time) {
      grunt.log.writeln('The watch finished in ' + time + 'ms at' + (new Date()).toString());
      grunt.log.writeln('Waiting for more changes...');
    },
  },
  scripts: {
    files: '**/*.js',
    tasks: 'jshint',
  },
},
```

## options.atBegin
Type: `Boolean`
Default: false

This option will trigger the run of each specified task at startup of the watcher.

## options.livereload
Type: `Boolean|Number|Object`
Default: false

Set to `true` or set `livereload: 1337` to a port number to enable live reloading. Default and recommended port is `35729`.

If enabled a live reload server will be started with the watch task per target. Then after the indicated tasks have run, the live reload server will be triggered with the modified files.

See also how to [enable livereload on your HTML](https://github.com/gruntjs/grunt-contrib-watch/blob/master/docs/watch-examples.md#enabling-live-reload-in-your-html).

Example:
```js
watch: {
  css: {
    files: '**/*.sass',
    tasks: ['sass'],
    options: {
      livereload: true,
    },
  },
},
```

It's possible to get livereload working over https connections. To do this, pass an object to `livereload` with a `key` and `cert` paths specified.

Example:
```js
watch: {
  css: {
    files: '**/*.sass',
    tasks: ['sass'],
    options: {
      livereload: {
        port: 9000,
        key: grunt.file.read('path/to/ssl.key'),
        cert: grunt.file.read('path/to/ssl.crt')
        // you can pass in any other options you'd like to the https server, as listed here: http://nodejs.org/api/tls.html#tls_tls_createserver_options_secureconnectionlistener
      }
    },
  },
},
```


## options.cwd
Type: `String|Object`
Default: `process.cwd()`

Ability to set the current working directory. Defaults to `process.cwd()`. Can either be a string to set the cwd to match files and spawn tasks. Or an object to set each independently. Such as `options: { cwd: { files: 'match/files/from/here', spawn: 'but/spawn/files/from/here' } }`.

Set `options: { cwd: { files: 'a/path', event: 'a/path' }}` to strip off `a/path` before emitting events. This option is useful for specifying the base directory to use with livereload.


## options.livereloadOnError
Type: `Boolean`  
Default: `true`  

Option to prevent the livereload if the executed tasks encountered an error.  If set to `false`, the livereload will only be triggered if all tasks completed successfully.
