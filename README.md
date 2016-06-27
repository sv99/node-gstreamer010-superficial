node-gstreamer010-superficial
==========================

Backport [Superficial GStreamer binding](https://github.com/dturing/node-gstreamer-superficial) to gstreamer-0.10


## What?

This is a superficial binding of GStreamer to nodejs. It does not attempt at being a complete binding, and will hopefully one day be replaced by (or implemented with) node-gir.


## How?

```javascript
var gstreamer = require('gstreamer-superficial');
var pipeline = new gstreamer.Pipeline("videotestsrc ! textoverlay name=text ! autovideosink");
pipeline.play();
```

Then, you can find an element within the pipeline, and set its properties:

```javascript
var target = pipeline.findChild("text");

target.set("text", "Hello");
target.set( {
	"text":"Hello", 
	"font-desc":"Helvetica 32",
	} );
```

Pipeline also knows .stop(), .pause() and .pollBus(),
the elements returned by findChild support .set(object) and .set(name,value), appsinks also support .pull (see below). 


### Polling the GStreamer Pipeline Bus

You can asynchronously handle bus messages using Pipeline.pollBus(callback):

```javascript
pipeline.pollBus( function(msg) {
	console.log( msg );
});
```

### Handling binary data

You can feast off GStreamer's appsink to handle binary data.
.pull starts a background work queue and calls your callback whenever a buffer is (or caps are) available:

```javascript
var appsink = pipeline.findChild("sink");

appsink.pull( function(buf) {
	console.log("BUFFER size",buf.length);
}, function(caps) {
	console.log("CAPS",caps);
} );
```


(see examples/appsink.js)


## Who?

gstreamer-superficial was written by Daniel Turing (mail AT danielturing.com) and currently licensed under the GPLv3. Pester me if you prefer a different License..
