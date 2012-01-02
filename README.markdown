ordered-emitter
===============

Buffer events that may arrive out of order so that they are emitted in order.

Just emit event objects with an `"order"` key starting at 0.

example
=======

emit.js
-------

``` js
var OrderedEmitter = require('ordered-emitter');
var em = new OrderedEmitter;

em.on('beep', function (obj) {
    console.dir(obj);
});

var objects = [
    { order : 1 },
    { order : 2 },
    { order : 4 },
    { order : 0 },
    { order : 3 },
];

var iv = setInterval(function () {
    var obj = objects.shift();
    if (!obj) clearInterval(iv)
    else em.emit('beep', obj)
}, 500);
```

output:

```
{ order: 0 }
{ order: 1 }
{ order: 2 }
{ order: 3 }
{ order: 4 }
```

methods
=======

var OrderedEmitter = require('ordered-emitter');

new OrderedEmitter(opts={})
---------------------------

`OrderedEmitter` acts just like an EventEmitter, except that any event that
emits objects as its first argument with numeric `order` keys will be buffered
so that the events will be emitted in order.

By default, order keys are isolated by event names so the order keys from
different event names won't influence each other. However, you can have order
keys work across multiple event names by setting `opts.span` to `true`.

install
=======

With [npm](http://npmjs.org) do:

```
npm install ordered-emitter
```

license
=======

MIT/X11
