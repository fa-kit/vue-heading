# FA-Kit vue module Heading

Is a part of [FA-Kit lib](https://github.com/fa-kit).

## Introduction

Hi!

THis is usefull module to work with headings in you project. Mostly is needed if you have 2 or more columns text that contains headings. In that case even with good design guide you may got problem with with an even or odd number of rows in the headings.

I've created this module to make auto calc offsets easier.

## Installation

You can use source from github or install it like a npm package:

```
npm i @fakit/vue-heading
```

This is scoped package, so if in future you will use other packages from FA-Kit it will be in one directory.

## Usage

When you install package you can import it to your Vue project:

```javascript
import Heading from './components/Heading.vue'

// Then locally:

export default {
    components: {
        Heading
    }
}

// or global:

Vue.component('Heading', Heading)
```

And finally you can use it in your HTML:

```html
<Heading
    :level="3"
    :calc-offset="true"
    :calc-move="[0, 2]">Привет, мир</Heading>
```

And in output you may got something like this:

```html
<h3 
    class="heading heading--3" 
    style="line-height: 1.3; margin-top: 0px; margin-bottom: 17.6px;">Привет, мир</h3>
```

Also you can watch example with all props:

```html
<Heading
    :level="2"
    :calc-offset="true"
    :calc-move="[0, 2]"
    :line-height="1"
    :text-line-size="22.4">Integer tincidunt. Cras dapibus.</Heading>
```

### Props

1. level — For correct heading level (h[level]). Only this property is required.
1. line-height — Non important value, have fallback from original styles goes with CSS-code or browser defaults. But if you need to set specific lineheight you can use this one.
1. calc-offset — Trigger to start calculation compensation for test lines. Without this param this is very simple heading.
1. text-line-size — Value is needed if you need to calculate offset in case you document have no default params.
1. calc-move — Contain ratio that will be used in calculating additional offset. Default value is [2, 0]: means that heading will have more offset from top to fit the lines.

### Defaults

1. Class name as default have FA-Kit logic mixed with BEM — ```heading heading--h1```. You can pass additional classes.
1. calc-move — [2, 0]. If you want change it pass array width new ratio, like [1, 1]