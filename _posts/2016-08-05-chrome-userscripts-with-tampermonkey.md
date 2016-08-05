I always want github's editor to default to using soft-wrap. To that end, I finally looked into chrome userscripts. TamperMonkey seems to be the "normal" way to edit and manage these scripts.

Here's some essential links for TamperMonkey:

* [Tampermonkey - Chrome Web Store](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en)
* [Tampermonkey • FAQ](https://tampermonkey.net/faq.php?ext=dhdg)
* [Tampermonkey Tutorial - hibbard.eu](http://hibbard.eu/tampermonkey-tutorial/)

Here's what I have so far. Seems to reliably do what it's supposed to do.

```
// ==UserScript==
// @name         github-editor-softwrap
// @namespace    http://matty.net/
// @version      0.1
// @description  Set the github.com editor's wrap mode default to soft-wrap.
// @author       Matty Maloney
// @match        https://github.com/*
// @grant        none
// @require      none
// ==/UserScript==

(function() {
  'use strict';
  var wrapmode = window.wrapmode = document.querySelector('select.form-select.select-sm.js-code-wrap-mode');
  wrapmode.value = 'on';
  wrapmode.dispatchEvent(new Event('change', { bubbles: true, cancelable: true }));
})();
```
