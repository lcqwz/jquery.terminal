```
      _______                 ________                        __
     / / _  /_ ____________ _/__  ___/______________  _____  / /
 __ / / // / // / _  / _/ // / / / _  / _/     / /  \/ / _ \/ /
/  / / // / // / ___/ // // / / / ___/ // / / / / /\  / // / /__
\___/____ \\__/____/_/ \__ / /_/____/_//_/_/_/_/_/  \/\__\_\___/
         \/          /____/                      version 2.6.2
```
http://terminal.jcubic.pl

[![npm](https://img.shields.io/badge/npm-2.6.2-blue.svg)](https://www.npmjs.com/package/jquery.terminal)
![bower](https://img.shields.io/badge/bower-2.6.2-yellow.svg)
[![travis](https://travis-ci.org/jcubic/jquery.terminal.svg?branch=master&2bafd85341890ca1331b87614f74879aef0ac64f)](https://travis-ci.org/jcubic/jquery.terminal)
[![Coverage Status](https://coveralls.io/repos/github/jcubic/jquery.terminal/badge.svg?branch=master&6808ef8c98305b27874fd2cfe04caba1)](https://coveralls.io/github/jcubic/jquery.terminal?branch=master)
![downloads](https://img.shields.io/npm/dm/jquery.terminal.svg?style=flat)
[![package quality](http://npm.packagequality.com/shield/jquery.terminal.svg)](http://packagequality.com/#?package=jquery.terminal)
[![](https://data.jsdelivr.com/v1/package/npm/jquery.terminal/badge?style=rounded)](https://www.jsdelivr.com/package/npm/jquery.terminal)
[![](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/jcubic/jquery.terminal/blob/master/LICENSE)

### Summary

jQuery Terminal Emulator is a plugin for creating command line interpreters in
your applications. It can automatically call JSON-RPC service when a user types
commands or you can provide you own function in which you can parse user
commands. It's ideal if you want to provide additional functionality for power
users. It can also be used to debug your application.

You can use this JavaScript library to create web based terminal on any website.

### Features:

* You can create an interpreter for your JSON-RPC service with one line
  of code (just use url as first argument).

* Support for authentication (you can provide functions when users enter
  login and password or if you use JSON-RPC it can automatically call
  login function on the server and pass token to all functions).

* Stack of interpreters - you can create commands that trigger additional
  interpreters (eg. you can use couple of JSON-RPC service and run them
  when user type command)

* Command Tree - you can use nested objects. Each command will invoke a
  function, if the value is an object it will create a new interpreter and
  use the function from that object as commands. You can use as many nested
  object/commands as you like. If the value is a string it will create
  JSON-RPC service.

* Support for command line history, it uses Local Storage if possible.

* Support for tab completion.

* Includes keyboard shortcut from bash like CTRL+A, CTRL+D, CTRL+E etc.

* Multiply terminals on one page (every terminal can have different
  command, it's own authentication function and it's own command history).

* It catches all exceptions and displays error messages in the terminal
  (you can see errors in your javascript and php code in terminal if they
  are in the interpreter function).

* Using extended commands you can change working of the terminal without
  touching the front-end code (using echo method and terminal formatting
  like syntax). Read more in
  [docs](https::/terminal.jcubic.pl/api_reference.php#extended_commands)

### Installation

Include jQuery library, you can use cdn from http://jquery.com/download/


```html
<script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>

```


Then include js/jquery.terminal-2.6.2.min.js and css/jquery.terminal-2.6.2.min.css

You can grab the files from CDN:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.terminal/2.6.2/js/jquery.terminal.min.js"></script>
<link href="https://cdnjs.cloudflare.com/ajax/libs/jquery.terminal/2.6.2/css/jquery.terminal.min.css" rel="stylesheet"/>
```

or

```html
<script src="https://cdn.jsdelivr.net/npm/jquery.terminal@2.6.2/js/jquery.terminal.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/jquery.terminal@2.6.2/css/jquery.terminal.min.css">
```

If you always want latest version, you can get it from [unpkg](https://unpkg.com/) without specifying version,
it will redirect to the latest ones:


```html
<script src="https://unpkg.com/jquery.terminal/js/jquery.terminal.min.js"></script>
<link href="https://unpkg.com/jquery.terminal/css/jquery.terminal.min.css" rel="stylesheet"/>
```

#### Bleeding Edge Version

If you want to test bleeding edge, development version of jQuery Terminal. You can use those files:

```html
<script src="https://cdn.rawgit.com/jcubic/jquery.terminal/devel/js/jquery.terminal.min.js"></script>
<link href="https://cdn.rawgit.com/jcubic/jquery.terminal/devel/css/jquery.terminal.min.css" rel="stylesheet"/>
```

but the service will be [shutdown in October 2019](https://rawgit.com/), and it don't accept new files. Great things
about rawgit that (at least when you don't put cdn subdomain) you can get lastest version very fast after the commit
is pushed on GitHub (sometimes you need to add no cache query string, anything unique, for faster refresh).

there is also alternative from jsdelivr:

```html
<script src="https://cdn.jsdelivr.net/gh/jcubic/jquery.terminal@devel/js/jquery.terminal.min.js"></script>
<link href="https://cdn.jsdelivr.net/gh/jcubic/jquery.terminal@devel/css/jquery.terminal.min.css" rel="stylesheet"/>
```

but it's not refreshed as fast as rawgit does, because it's CDN and need to be propagated to different servers.

#### Keyboard key polyfill

**NOTE:** From version 1.0.0 if you want to support old browsers then you'll need to use [key event property polyfill](https://rawgit.com/inexorabletash/polyfill/master/keyboard.js). You can check the support for it on [can I use](https://caniuse.com/#feat=keyboardevent-key) (as you can see in chart it's required by Android so it's good to add).

```html
<script src="https://cdn.rawgit.com/inexorabletash/polyfill/master/keyboard.js"></script>
```

or

```html
<script src="https://unpkg.com/js-polyfills@0.x.x/keyboard.js"></script>
```

#### Command Line


You can also install jQuery Terminal using command line, from [bower repository](https://bower.io/):

```
bower install jquery.terminal
```

or [npm registry](https://www.npmjs.com/):

```
npm install jquery.terminal
```


### Example of usage

This is code that uses low level function, that gives you full control of the commands,
just pass anything that the user types into a function.

```javascript
jQuery(function($, undefined) {
    $('#term_demo').terminal(function(command) {
        if (command !== '') {
            var result = window.eval(command);
            if (result != undefined) {
                this.echo(String(result));
            }
        }
    }, {
        greetings: 'Javascript Interpreter',
        name: 'js_demo',
        height: 200,
        width: 450,
        prompt: 'js> '
    });
});
```

Here is a higher level call, using an object as an interpreter, By default the terminal will
parse commands that a user types and replace number like strings with real numbers
regex with regexes and process escape characters in double quoted strings.

```javascript
jQuery(function($, undefined) {
    $('#term_demo').terminal({
        add: function(a, b) {
            this.echo(a + b);
        },
        foo: 'foo.php',
        bar: {
            sub: function(a, b) {
                this.echo(a - b);
            }
        }
    }, {
        height: 200,
        width: 450,
        prompt: 'demo> '
    });
});
```

Command foo will execute json-rpc from foo.php file.

You can create JSON-RPC interpreter with authentication in just one line:

```javascript
$('#term_demo').terminal('service.php', {login: true});
```

More examples [here](http://terminal.jcubic.pl/examples.php). You can also check
[Full Documentation](http://terminal.jcubic.pl/api_reference.php) or
[Getting Started Guide in Wiki](https://github.com/jcubic/jquery.terminal/wiki/Getting-Started).

### Security

Because of security in version 1.20.0 links with protocols different then ftp or http(s) (it was
possible to enter javascript protocol, that could lead to XSS if author of hte app echo user input
and save it in DB) was turn off by default. To enable it, you need to use `anyLinks: true` option.

In version 1.21.0 executing terminal methods using extendend commands `[[ terminal::clear() ]]` was
also disabled by default because attacker (depending on your application) could execute
`terminal::echo` with raw option to enter any html and execute any javascript. To enable this
feature from this version you need to use `invokeMethods: true` option.

The features are safe to enable, if you don't save user input in DB and don't echo it back to
different users (like with chat application). It's also safe if you escape formatting before you
echo stuff.

If you don't save user input in DB but allow to echo back what user types and have enabled
`execHash` options, you may have reflected XSS vulnerability if you enable this features. If you
escape formatting this options are also safe.

**NOTE**: To disable exec if you have `execHash` (or echo stuff from users with `invokeMethods: true`),
you can also set option `{exec: false}` to your `echo` call and use it only when you get
values from server (not from DB indireclty from users). If you do this you will be able to echo stuff
from users and execute terminal methods from server (this feature is mostly done just for that).

### Contributors

If you want to contrubite read [CONTRIBUTING.md](CONTRIBUTING.md) first. Here are project contributors:

<!-- CONTRIBUTORS-START -->
<!-- CONTRIBUTORS-END -->

[jQuery Terminal Website](https://github.com/jcubic/jquery.terminal-www) contributors:

<!-- CONTRIBUTORS-WWW-START -->
<!-- CONTRIBUTORS-WWW-END -->


### License

Licensed under [MIT](http://opensource.org/licenses/MIT) license

Copyright (c) 2011-2019 [Jakub Jankiewicz](https://jcubic.pl/jakub-jankiewicz)
