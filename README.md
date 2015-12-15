# Unrest <small>a vanilla javascript rest api</small>

A client side rest api with cache that just works as you'd expect.

## Installation
```sh
bower install --save --production http://edcgit/developers/javascript-unrest.git
# eventually you'll be able to do.
bower install -Sp unrest
```

## Usage
```javascript
var db = new DB('/api');
// sync should begin fetching the entire database, until the database is synced
// all requests will be stalled.

// List querying
// GET /api/TableName
db('TableName')
  .query({name: 'Evan%'})
  .cacheable()
  .then((data, cache) => {
    // function will run twice. Once for cache with cache=true, then again when
    //  the live data comes in, with cache=false
  }).catch(err => {
    // err: String, server error
    // cache: Object, the old object
  })

// Single query
// GET /api/TableName/53
db('TableName')
  .fetch(53)
  .cacheable()
  .then((item, cache) => {
    // if the cached data-structure has a field named 'id' it will use that
    // field, otherwise it will not be triggered. This function will be triggered
    // again with cache=false when the real date comes down the pipe.
  }).catch(err => {

  })

// Save item
// POST/PUT /api/TableName(/53)?
data = { Id: 53, Name: 'Evan' }
db('TableName').save(data)
  .then(...)
  .catch(...);

db('TableName').remove(53)
  .then(...).catch(...)

// Live querying with Signlr
// NOT IMPLEMENTED
db('Table')
  .on('insert', (new, data) => {
    // insert into template $scope.model
  }).on('delete', (removed, data) => {
    // remove item
  }).on('update', (changed, data) => {
    // splice model
  });
```

## Building

#### Build Minified
    npm run build;

#### Build <small>(all-the-time)</small>; Test <small>(all-the-time)</small>
```sh
npm install; gulp;
# I currently don't test all the time...
gulp test # for that.
```
