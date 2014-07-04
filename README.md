# purescript-d3

### Example

Here is the JavaScript code from [part 1](http://bl.ocks.org/mbostock/7322386) of Mike Bostock's [Let's Make a Bar Chart](http://bost.ocks.org/mike/bar/) series of tutorials for D3:

```javascript
var data = [4, 8, 15, 16, 23, 42];

var x = d3.scale.linear()
  .domain([0, d3.max(data)])
  .range([0, 420]);

d3.select(".chart")
  .selectAll("div")
    .data(data)
  .enter().append("div")
    .style("width", function(d) { return x(d) + "px"; })
    .text(function(d) { return d; });
```

And here is the PureScript equivalent:

```haskell
array = [4, 8, 15, 16, 23, 42]

barChart = do
  x <- linearScale
    .. domain [0, max array]
    .. range [0, 420]
    .. freeze

  rootSelect ".chart"
    .. selectAll "div"
      .. bind array
    .. enter .. append "div"
      .. style "width" (\d -> show (x d) ++ "px")
      .. text show
```

Note that `..` is an alias for `>>=`. The [fluent interface](http://en.wikipedia.org/wiki/Fluent_interface) is just a poor man's [programmable semicolon](http://en.wikipedia.org/wiki/Monad_(functional_programming))!

### Development

You will need the following pre-requisites installed:

*  [PureScript](http://www.purescript.org/)
*  [nodejs](http://nodejs.org/)
*  [bower](http://bower.io/) (e.g., `npm install -g bower`)
*  [gulp.js](http://gulpjs.com/) (e.g., `npm install -g gulp`)

Once you have these installed you can run the following in a cloned repo:

```
npm install    # install dependencies from package.json
bower update   # install dependencies from bower.json
gulp           # compile the code
```

# Module Documentation

## Module Graphics.D3.Base

### Types

    type D3Eff a = forall e. Eff (dom :: DOM | e) a

    data DOM :: !


## Module Graphics.D3.Scale

### Types

    data LinearScale :: *


### Type Classes

    class Scale s where


### Type Class Instances

    instance scaleLinear :: Scale LinearScale


### Values

    domain :: forall s a. (Scale s) => [a] -> s -> D3Eff s

    freeze :: forall s. (Scale s) => s -> D3Eff (Number -> Number)

    linearScale :: D3Eff LinearScale

    range :: forall s a. (Scale s) => [a] -> s -> D3Eff s


## Module Graphics.D3.Selection

### Types

    data Selection :: # * -> *


### Values

    append :: forall r. String -> Selection r -> D3Eff (Selection (exists :: Unit | r))

    attr :: forall d v r. String -> (d -> v) -> Selection (hasData :: d | r) -> D3Eff (Selection (hasData :: d | r))

    bind :: forall d r. [d] -> Selection (exists :: Unit | r) -> D3Eff (Selection (isJoin :: Unit, hasData :: d, exists :: Unit | r))

    enter :: forall r. Selection (exists :: Unit, isJoin :: Unit | r) -> D3Eff (Selection r)

    exit :: forall r. Selection (isJoin :: Unit | r) -> D3Eff (Selection r)

    remove :: forall r. Selection r -> D3Eff Unit

    rootSelect :: String -> D3Eff (Selection (exists :: Unit))

    rootSelectAll :: String -> D3Eff (Selection (exists :: Unit))

    select :: forall r. String -> Selection (exists :: Unit | r) -> D3Eff (Selection (exists :: Unit | r))

    selectAll :: forall r. String -> Selection (exists :: Unit | r) -> D3Eff (Selection (exists :: Unit | r))

    style :: forall d v r. String -> (d -> v) -> Selection (hasData :: d | r) -> D3Eff (Selection (hasData :: d | r))

    text :: forall d r. (d -> String) -> Selection (hasData :: d | r) -> D3Eff (Selection (hasData :: d | r))

    transition :: forall r. Selection r -> D3Eff (Selection r)


## Module Graphics.D3.Util

### Values

    max :: [Number] -> Number

    min :: [Number] -> Number



