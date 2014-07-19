html-to-rss
===========

Scrape HTML and generate RSS with no extra effort.
Host with Node.js. No database. No HTTP headers. No worries.

This library is at the design stage.

## API design draft

### Base structure
The following creates an HTTP server that listens on port 3000
and serves RSS feed at `/feed` with items as configured.

```javascript
var app = require('html-to-rss')();

// Create a feed and configure it
var feed = app('/feed');
feed.index(function(){});
feed.item(function(){});

app.listen(3000);
```

### Configuring items

`.index` callback must return a list of feed elements.
If `.item` callback is provided, each data element will be
passed to `.item` callback for details generation.

All callback may return promises instead of values.

Full generation upfront:

```javascript
app('/feed').index(function(){
  return [
    { title: 'title1', html: 'html1' },
    { title: 'title2', html: 'html2' }
  ];
});
```

Fully raw per-item:

```javascript
app('/feed').index(function(){
  return ['data1', 'data2'];
}).item(function(data){
  return {
    title: data,
    html: 'This is an article with data ' + data
  };
});
```

Fully friendly items:

```javascript
app('/feed').index('http://example.com/articles', function($){
  return $('.article h1 a');
}).item(function($){
  return {
    title: $('h1'),
    html: $('.text')
  };
});
```

