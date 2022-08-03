# react-medium-image-zoom

[![All Contributors](https://img.shields.io/badge/all_contributors-44-orange.svg?style=flat-square)](#contributors-) [![npm version](https://img.shields.io/npm/v/react-medium-image-zoom.svg?style=flat-square)](https://www.npmjs.com/package/react-medium-image-zoom) [![npm downloads](https://img.shields.io/npm/dm/react-medium-image-zoom.svg?style=flat-square)](https://www.npmjs.com/package/react-medium-image-zoom) [![bundlephobia size](https://badgen.net/bundlephobia/minzip/react-medium-image-zoom)](bundlephobia.com/result?p=react-medium-image-zoom) [![Coverage Status](https://coveralls.io/repos/github/rpearce/react-medium-image-zoom/badge.svg?branch=master)](https://coveralls.io/github/rpearce/react-medium-image-zoom?branch=master)

The original [medium.com-inspired image zooming](https://medium.design/image-zoom-on-medium-24d146fc0c20)
library for [React](https://reactjs.org).

Features:

* `<img />`, including all [`object-fit`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit)
  values, any [`object-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position),
  and [`loading="lazy"`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-loading)
* `<div>` and `<span>` with any [`background-image`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image),
  [`background-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-size),
  and [`background-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)
* `<picture>` with `<source />` and `<img />`
* `<figure>` with `<img />`
* Accessibility:
  * JAWS in Chrome, Edge, and Firefox (Windows)
  * NVDA in Chrome, Edge, and Firefox (Windows)
  * VoiceOver in Safari (macOS, iOS)
  * TalkBack in Chrome (Android)
* Zero `dependencies`

[View the storybook examples](https://rpearce.github.io/react-medium-image-zoom/)
to see various usages. _NOTE: Help is wanted with making the examples more
informative, so please [start a discussion](https://github.com/rpearce/react-medium-image-zoom/discussions)
if you're able to help!_

## Quickstart

```bash
npm install --save react-medium-image-zoom
```

```javascript
import React from 'react'
import Zoom from 'react-medium-image-zoom'
import 'react-medium-image-zoom/dist/styles.css'

export const MyImg = () => (
  <Zoom>
    <img
      alt="That Wanaka Tree, New Zealand by Laura Smetsers"
      src="/path/to/thatwanakatree.jpg"
      width="500"
    />
  </Zoom>
)
```

This library's compilation target is `ES2022`, but it only uses features from
`ES2020` and below. If you find you need to support older browsers, add
`react-medium-image-zoom` to your build system.

## API

You can pass these options to either the `Uncontrolled` (default) or
`Controlled` components.

```typescript
export interface UncontrolledProps {
  // Accessible label text for when you want to unzoom
  // Default: 'Minimize image'
  a11yNameButtonUnzoom?: string

  // Accessible label text for when you want to zoom
  // Default: 'Expand image'
  a11yNameButtonZoom?: string

  // Your image
  children: ReactNode

  // Provide your own unzoom button icon
  // Default: ICompress
  IconUnzoom?: ElementType

  // Provide your own zoom button icon
  // Default: IEnlarge
  IconZoom?: ElementType

  // Scrollable parent element
  // Default: window
  scrollableEl?: Window | HTMLElement

  // Higher quality image attributes to use on zoom
  zoomImg?: ImgHTMLAttributes<HTMLImageElement>

  // Offset in pixels the zoomed image should
  // be from the window's boundaries
  zoomMargin?: number
}
```

You can pass these options to only the `Controlled` component.

```typescript
export interface ControlledProps {
  // ...same as UncontrolledProps

  // Tell the component whether or not it should be zoomed
  // Default: false
  isZoomed: boolean

  // Listen for hints from the component about when you
  // should zoom (`true` value) or unzoom (`false` value)
  onZoomChange?: (value: boolean) => void
}
```

## Basic Usage

### Uncontrolled component (default)

Import the component and the CSS, wrap your image with the component, and the
component will handle it's own state.

```javascript
import React from 'react'
import Zoom from 'react-medium-image-zoom'
import 'react-medium-image-zoom/dist/styles.css'

// <img />
export const MyImg = () => (
  <Zoom>
    <img
      alt="That Wanaka Tree, New Zealand by Laura Smetsers"
      src="/path/to/thatwanakatree.jpg"
      width="500"
    />
  </Zoom>
)

// <div>
export const MyDiv = () => (
  <Zoom>
    <div
      aria-label="That Wanaka Tree, New Zealand by Laura Smetsers"
      role="img"
      style={{
        backgroundColor: '#fff',
        backgroundImage: `url("/path/to/thatwanakatree.jpg")`,
        backgroundPosition: '50%',
        backgroundRepeat: 'no-repeat',
        backgroundSize: 'cover',
        height: '0',
        paddingBottom: '56%',
        width: '100%',
      }}
    />
  </Zoom>
)

// <picture>
export const MyPicture = () => (
  <Zoom>
    <picture>
      <source media="(max-width: 800px)" srcSet="/path/to/teAraiPoint.jpg" />
      <img
        alt="A beautiful, serene setting in nature"
        src="/path/to/thatwanakatree.jpg"
        width="500"
      />
    </picture>
  </Zoom>
)

// <figure>
export const MyFigure = () => (
  <figure>
    <Zoom>
      <img
        alt="That Wanaka Tree, New Zealand by Laura Smetsers"
        src="/path/to/thatwanakatree.jpg"
        width="500"
      />
    </Zoom>
    <figcaption>Photo by Laura Smetsers</figcaption>
  </figure>
)
```

### Controlled component

Import the `Controlled` component and the CSS, wrap your image with the
component, and then dictate the `isZoomed` state to the component.

```javascript
import React, { useCallback, useState } from 'react'
import { Controlled as ControlledZoom } from 'react-medium-image-zoom'
import 'react-medium-image-zoom/dist/styles.css'

const MyComponent = () => {
  const [isZoomed, setIsZoomed] = useState(false)

  const handleZoomChange = useCallback(shouldZoom => {
    setIsZoomed(shouldZoom)
  }, [])

  return (
    <ControlledZoom isZoomed={isZoomed} onZoomChange={handleZoomChange}>
      <img
        alt="That wanaka tree, alone in the water near mountains"
        src="/path/to/thatwanakatree.jpg"
        width="500"
      />
    </ControlledZoom>
  )
)

export default MyComponent
```

The `onZoomChange` prop accepts a callback that will receive `true` or `false`
based on events that occur (like click or scroll events) to assist you in
determining when to zoom and unzoom the component.

## Styles

You can import the default styles from `react-medium-image-zoom/dist/styles.css`
and override the values from your code, or you can copy [the styles.css
file](./source/styles.css) and alter it to your liking. The latter is the best
option, given `rem`s should be used instead of `px` to account for different
default browser font sizes, but it's hard to guess at what these values should
be.

## Migrating From v4 to v5

TODO

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://robertwpearce.com"><img src="https://avatars2.githubusercontent.com/u/592876?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Robert Pearce</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=rpearce" title="Code">💻</a> <a href="#question-rpearce" title="Answering Questions">💬</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=rpearce" title="Tests">⚠️</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Arpearce" title="Bug reports">🐛</a> <a href="#example-rpearce" title="Examples">💡</a> <a href="#design-rpearce" title="Design">🎨</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Arpearce" title="Reviewed Pull Requests">👀</a> <a href="#ideas-rpearce" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=rpearce" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/cbothner"><img src="https://avatars1.githubusercontent.com/u/4642599?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Cameron Bothner</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=cbothner" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=cbothner" title="Documentation">📖</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Acbothner" title="Bug reports">🐛</a> <a href="#example-cbothner" title="Examples">💡</a> <a href="#ideas-cbothner" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Acbothner" title="Reviewed Pull Requests">👀</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=cbothner" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/jeremybini"><img src="https://avatars2.githubusercontent.com/u/12982155?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jeremy Bini</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=jeremybini" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Ajeremybini" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://ismaywolff.nl"><img src="https://avatars1.githubusercontent.com/u/7355199?v=4?s=100" width="100px;" alt=""/><br /><sub><b>ismay</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aismay" title="Bug reports">🐛</a> <a href="#ideas-ismay" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://www.qeek.co"><img src="https://avatars0.githubusercontent.com/u/220647?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Rajit Singh</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Arajit" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/rsaccon"><img src="https://avatars1.githubusercontent.com/u/16122?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Roberto Saccon</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Arsaccon" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/wtfdaemon"><img src="https://avatars0.githubusercontent.com/u/6598350?v=4?s=100" width="100px;" alt=""/><br /><sub><b>wtfdaemon</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Awtfdaemon" title="Bug reports">🐛</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://www.joshsloat.com"><img src="https://avatars1.githubusercontent.com/u/606159?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Josh Sloat</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Ajoshsloat" title="Bug reports">🐛</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=joshsloat" title="Code">💻</a> <a href="#example-joshsloat" title="Examples">💡</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Ajoshsloat" title="Reviewed Pull Requests">👀</a> <a href="#ideas-joshsloat" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=joshsloat" title="Documentation">📖</a> <a href="#design-joshsloat" title="Design">🎨</a> <a href="#question-joshsloat" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/aswinckr"><img src="https://avatars1.githubusercontent.com/u/5960217?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Aswin</b></sub></a><br /><a href="#question-aswinckr" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/alexshelkov"><img src="https://avatars3.githubusercontent.com/u/1233347?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alex Shelkovskiy</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aalexshelkov" title="Bug reports">🐛</a></td>
    <td align="center"><a href="http://adrian-design.com"><img src="https://avatars1.githubusercontent.com/u/7365629?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Adrian Bindiu</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3ASnowsoul" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/kendagriff"><img src="https://avatars3.githubusercontent.com/u/110935?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kendall Buchanan</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Akendagriff" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/HippoDippo"><img src="https://avatars2.githubusercontent.com/u/25674779?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kaycee</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=HippoDippo" title="Code">💻</a></td>
    <td align="center"><a href="http://shuffle.do"><img src="https://avatars2.githubusercontent.com/u/9633371?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Anuj</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aoyeanuj" title="Bug reports">🐛</a> <a href="#question-oyeanuj" title="Answering Questions">💬</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/ludwigfrank"><img src="https://avatars1.githubusercontent.com/u/10273946?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ludwig Frank</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aludwigfrank" title="Bug reports">🐛</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=ludwigfrank" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/serfgy"><img src="https://avatars2.githubusercontent.com/u/20569525?v=4?s=100" width="100px;" alt=""/><br /><sub><b>LX</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aserfgy" title="Bug reports">🐛</a> <a href="#ideas-serfgy" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="http://www.rosentomov.com"><img src="https://avatars3.githubusercontent.com/u/5452135?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Rosen Tomov</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3AbabyPrince" title="Bug reports">🐛</a></td>
    <td align="center"><a href="http://tommoor.com"><img src="https://avatars2.githubusercontent.com/u/380914?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tom Moor</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=tommoor" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Atommoor" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/jpreynat"><img src="https://avatars2.githubusercontent.com/u/7927876?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Johan Preynat</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=jpreynat" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Ajpreynat" title="Bug reports">🐛</a></td>
    <td align="center"><a href="http://rahulgaba.com"><img src="https://avatars3.githubusercontent.com/u/7898942?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Rahul Gaba</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=rgabs" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Argabs" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/spencerfdavis"><img src="https://avatars3.githubusercontent.com/u/1526292?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Spencer Davis</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=spencerfdavis" title="Code">💻</a> <a href="#ideas-spencerfdavis" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Aspencerfdavis" title="Reviewed Pull Requests">👀</a> <a href="#design-spencerfdavis" title="Design">🎨</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/dnlnvl"><img src="https://avatars2.githubusercontent.com/u/39607648?v=4?s=100" width="100px;" alt=""/><br /><sub><b>dnlnvl</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=dnlnvl" title="Code">💻</a></td>
    <td align="center"><a href="http://tomorrowstudio.co"><img src="https://avatars3.githubusercontent.com/u/6374876?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sean King</b></sub></a><br /><a href="#ideas-seaneking" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/0x6e6562"><img src="https://avatars3.githubusercontent.com/u/14088?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ben Hood</b></sub></a><br /><a href="#ideas-0x6e6562" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3A0x6e6562" title="Bug reports">🐛</a> <a href="#example-0x6e6562" title="Examples">💡</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3A0x6e6562" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="http://puthir.in"><img src="https://avatars2.githubusercontent.com/u/45002?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Navilan</b></sub></a><br /><a href="#ideas-navilan" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/13806"><img src="https://avatars1.githubusercontent.com/u/31736960?v=4?s=100" width="100px;" alt=""/><br /><sub><b>13806</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3A13806" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://www.twitter.com/deadcoder0904"><img src="https://avatars1.githubusercontent.com/u/16436270?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Akshay Kadam (A2K)</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Adeadcoder0904" title="Bug reports">🐛</a> <a href="#ideas-deadcoder0904" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/xerona"><img src="https://avatars0.githubusercontent.com/u/8929085?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jake Stewart</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Axerona" title="Bug reports">🐛</a> <a href="#ideas-xerona" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/henrych4"><img src="https://avatars0.githubusercontent.com/u/19466940?v=4?s=100" width="100px;" alt=""/><br /><sub><b>hhh</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Ahenrych4" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/davalapar"><img src="https://avatars0.githubusercontent.com/u/41451953?v=4?s=100" width="100px;" alt=""/><br /><sub><b>@davalapar</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Adavalapar" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://sunknudsen.com"><img src="https://avatars3.githubusercontent.com/u/2117655?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sun Knudsen</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=sunknudsen" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Asunknudsen" title="Bug reports">🐛</a> <a href="#ideas-sunknudsen" title="Ideas, Planning, & Feedback">🤔</a> <a href="#example-sunknudsen" title="Examples">💡</a> <a href="#question-sunknudsen" title="Answering Questions">💬</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Asunknudsen" title="Reviewed Pull Requests">👀</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=sunknudsen" title="Tests">⚠️</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=sunknudsen" title="Documentation">📖</a></td>
    <td align="center"><a href="http://dougg0k.js.org"><img src="https://avatars3.githubusercontent.com/u/10801221?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Douglas Galdino</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=dougg0k" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=dougg0k" title="Documentation">📖</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Adougg0k" title="Bug reports">🐛</a> <a href="#ideas-dougg0k" title="Ideas, Planning, & Feedback">🤔</a> <a href="#example-dougg0k" title="Examples">💡</a> <a href="https://github.com/rpearce/react-medium-image-zoom/pulls?q=is%3Apr+reviewed-by%3Adougg0k" title="Reviewed Pull Requests">👀</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=dougg0k" title="Tests">⚠️</a></td>
    <td align="center"><a href="http://mohammedfaragallah.herokuapp.com"><img src="https://avatars0.githubusercontent.com/u/14910456?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mohammed Faragallah</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3AMohammedFaragallah" title="Bug reports">🐛</a> <a href="#ideas-MohammedFaragallah" title="Ideas, Planning, & Feedback">🤔</a> <a href="#example-MohammedFaragallah" title="Examples">💡</a></td>
    <td align="center"><a href="http://rokoroku.github.io"><img src="https://avatars1.githubusercontent.com/u/5208632?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Youngrok Kim</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=rokoroku" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Arokoroku" title="Bug reports">🐛</a></td>
    <td align="center"><a href="http://nandhae.me"><img src="https://avatars1.githubusercontent.com/u/11366094?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nandhagopal Ezhilmaran</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Anandhae" title="Bug reports">🐛</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://equinusocio.dev"><img src="https://avatars.githubusercontent.com/u/10454741?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mattia Astorino</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aequinusocio" title="Bug reports">🐛</a></td>
    <td align="center"><a href="http://valtism.com"><img src="https://avatars.githubusercontent.com/u/1286001?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dan Wood</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=valtism" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/zacherygentry"><img src="https://avatars.githubusercontent.com/u/14227467?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Zachery C Gentry</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Azacherygentry" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://xmflsct.com/"><img src="https://avatars.githubusercontent.com/u/292204?v=4?s=100" width="100px;" alt=""/><br /><sub><b>xmflsct</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Axmflsct" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://supwill.dev/"><img src="https://avatars.githubusercontent.com/u/15272175?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Will.iam</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=iamwill123" title="Code">💻</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=iamwill123" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://Gourav.io"><img src="https://avatars.githubusercontent.com/u/7106086?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Gourav Goyal</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=GorvGoyl" title="Documentation">📖</a></td>
    <td align="center"><a href="https://joshcena.com/"><img src="https://avatars.githubusercontent.com/u/55398995?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Joshua Chen</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3AJosh-Cena" title="Bug reports">🐛</a> <a href="https://github.com/rpearce/react-medium-image-zoom/commits?author=Josh-Cena" title="Code">💻</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/edlerd"><img src="https://avatars.githubusercontent.com/u/1155472?v=4?s=100" width="100px;" alt=""/><br /><sub><b>David Edler</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Aedlerd" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://rikusen.dev/"><img src="https://avatars.githubusercontent.com/u/19174234?v=4?s=100" width="100px;" alt=""/><br /><sub><b>rikusen0335</b></sub></a><br /><a href="#ideas-rikusen0335" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="http://surjithctly.in/"><img src="https://avatars.githubusercontent.com/u/1884712?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Surjith S M</b></sub></a><br /><a href="#ideas-surjithctly" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/developergunny"><img src="https://avatars.githubusercontent.com/u/67149898?v=4?s=100" width="100px;" alt=""/><br /><sub><b>developergunny</b></sub></a><br /><a href="https://github.com/rpearce/react-medium-image-zoom/issues?q=author%3Adevelopergunny" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://uxdkhan.com/"><img src="https://avatars.githubusercontent.com/u/6104751?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Khan Mohsin</b></sub></a><br /><a href="#question-m90khan" title="Answering Questions">💬</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!
