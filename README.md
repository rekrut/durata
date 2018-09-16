# Durata

A class to move a initial float value to a target value during a defined time period.

### Features / Structure / Interface
* Test driven with karma
* AMD-, CommonJS and global-support 

### Installation

```shell
    $ npm install durata
```

### Examples

##### Simple Demonstration

```html
<div id="output"></div>
<script src="node_modules/durata/dist/durata.min.js"></script>
<script>
    var interval,
        output = document.getElementById('output'),
        // Move value from 200 to 10000 during 5000 milliseconds
        progress = Durata.create(200, 10000, 5000);

    // Output of value each 200 milliseconds
    interval = setInterval(function () {
        output.innerText = Math.ceil(progress.get());
        // Stop output, when animation is complete
        if (progress.isComplete()) {
            clearInterval(interval);
        }
    }, 200);
</script>
```

##### Create a new animated value

Moving a maroon colored div from a upper left position to a lower right position.

```html
<div id="spot"
     style="position: absolute;
            width: 100px;
            height: 100px;
            top:30px;
            left: 45px;
            background-color:maroon">&nbsp;</div>

<script src="node_modules/durata/dist/durata.min.js"></script>

<script>
    var spot = document.getElementById('spot'),
        xPos = Durata.create(5, 515, 1000),
        yPos = Durata.create(25, 445, 1200);

    function updateSpot(xPos, yPos) {
        spot.style.left = xPos.get() + 'px';
        spot.style.top = yPos.get() + 'px';
        if (!xPos.isComplete() || !yPos.isComplete()) {
            requestAnimationFrame(updateSpot.bind(null, xPos, yPos));
        }
    }

    requestAnimationFrame(updateSpot.bind(null, xPos, yPos));
</script>
```

##### Use durata with AMD

Morphing color maroon to marine blue in 2 seconds

```html
<div id="spot"
     style="position: absolute;
            width: 100px;
            height: 100px;
            top:30px;
            left: 45px;
            background-color:maroon">&nbsp;</div>

<script src="node_modules/requirejs/require.js"></script>

<script>
    require.config({baseUrl: 'node_modules'});

    var spot = document.getElementById('spot');
    function updateSpotColor(r, g, b) {
        spot.style.backgroundColor
            = 'rgb(' + r.get() + ',' + g.get() + ',' + b.get() + ')';
        if (!r.isComplete() || !g.isComplete() || !b.isComplete()) {
            requestAnimationFrame(updateSpotColor.bind(null, r, g, b));
        }
    }

    require(['durata/dist/durata.min'], function(Durata) {
        var r = Durata.create(128, 42, 2000);
        var g = Durata.create(0, 107, 2000);
        var b = Durata.create(0, 204, 2000);
        requestAnimationFrame(updateSpotColor.bind(null, r, g, b));
    });
</script>
```

##### Use durata with easing

The easing-function takes a value between 0.0 and 1.0.
For best the function should return a value in this range (0.0 to 1.0).

```html
<div id="spot"
     style="position: absolute;
            width: 100px;
            height: 100px;
            top:30px;
            left: 45px;
            background-color:rgb(42,107,204)">&nbsp;</div>

<script src="node_modules/requirejs/require.js"></script>

<script>
    var spot = document.getElementById('spot');

    function updateSpot(xPos, yPos, r, g, b) {
        spot.style.left = xPos.get() + 'px';
        spot.style.top = yPos.get() + 'px';
        spot.style.backgroundColor
            = 'rgb(' + r.get() + ',' + g.get() + ',' + b.get() + ')';
        if (!xPos.isComplete() || !yPos.isComplete()
            || !r.isComplete() || !g.isComplete() || !b.isComplete()
        ) {
            requestAnimationFrame(updateSpot.bind(null, xPos, yPos, r, g, b));
        }
    }

    require (['durata/dist/durata.min'], function (Durata) {
        var xPos = Durata.create(5, 515, 1000),
                                                // pass easing function
            yPos = Durata.create(25, 445, 1000, function(y) { return y*y*y; });
        var r = Durata.create(42, 255, 2000);
        var g = Durata.create(107, 215, 1200);
        var b = Durata.create(204, 0, 1500);

        requestAnimationFrame(updateSpot.bind(null, xPos, yPos, r, g, b));
    });
</script>
```

### Development

If you want to distribute your changes in the 'dist'-directory, you can use gulp:

```shell
    $ gulp distribute
```

## License

The MIT License (MIT)

Copyright (c) 2018 Daniel Moritz

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.