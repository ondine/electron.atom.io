---
version: v1.4.2
category: API
redirect_from:
    - /docs/v0.24.0/api/frameless-window/
    - /docs/v0.25.0/api/frameless-window/
    - /docs/v0.26.0/api/frameless-window/
    - /docs/v0.27.0/api/frameless-window/
    - /docs/v0.28.0/api/frameless-window/
    - /docs/v0.29.0/api/frameless-window/
    - /docs/v0.30.0/api/frameless-window/
    - /docs/v0.31.0/api/frameless-window/
    - /docs/v0.32.0/api/frameless-window/
    - /docs/v0.33.0/api/frameless-window/
    - /docs/v0.34.0/api/frameless-window/
    - /docs/v0.35.0/api/frameless-window/
    - /docs/v0.36.0/api/frameless-window/
    - /docs/v0.36.3/api/frameless-window/
    - /docs/v0.36.4/api/frameless-window/
    - /docs/v0.36.5/api/frameless-window/
    - /docs/v0.36.6/api/frameless-window/
    - /docs/v0.36.7/api/frameless-window/
    - /docs/v0.36.8/api/frameless-window/
    - /docs/v0.36.9/api/frameless-window/
    - /docs/v0.36.10/api/frameless-window/
    - /docs/v0.36.11/api/frameless-window/
    - /docs/v0.37.0/api/frameless-window/
    - /docs/v0.37.1/api/frameless-window/
    - /docs/v0.37.2/api/frameless-window/
    - /docs/v0.37.3/api/frameless-window/
    - /docs/v0.37.4/api/frameless-window/
    - /docs/v0.37.5/api/frameless-window/
    - /docs/v0.37.6/api/frameless-window/
    - /docs/v0.37.7/api/frameless-window/
    - /docs/v0.37.8/api/frameless-window/
    - /docs/latest/api/frameless-window/
source_url: 'https://github.com/electron/electron/blob/master/docs/api/frameless-window.md'
excerpt: "Open a window without toolbars, borders, or other graphical &quot;chrome&quot;."
title: "Frameless Window"
sort_title: "frameless window"
---

# Frameless Window

> Open a window without toolbars, borders, or other graphical "chrome".

A frameless window is a window that has no
[chrome](https://developer.mozilla.org/en-US/docs/Glossary/Chrome), the parts of
the window, like toolbars, that are not a part of the web page. These are
options on the [`BrowserWindow`](http://electron.atom.io/docs/api/browser-window) class.

## Create a frameless window

To create a frameless window, you need to set `frame` to `false` in
[BrowserWindow](http://electron.atom.io/docs/api/browser-window)'s `options`:


```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({width: 800, height: 600, frame: false})
win.show()
```

### Alternatives on macOS

On macOS 10.9 Mavericks and newer, there's an alternative way to specify
a chromeless window. Instead of setting `frame` to `false` which disables
both the titlebar and window controls, you may want to have the title bar
hidden and your content extend to the full window size, yet still preserve
the window controls ("traffic lights") for standard window actions.
You can do so by specifying the new `titleBarStyle` option:

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({titleBarStyle: 'hidden'})
win.show()
```

## Transparent window

By setting the `transparent` option to `true`, you can also make the frameless
window transparent:

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({transparent: true, frame: false})
win.show()
```

### Limitations

* You can not click through the transparent area. We are going to introduce an
  API to set window shape to solve this, see
  [our issue](https://github.com/electron/electron/issues/1335) for details.
* Transparent windows are not resizable. Setting `resizable` to `true` may make
  a transparent window stop working on some platforms.
* The `blur` filter only applies to the web page, so there is no way to apply
  blur effect to the content below the window (i.e. other applications open on
  the user's system).
* On Windows operating systems, transparent windows will not work when DWM is
  disabled.
* On Linux users have to put `--enable-transparent-visuals --disable-gpu` in
  the command line to disable GPU and allow ARGB to make transparent window,
  this is caused by an upstream bug that [alpha channel doesn't work on some
  NVidia drivers](https://code.google.com/p/chromium/issues/detail?id=369209) on
  Linux.
* On Mac the native window shadow will not be shown on a transparent window.

## Click-through window

To create a click-through window, i.e. making the window ignore all mouse
events, you can call the [win.setIgnoreMouseEvents(ignore)][ignore-mouse-events]
API:

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()
win.setIgnoreMouseEvents(true)
```

## Draggable region

By default, the frameless window is non-draggable. Apps need to specify
`-webkit-app-region: drag` in CSS to tell Electron which regions are draggable
(like the OS's standard titlebar), and apps can also use
`-webkit-app-region: no-drag` to exclude the non-draggable area from the
 draggable region. Note that only rectangular shapes are currently supported.

To make the whole window draggable, you can add `-webkit-app-region: drag` as
`body`'s style:

```html
<body style="-webkit-app-region: drag">
</body>
```

And note that if you have made the whole window draggable, you must also mark
buttons as non-draggable, otherwise it would be impossible for users to click on
them:

```css
button {
  -webkit-app-region: no-drag;
}
```

If you're setting just a custom titlebar as draggable, you also need to make all
buttons in titlebar non-draggable.

## Text selection

In a frameless window the dragging behaviour may conflict with selecting text.
For example, when you drag the titlebar you may accidentally select the text on
the titlebar. To prevent this, you need to disable text selection within a
draggable area like this:

```css
.titlebar {
  -webkit-user-select: none;
  -webkit-app-region: drag;
}
```

## Context menu

On some platforms, the draggable area will be treated as a non-client frame, so
when you right click on it a system menu will pop up. To make the context menu
behave correctly on all platforms you should never use a custom context menu on
draggable areas.

[ignore-mouse-events]: http://electron.atom.io/docs/api/browser-window#winsetignoremouseeventsignore
