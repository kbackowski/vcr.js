# VCR.js

Record XMLHttpRequest calls and saves them using localStorage or files if using
Nodejs.
It's a js implementation of myronmarston's [VCR](https://github.com/myronmarston/vcr)
but for javasccript without any dependencies

```bash
$ npm install vcr
```

## How to use it

I try to make it as similar to original VCR as possible.
Using [Gerbil](http://github.com/elcuervo/gerbil) :P it's something like this:

```javascript
scenario("Ajax interception", {
  'setup': function() {
    VCR.configure(function(c) {
      c.hookInto = window.XMLHttpRequest;
    });
  },

  'Recording ajax request': function(g) {
    VCR.useCassette('test', function(v) {
      XMLHttpRequest = v.XMLHttpRequest;

      var makeRequest = function() {
        var ajax = new XMLHttpRequest();

        ajax.open('GET', 'test.html');
        ajax.onreadystatechange = function() {
          if(ajax.readyState === 4) {
            g.assertEqual("Hello World!\n", ajax.responseText);
          }
        };
        ajax.send(null);
      }
      // Record First Request
      makeRequest();

      // Wait for it...
      g.setTimeout(function() { makeRequest(); }, 100);
    });
  }
});
```

## What will happend?

If you are using nodejs .json files will be created as cassetes to reproduce
afterwards. In the other hand if you are running it in a browser localStorage
will be used to persist the recordings.