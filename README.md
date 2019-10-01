# vue-blotter
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Easily use [Blotter's](https://github.com/bradley/Blotter) unconventional text effects in your Vue project

## Features
- Declarative API for creating Blotter text effects
- Pause animation when outside the viewport
- Combine with Vue's event API to manipulate effects

## Getting started
Blotter.js is not currently available on npm, so you will need to add CDN links to the library and materials you need. (I'm currently looking into how we can get Blotter and the material files up on npm to make this process more seamless)

For Vue you will need to add the main blotter.js library link and any additional blotter material links to your `index.html` file:
Library:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/blotter.min.js"></script>
```
Materials (At least one):
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/channelSplitMaterial.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/fliesMaterial.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/liquidDistortMaterial.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/rollingDistortMaterial.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/slidingDoorMaterial.min.js"></script>
```

For Nuxt you will use the same links but in your `nuxt.config.js`
```js
module.exports = {
    mode: 'spa',
    head: {
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description' }
    ],
    link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }],
    script: [
      { src: 'https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/blotter.min.js' },
      { src: 'https://cdnjs.cloudflare.com/ajax/libs/Blotter/0.1.0/materials/rollingDistortMaterial.min.js' }
    ]
    },
}
```

## Usage
Once you've imported the scripts you can access the component by installing it:

`npm install vue-blotter`

and then importing it:

```js
<template>
    <vue-blotter
        family="Garamond"
        fill="#FFAAEE"
        textStyle="italic"
        :size="120"
        materialType="RollingDistortMaterial"
        text="Blotter"
        :uniforms="{
            uSineDistortSpread: 0.25,
            uSineDistortAmplitude: 0.8
        }"
    >
        <!-- Element to append canvas to -->
        <h1 slot-scope="{ blotterScope }"></h1>
    </vue-blotter>
</template>

<script>
import VueBlotter from 'vue-blotter'

export default {
  components: {
    VueBlotter
  }
}
</script>
```

## Interaction example
```js
<template>
    <vue-blotter
        family="Inconsolata"
        fill="#171717"
        textStyle="normal"
        :size="180"
        materialType="LiquidDistortMaterial"
        text="Blotter"
        :uniforms="{
            uSpeed: 0.25
        }"
    >
        <h1 slot-scope="{ blotterScope }" @mousemove="(e) => mouseMove(e, blotterScope)"></h1>
    </vue-blotter>
</template>

<script>
import VueBlotter from 'vue-blotter'

export default {
  components: {
    VueBlotter
  },
  methods: {
      mouseMove(e, scope) {
          scope.material.uniforms.uSpeed.value = (e.x + e.y) / 4500
      }
  }
}
</script>
```

## Configuration options

Detailed information about materials and their respective uniforms can be found at https://blotter.js.org/#/materials

I've exposed the following configuration options, if you think I've missed anything major please feel free to create an issue.

| Key | Type | Default | Description |
|--|--|--|--|
| `text` | String | 'Blotter' | The text to be rendered |
| `materialType` | String | 'LiquidDistortMaterial' | The type of Blotter material to be used |
| `uniforms` | Object | {} | The uniforms to be applied to the material |
| `autoplay` | Boolean | true | Whether or not to immediately begin the render loop |
| `checkInViewport` | Boolean | true | Whether to pause the animation when outisde the viewport |
| `family` | String | 'Garamond' | The font family to be used |
| `size` | Number | 120 | The size of the font |
| `fill` | String | '#171717' | The fill colour of the text |
| `textStyle` | String | 'normal' | The style of the text |
| `weight` | Number | 400 | The weight of the font |
| `padding` | Number | 0 | The padding around the text |
| `paddingTop` | Number | 0 | The top padding of the text |
| `paddingRight` | Number | 0 | The right padding of the text |
| `paddingBottom` | Number | 0 | The bottom padding of the text |
| `paddingLeft` | Number | 0 | The left padding of the text |

## Contributing
Please read [CONTRIBUTING.md](https://github.com/arapl3y/vue-blotter/CONTRIBUTING.md) for details on code of conduct, and the process for submitting pull requests.

## Acknowledgments
- [Bradley Griffith](http://bradley.computer/) for making the awesome Blotter.js library
- [Adam Watham](https://adamwathan.me/) for his excellent course on [Advanced Vue Component Design](https://adamwathan.me/advanced-vue-component-design/)
- [Mentally Friendly](https://mentallyfriendly.com) for supporting me in releasing my first Open Source Vue component
- [vue-sfc-rollup](https://github.com/team-innovation/vue-sfc-rollup) boilerplate which made uploading a Vue component to npm really easy