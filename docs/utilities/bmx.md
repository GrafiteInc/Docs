# Bootstrap Magic Extras

A great question you may find yourself asking is why do this at all? There is tailwind and there are lots of parts built into Bootstrap already. Its true that Tailwind is great, however we predominantly use Bootstrap and its utlities are lacking. This collection of animations, utilities, and small bonus pieces are all built around Bootstrap's variables ensuring that anything added follows suit with the standard Bootstrap feel. This to us covers 95% of the needed aspects of designing and making any modern website.


[Source Code](https://github.com/grafiteinc/bmx)

!!! warning "The charts package only works with Bootstrap 4.6+"

## Setup

```sh
npm i @grafite/bootstrap-magic-extras
```

## Format

All classes follow a simple format. `bmx` as the prefix, then the specific class details then `hover` and or `gpu` appended to the end to get more accurate.

## Animations
Animations can have a `duration` and `delay` set for them. By default all animations have a `0s` delay and a `1s` duration.

```html
<div class="bmx-animation-duration-8 bmx-animation-delay-3 bmx-bounce">Animated Div</div>
```

```
"200": 200ms,
"500": 500ms,
"2": 2s,
"3": 3s,
"4": 4s,
"5": 5s,
"6": 6s,
"7": 7s,
"8": 8s,
"9": 9s,
"10": 10s
```

#### Bounce
```html
<p class="bmx-bounce">Just Bounce</p>
<p class="bmx-bounce-hover">Bounce on Hover</p>
```

#### Fade
```html
<p class="bmx-fade-in">Just fade in</p>
<p class="bmx-fade-in-hover">Fade in on Hover</p>
<p class="bmx-fade-out">Just fade out</p>
<p class="bmx-fade-out-hover">Fade out on hover</p>
```

#### Hinge
```html
<p class="bmx-hinge">Just hinge</p>
<p class="bmx-hinge-hover">Hinge on Hover</p>
```

#### Pulse
```html
<p class="bmx-pulse">Just pulse</p>
<p class="bmx-pulse-hover">Pulse on Hover</p>
```

#### Rubber Band
```html
<p class="bmx-rubber-band">Just rubber-band</p>
<p class="bmx-rubber-band-hover">Rubber-band on Hover</p>
```

#### Shake
```html
<p class="bmx-shake-x">Just shake X axis</p>
<p class="bmx-shake-x-hover">Shake X axis on Hover</p>
<p class="bmx-shake-y">Just shake Y axis</p>
<p class="bmx-shake-y-hover">Shake Y axis on Hover</p>
```

#### Swing
```html
<p class="bmx-swing">Just swing</p>
<p class="bmx-swing-hover">Swing on Hover</p>
```

#### Tada
```html
<p class="bmx-tada">Just tada</p>
<p class="bmx-tada-hover">Tada on Hover</p>
```

## Utilities

### Standard

Anything color related has the following colors, which are set by Bootstrap variables:

```
white
black
blue
indigo
purple
pink
red
orange
yellow
green
teal
cyan
gray-100
gray-200
gray-300
gray-400
gray-500
gray-600
gray-700
gray-800
gray-900
```

#### Backgrounds
```html
<p class="bmx-bg-black">Black BG</p>
<p class="bmx-bg-black-hover">Black BG on Hover</p>
```

#### Borders

Borders range in size from 1px to 10px.

```html
<p class="bmx-border-3">3px border</p>
<p class="bmx-border-3-hover">3px border on hover</p>
<p class="bmx-border-black">Black border</p>
<p class="bmx-border-black-hover">Black border on hover</p>
```

#### Buttons
```html
<button class="bmx-btn-indigo">Indigo button</button>
<button class="bmx-btn-outline-indigo">indigo border on hover</button>
```

#### Cursor
```html
<img src="picture.png" class="bmx-pointer">
```

#### Heights

Heights follow these patterns:

```
mh: min-height
h: height
vh: viewport height
```

Min heights are based on viewport height. The following are available:
```
25, 50, 75, 100
```
Standard heights are based on `rem` values ranging from the following:
```
1 - 24
```

```html
<div class="bmx-mh-25"></div>
<div class="bmx-h-24"></div>
<div class="bmx-vh-75"></div>
```

#### Margins & Padding

All margins and padding follow Bootstrap style with `m, mt, mb, mr, ml, my, mx` and adds a full range of `rem` values which range from:
```
1 - 10
```

```html
<div class="bmx-m-7"></div>
<div class="bmx-py-9"></div>
```

#### Shadow
```html
<img src="picture.png" class="bmx-drop-shadow">
<img src="picture.png" class="bmx-drop-shadow-hover">
```

#### Text
Opacities help you access a further range of exising colors defined by your Bootstrap config. The following percentage keys are available by default.

```
0
5
10
20
25
30
40
50
60
70
75
80
90
95
100
```

```html
<p class="bmx-text-blue">blue text</p>
<p class="bmx-text-opacity-80">blue text at 80% opacity</p>
<p class="bmx-link-blue">blue link style with hover etc.</p>
<p class="bmx-td-non-hover">No text decoration on hover</p>
<p class="bmx-td-underline">Underline the text</p>
<p class="bmx-td-underline-hover">Underline the text on hover</p>
```

#### Typography
The following font sizes, weights and line heights are available.

Font Sizes:
```
"10": 5rem
"9": 4.5rem
"8": 4rem
"7": 3.5rem
"6": 3rem
"5": 2.5rem
"4": 2rem
"3": 1.75rem
"2": 1.5rem
"1": 1.25rem
```

Font Weights
```
"b": bold
"l": lighter
"100": 100
"200": 200
"300": 300
"400": 400
"500": 500
"600": 600
"700": 700
"800": 800
"900": 900
```

Line Heights
```
"10": 5rem
"9": 4.5rem
"8": 4rem
"7": 3.5rem
"6": 3rem
"5": 2.5rem
"4": 2rem
"3": 1.75rem
"2": 1.5rem
"1": 1.25rem
```

```html
<p class="bmx-fs-2">Font size of 2rem</p>
<p class="bmx-fw-b">Font weight of bold</p>
<p class="bmx-fw-l">Font weight of lighter</p>
<p class="bmx-fw-300">Font weight of 300</p>
<p class="bmx-lh-4">Line height of 4rem</p>
<p class="bmx-f-mono">Mono font family</p>
```

### Transition
Transitions use the default `$transition-base` variable from Bootstrap.

#### Blur
The following are the blur scopes:
```
"0": 0px
"1": 3px
"2": 6px
"3": 18px
"4": 72px
```

```html
<img src="picture.png" class="bmx-blur-3">
<img src="picture.png" class="bmx-blur-0-hover">
```

#### Rotate
The following are rotate scopes:
```
"0": 0deg
"1": 45deg
"2": 90deg
"3": 180deg
"4": 270deg
```

```html
<img src="picture.png" class="bmx-rotate-3">
<img src="picture.png" class="bmx-rotate-0-hover">
```
#### Scale
The following are scale scopes:
```
"0": 1.00
"1": 1.05
"2": 1.15
"3": 1.30
"4": 1.50
```

```html
<img src="picture.png" class="bmx-scale-3">
<img src="picture.png" class="bmx-scale-0-hover">
```

### Specials
The specials use the `skew` transform method.

The skews and slants use the following default scopes:

```
"1": 1deg
"2": 2deg
"3": 3deg
"4": 4deg
"5": 5deg
"6": 6deg
"7": 7deg
"8": 8deg
"9": 9deg
"10": 10deg
```

#### Skew
```html
<div class="bmx-h-12 bmx-bg-indigo bmx-skew-5">This div is skewed by 5deg.</div>
```

#### Slant
Slants use `before` and `after` content components,

```html
<div class="bmx-h-12 bmx-bg-indigo bmx-slant-top-n5 bmx-slant-bottom-n5">Has a top and bottom slant</div>
```

## General Variables

There are a few general variables you can set for your use cases. The thing to note here is that the height class `bmx-min-h-window` does the calculation of the window height minus the header and footer assuming that your using sticky or fixed headers and footers.

```scss
$bmx-header-height: 52px;
$bmx-footer-height: 52px;
$bmx-mono-font-family: SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace;
```
