---
published: true
layout: post
title: Silently Printing Our Packing Slips
---

Preferences created or changed for Firfox 3.6.9 to enable auto login and silent printing.

In `about:config`, I added these 2 properties:

* `network.http.phishy-userpass-length`, integer, 255
* `print.always_print_silent`, boolean, true

---

Printing:

* [web applications - Automatically Print Image from Website - Stack Overflow](http://stackoverflow.com/questions/945691/automatically-print-image-from-website/27432677#27432677)
* [Seamless Print :: Add-ons for Firefox](https://addons.mozilla.org/en-US/firefox/addon/seamless-print/) (requires firefox v19+)
* [AttendPrint :: Add-ons for Firefox](https://addons.mozilla.org/en-US/firefox/addon/attendprint/) (requires firefox 3+)
* [web applications - Automatically Print Image from Website - Stack Overflow](http://stackoverflow.com/questions/945691/automatically-print-image-from-website)

Auth:

* [testing - HTTP Basic Auth via URL in Firefox does not work? - Stack Overflow](http://stackoverflow.com/questions/3021602/http-basic-auth-via-url-in-firefox-does-not-work)
* [Alek's Blog: Selenium WebDriver tips](http://aleetesting.blogspot.com/2011/10/selenium-webdriver-tips.html)
* [http - Using in-url basic authentication in firefox - Stack Overflow](http://stackoverflow.com/questions/2848287/using-in-url-basic-authentication-in-firefox)
* [java - How to perform Basic Authentication for FirefoxDriver, ChromeDriver and IEdriver in Selenium WebDriver? - Stack Overflow](http://stackoverflow.com/questions/5672407/how-to-perform-basic-authentication-for-firefoxdriver-chromedriver-and-iedriver)

---

Other changes:

app.update.enabled, false
extensions.update.enabled, false
browser.search.update, false
security.warn_viewing_mixed, false

I didn't uncheck "Block pop-up windows", although I had expected I'd need to do that.

Printing:

Margins:

* top: 0.4in
* left: 0.4in
* right: 0.2in
* bottom: 0.2in

Check box to print background images and colors.

Remove all the header and footer components.
