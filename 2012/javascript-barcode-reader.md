# JavaScript Barcode Reader

TL;DR [barcode.js](https://github.com/fabian/barcode.js) is a proof-of-concept for a barcode reader in JavaScript which uses Canvas and the File API for decoding the image inside the browser. [Demo Barcode Reader](http://fabian.github.com/barcode.js/).

The long-awaited arrival of file uploads in Safari on iOS 6 opens the possibility for instant photo processing within a mobile website, especially since the new HTML5 [File API] is available as well. It does not only allow you to upload a photo to a website but also lets you scan the photo with JavaScript.

So as some of our clients deal with retail products with usually poses a EAN-13 barcode, it would be interesting to search a product simply by taking a photo of it — without leaving the website. So during the last Hackday I set out to implement a Barcode Reader in pure JavaScript.


