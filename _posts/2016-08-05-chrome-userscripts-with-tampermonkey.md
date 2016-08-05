I always want github's editor to default to using soft-wrap. To that end, I finally looked into chrome userscripts. TamperMonkey seems to be the "normal" way to edit and manage these scripts.

Here's some essential links for TamperMonkey:

* [Tampermonkey - Chrome Web Store](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en)
* [Tampermonkey • FAQ](https://tampermonkey.net/faq.php?ext=dhdg)
* [Tampermonkey Tutorial - hibbard.eu](http://hibbard.eu/tampermonkey-tutorial/)

Here's what I have so far.

```
// ==UserScript==
// @name         github editor softwrap
// @namespace    http://matty.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://github.com/*
// @grant        none
// ==/UserScript==

(function() {
  'use strict';

  // Your code here...
  var wrapmode = document.querySelector('select.form-select.select-sm.js-code-wrap-mode');
  wrapmode.value = 'on';
})();
```

Unfortunately, changing the valueof the select box doesn't actually alter the behavior of the text area.
