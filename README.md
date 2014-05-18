# StyleTalk

	Author: Christopher Ross-Gill
	Date: 23-June-2013

StyleTalk is a compact reworking of [CSS](http://w3.org/css "Cascading Style Sheets") in Rebol notation. It's designed to be more light-weight, expressive and deliberate.

# The Format

StyleTalk is just plain text containing: a few commands to include some core features, some selectors and some properties to apply to selected HTML/XML elements. Values only need be separated by white space and there's no requirement to hold to any particular space/newline/indent patterns. Squeeze everything on one line, or place each value on a separate line.

## css/reset

A short command to include [Eric Meyer's CSS Reset](http://meyerweb.com/eric/tools/css/reset/) style sheet, occasionally useful in circumventing differences between browsers.

## google fonts

A literate way to include [Google Web Fonts](http://www.google.com/fonts/) into the style sheet. The format is simply Font Name (quoted) with accompanying styles with Hash (#) prefix:

```Rebol
google fonts
	"Open Sans" #regular #semibold #italic #italicsemibold
	"PT Sans" #400 #700 #400italic #700italic
	"PT Serif" #400 #700 #400italic #700italic
```

Although a URL obtained from Google can be used in place.

## Selectors

Currently there are four supported selector elements and three methods of establishing their relationship.

| Type | Example | Description
|----|----|----
| **Tags** | `<p> <h1> <div>` | Tags pertain to the generic elements of HTML.
| **IDs** | `#ticker #messagebox` | IDs apply to the specific element containing the matching ID.
| **Classes** | `.header .footer .attention` | Classes relate to elements with given classes.
| **Pseudo-Elements** | `:before :after :hover` | Pseudo elements are an adjunct to other elements that apply when those elements are in the given position/state.

Combinations of elements can be used as follows:

```Rebol
<b> <strong> #alertbox .attention
```

Rules will apply to all of the above elements.

```Rebol
<b> in .jumbotron
```

Rules will apply to all `<b>` elements contained within elements of class `jumbotron`.

```Rebol
<div> with .jumbotron
```

Rules will apply to all `div` elements with class `jumbotron`.

```Rebol
<p> <q> with .attention
```

Rules will apply to all `<p>` and `<q>` elements with class `attention`.

```Rebol
<pre> <code> with .rebol in #main
```

Rules will apply to all `<pre>` and `<code>` elements with class `rebol` contained within the element with the id `main`

Using `and` can separate rule combinations:

```Rebol
<pre> with .rebol and <code> with .red
```

Rules will apply to all `<pre>` elements with class `rebol` **and** `<code>` elements with class `red`.

Any rules used before the first selector will apply to the `<body>` element.

## Values

Values are an important component of each rule.

| Value | Sample
|----|----
| Zero | `0`
| Number | `1` or `-0.5`
| Ems | `em 1` (or Zero)
| Pixels | `1` or `px 1` (or Zero)
| Degrees | `deg 45` (or Zero)
| Scalars | `* 1.5` (or Zero)
| Percent | `pct 90` (or Zero)
| Color | `102.153.204` or `204.204.153.51` or any valid CSS named color, e.g. `black`, `red`
| Time | `0:0:1`
| Pair | `900x510`
| File | `%/a/relative/location`
| Url | `http://some.distant/location`
| Position | </code>[top &#124; bottom &#124; right &#124; left]</code>
| Edge | <code>[[top &#124; bottom] [right &#124; left] &#124; top &#124; bottom &#124; right &#124; left]</code>
| Length | Ems, Pixels or Percent
| Angle | Degrees

## Rules

Rules are composed with one or more of the following contructs.

| Rule | Action
|----|----
| `block` `inline block` `inline-block` | Sets the CSS `display` property.
| `border-box` | Sets the CSS `border-sizing` property.
| `min height <length>` or `min-height <length>` | Sets the CSS `min-height` property.
| <code>margin 1 2 [&lt;length> [auto &#124; &lt;length>]]</code> | Sets the CSS `margin` property
| <code>margin &lt;position> [&lt;length> &#124; auto]</code> | Sets the CSS `margin-<position>` property.
| `padding 1 4 <length>` | Sets the CSS `padding` property.
| `padding <position> <length>` | Sets the CSS `padding-<position>` property.
| <code>border 1 4 [solid &#124; dotted &#124; dashed]</code> | Sets the CSS `border-style` property (one or more of the following `border` rules need only be prefaced with one `border` keyword, e.g. `border 2 red solid`).
| `border 1 4 <color>` | Sets the CSS `border-color` property.
| `border radius <length>` | Sets the CSS `border-radius` property.
| `border 1 4 <length>` | Sets the CSS `border-width` property.
| `radius <length>` or `rounded <length>` | Sets the CSS `border-radius` property
| `rounded` | Sets the CSS `border-radius` property to `0.6em`.
| `font <length>` | Sets the CSS `font-size` property (one or more of the following `font` rules need only be prefaced with one `font` keyword, e.g. `font 40 red bold no underline "Open Sans" shadow 1x1 em 2 black`).
| <code>font any &lt;font-name> opt [sans-serif &#124; serif &#124; font-fixed]</code> | Sets the CSS `font-name` property where `<font-name>` is a string, e.g. `"Open Sans"`
| `font <color>` | Sets the CSS `color` property.
| `font line height <number>` | Sets the CSS `line-height` property.
| `font spacing <number>` | Sets the CSS `letter-spacing` property.
| `font shadow <pair> <length> <color>` | Sets the CSS `text-shadow` property. The `<pair>` defines the offset, the `<length>` sets the blur length and `<color>` sets the shadow color.
| `font strike through` or `font line-through` | Sets the CSS `text-decoration` property to `line-through`.
| <code>font opt no [bold &#124; italic &#124; underline]</code> | Sets one CSS property of `font-weight`, `font-style` or `text-decoration` dependent on the style.
| <code>opt no [bold &#124; italic &#124; underline]</code> | Sets one CSS property of `font-weight`, `font-style` or `text-decoration` dependent on the style.
| `shadow <pair> <length> <color>` | Sets the CSS `box-shadow` property. The `<pair>` defines the offset, the `<length>` sets the blur length and `<color>` sets the shadow color.
| `color <color>` | Sets the CSS `color` property.
| <code>[relative &#124; absolute &#124; fixed] any [&lt;position> &lt;length>]</code> | Sets the CSS `position` property with optional edge positions.
| `opacity <number>` | Sets the CSS `opacity` property.
| `opaque` | Sets the CSS `opacity` property to `1`.
| `no-wrap` | Sets the CSS `white-space` property.
| `center` | Sets the CSS `text-align` property.
| `transition <property> <time> opt <time>` | Sets the CSS `transition` property. This is an essential component of CSS animation—it describes the duration of the graduated change when one of the element's properties change. This can be used many times for different properties. Properties include `width`, `background`, `color`, `opacity`, `transform` (and more).
| `<time>` | Sets the CSS `transition` property for all properties.
| <code>translate [x &#124; y &#124; z] &lt;length></code> | Appends to the CSS `transform` property.
| `rotate <angle>` | Appends to the CSS `transform` property.
| <code>scale [[x &#124; y] &lt;number> &#124; 1 2 &lt;number>]</code> | Appends to the CSS `transform` property.
| `hide` | Sets the CSS `display` property to `none`.
| <code>float [left &#124; right]</code> | Sets the CSS `float` property.
| `pointer` | Sets the CSS `cursor` property.
| <code>[canvas &#124; background] &lt;color></code> | Sets the CSS `background-color` property.
| `canvas <edge>` | Sets the CSS `background-position` property.
| <code>canvas [repeat [x &#124; y] &#124; no repeat]</code> | Sets the CSS `background-repeat` property.
| <code>&lt;file> &#124; &lt;url></code> | Sets the CSS `background-image` property.
| `radial <color> <color>` | Sets the CSS `background-image` property.
| `linear <angle> <color> <color>` | Sets the CSS `background-image` property.
| `linear to <edge> <color> <color>` | Sets the CSS `background-image` property.
| `<color>` | First instance sets the CSS `color` property, second sets the CSS `background-color` property.
| `<pair>` | Sets the CSS `width` and `height` properties (in pixels).
| `<length>` | Sets the CSS `width` property.

# Example

A non-exhaustive example:

```Rebol
css/reset

white black  ; <body> set to white text on black

<a>
	purple white rounded no underline

.slide
	:0:1  ; transition speed
	black white  ; black text on white background
	font 50 "Open Sans" "Helvetica" sans-serif line height * 1.5

<b> in .slide bold green
```
