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

`.index` callback must return a list of data elements.
Each data element represents an RSS item with custom data that will be
passed to `.item` callback for details generation.

Fully raw items:

```javascript
app('/feed').index(function(){
  return ['data1', 'data2'];
}).item(function(data){
  return {
    title: data,
    text: 'This is an article with data ' + data
  };
});
```


