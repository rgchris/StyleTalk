=== Compact Style Sheets in Rebol

	Author: Christopher Ross-Gill
	Date: 23-June-2013

CSSR (Compact Style Sheets in Rebol) is a compact reworking of [CSS](http://w3.org/css "Cascading Style Sheets") in Rebol notation. It's designed to be more light-weight, expressive and deliberate.

=== The Format

CSSR is just plain text containing: a few commands to include some core features, some selectors and some properties to apply to selected HTML/XML elements. Values only need be separated by white space and there's no requirement to hold to any particular space/newline/indent patterns. Squeeze everything on one line, or place each value on a separate line.

--- css/reset

A short command to include [Eric Meyer's CSS Reset](http://meyerweb.com/eric/tools/css/reset/) style sheet, occasionally useful in circumventing differences between browsers.

--- google fonts

A literate way to include [Google Web Fonts](http://www.google.com/fonts/) into the style sheet. The format is simply Font Name (quoted) with accompanying styles with Hash (#) prefix:

	google fonts
		"Open Sans" #regular #semibold #italic #italicsemibold
		"PT Sans" #400 #700 #400italic #700italic
		"PT Serif" #400 #700 #400italic #700italic

Although a URL obtained from Google can be used in place.

--- Selectors

Currently there are four supported selector elements and three methods of establishing their relationship.

\table condensed

Type
Example
Description

=b Tags=b.
`<p> <h1> <div>`
Tags pertain to the generic elements of HTML.

=b IDs=b.
`#ticker #messagebox`
IDs apply to the specific element containing the matching ID.

=b Classes=b.
`.header .footer .attention`
Classes relate to elements with given classes.

=b Pseudo-Elements=b.
`:before :after :hover`
Pseudo elements are an adjunct to other elements that apply when those elements are in the given position/state.

/table

Combinations of elements can be used as follows:

	<b> <strong> #alertbox .attention

Rules will apply to all of the above elements.

	<b> in .jumbotron

Rules will apply to all `<b>` elements contained within elements of class `jumbotron`.

	<div> with .jumbotron

Rules will apply to all `div` elements with class `jumbotron`.

	<p> <q> with .attention

Rules will apply to all `<p>` and `<q>` elements with class `attention`.

	<pre> <code> with .rebol in #main

Rules will apply to all `<pre>` and `<code>` elements with class `rebol` contained within the element with the id `main`

Using `and` can separate rule combinations:

	<pre> with .rebol and <code> with .red

Rules will apply to all `<pre>` elements with class `rebol` **and** `<code>` elements with class `red`.

Any rules used before the first selector will apply to the `<body>` element.

--- Values

Values are an important component of each rule.

\table condensed

Value
Sample

Zero 
`0`

Number
`1` or `-0.5`

Ems
`em 1` (or Zero)

Pixels
`1` or `px 1` (or Zero)

Degrees
`deg 45` (or Zero)

Scalars
`\* 1.5` (or Zero)

Percent
`pct 90` (or Zero)

Color
`102.153.204` or `204.204.153.51` or any valid CSS named color, e.g. `black`, `red`

Time
`0:0:1`

Pair
`900x510`

File
`%/a/relative/location`

Url
`http://some.distant/location`

Position
`[top | bottom | right | left]`

Edge
`[[top | bottom] [right | left] | top | bottom | right | left]`

Length
Ems, Pixels or Percent

Angle
Degrees

/table

--- Rules

Rules are composed with one or more of the following contructs.

\table condensed

Rule
Action

`block` `inline block` `inline-block`
Sets the CSS `display` property.

`border-box`
Sets the CSS `border-sizing` property.

`min height <length>` or `min-height <length>`
Sets the CSS `min-height` property.

`margin *1 2* [<length> [auto | <length>]]`
Sets the CSS `margin` property

`margin <position> [<length> | auto]`
Sets the CSS `margin-<position>` property.

`padding *1 4* <length>`
Sets the CSS `padding` property.

`padding <position> <length>`
Sets the CSS `padding-<position>` property.

`border *1 4* [solid | dotted | dashed]`
Sets the CSS `border-style` property (one or more of the following `border` rules need only be prefaced with one `border` keyword, e.g. `border 2 red solid`).

`border *1 4* <color>`
Sets the CSS `border-color` property.

`border radius <length>`
Sets the CSS `border-radius` property.

`border *1 4* <length>`
Sets the CSS `border-width` property.

`radius <length>` or `rounded <length>`
Sets the CSS `border-radius` property

`rounded`
Sets the CSS `border-radius` property to `0.6em`.

`font <length>` 
Sets the CSS `font-size` property (one or more of the following `font` rules need only be prefaced with one `font` keyword, e.g. `font 40 red bold no underline "Open Sans" shadow 1x1 em 2 black`).

`font *any* <font-name> *opt* [sans-serif | serif | font-fixed]`
Sets the CSS `font-name` property where `<font-name>` is a string, e.g. `\"Open Sans\"`

`font <color>`
Sets the CSS `color` property.

`font line height <number>`
Sets the CSS `line-height` property.

`font spacing <number>`
Sets the CSS `letter-spacing` property.

`font shadow <pair> <length> <color>`
Sets the CSS `text-shadow` property. The `<pair>` defines the offset, the `<length>` sets the blur length and `<color>` sets the shadow color.

`font strike through` or `font line-through`
Sets the CSS `text-decoration` property to `line-through`.

`font *opt* no [bold | italic | underline]`
Sets one CSS property of `font-weight`, `font-style` or `text-decoration` dependent on the style.

`=i opt* no [bold | italic | underline]`
Sets one CSS property of `font-weight`, `font-style` or `text-decoration` dependent on the style.

`shadow <pair> <length> <color>`
Sets the CSS `box-shadow` property. The `<pair>` defines the offset, the `<length>` sets the blur length and `<color>` sets the shadow color.

`color <color>`
Sets the CSS `color` property.

`[relative | absolute | fixed] *any* [<position> <length>]`
Sets the CSS `position` property with optional edge positions.

`opacity <number>`
Sets the CSS `opacity` property.

`opaque`
Sets the CSS `opacity` property to `1`.

`no-wrap`
Sets the CSS `white-space` property.

`center`
Sets the CSS `text-align` property.

`transition <property> <time> *opt* <time>`
Sets the CSS `transition` property. This is an essential component of CSS animation—it describes the duration of the graduated change when one of the element's properties change. This can be used many times for different properties. Properties include `width`, `background`, `color`, `opacity`, `transform` (and more).

`<time>`
Sets the CSS `transition` property for all properties.

`translate [x | y | z] <length>`
Appends to the CSS `transform` property.

`rotate <angle>`
Appends to the CSS `transform` property.

`scale [[x | y] <number> | *1 2* <number>]`
Appends to the CSS `transform` property.

`hide`
Sets the CSS `display` property to `none`.

`float [left | right]`
Sets the CSS `float` property.

`pointer`
Sets the CSS `cursor` property.

`[canvas | background] <color>`
Sets the CSS `background-color` property.

`canvas <edge>`
Sets the CSS `background-position` property.

`canvas [repeat [x | y] | no repeat]`
Sets the CSS `background-repeat` property.

`<file> | <url>`
Sets the CSS `background-image` property.

`radial <color> <color>`
Sets the CSS `background-image` property.

`linear <angle> <color> <color>`
Sets the CSS `background-image` property.

`linear to <edge> <color> <color>`
Sets the CSS `background-image` property.

`<color>`
First instance sets the CSS `color` property, second sets the CSS `background-color` property.

`<pair>`
Sets the CSS `width` and `height` properties (in pixels).

`<length>`
Sets the CSS `width` property.

/table

=== Example

A non-exhaustive example:

	css/reset

	white black  ; <body> set to white text on black

	<a>
		purple white rounded no underline

	.slide
		:0:1  ; transition speed
		black white  ; black text on white background
		font 50 "Open Sans" "Helvetica" sans-serif line height * 1.5

	<b> in .slide bold green
