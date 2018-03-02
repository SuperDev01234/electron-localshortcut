# electron-localshortcut

A module to register/unregister a keyboard shortcut
locally to a BrowserWindow instance, without using a Menu.

This is built to circumvent [this Electron issue](https://github.com/atom/electron/issues/1334).

**Note:** Since this module internally use `global-shortcut` native module, you should not use it until the `ready` event of the app module is emitted. See [electron docs](http://electron.atom.io/docs/latest/api/global-shortcut/).

[![Travis Build Status](https://img.shields.io/travis/parro-it/electron-localshortcut/master.svg)](http://travis-ci.org/parro-it/electron-localshortcut)
[![NPM module](https://img.shields.io/npm/v/electron-localshortcut.svg)](https://npmjs.org/package/electron-localshortcut)
[![NPM downloads](https://img.shields.io/npm/dt/electron-localshortcut.svg)](https://npmjs.org/package/electron-localshortcut)
[![Greenkeeper badge](https://badges.greenkeeper.io/parro-it/electron-localshortcut.svg)](https://greenkeeper.io/)

# Installation

```bash
npm install --save electron-localshortcut
```

# Usage

```javascript
	const electronLocalshortcut = require('electron-localshortcut');
	const BrowserWindow = require('electron').BrowserWindow;

	const win = new BrowserWindow();
	win.loadUrl('https://github.com');
	win.show();

	electronLocalshortcut.register(win, 'Ctrl+A', () => {
		console.log('You pressed ctrl & A');
	});

	electronLocalshortcut.register(win, 'Ctrl+B', () => {
		console.log('You pressed ctrl & B');
	});

	electronLocalshortcut.register(win, ['Ctrl+R', 'F5'], () => {
        console.log('You pressed ctrl & R or F5');
    });

	console.log(
		electronLocalshortcut.isRegistered(win, 'Ctrl+A')
	);      // true

	electronLocalshortcut.unregister(win, 'Ctrl+A');
	electronLocalshortcut.unregisterAll(win);
```

# App shortcuts.

If you omit the window argument of `isRegistered`, `unregisterAll`, `unregister` and `register` methods, the shortcut is registered as an app shortcut.
It is active when any window of the app is focused.

They differ from native [global-shortcuts](https://github.com/atom/electron/blob/master/docs/api/global-shortcut.md) because they doesn't interfere with other apps running on the same machine.

# Shortcut behaviour.

If you register a shortcut for a window, this module unregister the shortcut when the window is hidden, unfocused or minimized, and automatically restore them when the window is restored and focused again.

If you register an app shortcut, this module unregister the shortcut when all windows of your app are hidden, unfocused or minimized, and automatically restore it when any window of your app is restored and focused again.

# API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

## disableAll

Disable all of the shortcuts registered on the BrowserWindow instance.
Registered shortcuts no more works on the `window` instance, but the module
keep a reference on them. You can reactivate them later by calling `enableAll`
method on the same window instance.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance

Returns **[Undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

## enableAll

Enable all of the shortcuts registered on the BrowserWindow instance that
you had previously disabled calling `disableAll` method.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance

Returns **[Undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

## unregisterAll

Unregisters all of the shortcuts registered on any focused BrowserWindow
instance. This method does not unregister any shortcut you registered on
a particular window instance.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance

Returns **[Undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

## register

Registers the shortcut `accelerator`on the BrowserWindow instance.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance to register.
    This argument could be omitted, in this case the function register
    the shortcut on all app windows.
-   `accelerator` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>)** the shortcut to register
-   `callback` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** This function is called when the shortcut is pressed
    and the window is focused and not minimized.

Returns **[Undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

## unregister

Unregisters the shortcut of `accelerator` registered on the BrowserWindow instance.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance to unregister.
    This argument could be omitted, in this case the function unregister the shortcut
    on all app windows. If you registered the shortcut on a particular window instance, it will do nothing.
-   `accelerator` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>)** the shortcut to unregister

Returns **[Undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)**

## isRegistered

Returns `true` or `false` depending on whether the shortcut `accelerator`
is registered on `window`.

**Parameters**

-   `win` **BrowserWindow** BrowserWindow instance to check. This argument
    could be omitted, in this case the function returns whether the shortcut
    `accelerator` is registered on all app windows. If you registered the
    shortcut on a particular window instance, it return false.
-   `accelerator` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the shortcut to check

Returns **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** if the shortcut `accelerator` is registered on `window`.

# License

The MIT License (MIT)

Copyright (c) 2017 Andrea Parodi
