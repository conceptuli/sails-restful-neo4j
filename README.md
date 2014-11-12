

# Restful Neo4j 2.1+

Sails.js Waterline adapter for the Neo4j restful apis

## Installation

Install from NPM.

```bash
$ npm install sails-restful-neo4j
```

## Compatibility

This is compatible with sails 0.10 and above

## Sails Configuration

Add the following config to the config/connections.js file:

```javascript
module.exports.connections = {

  default: 'restful-neo4j',

  restful-neo4j: {
    module: 'sails-restful-neo4j',
    type: 'json',             // expected response type (json | string | http)
    host: 'foo.myneo4j.com', // api host
    port: 7474,                 // api port
    protocol: 'http',         // HTTP protocol (http | https)
    rejectUnauthorized: true, // prevent https connections that use a self-signed certificate
    pathname: '/db/data'       // base api path
    resource: null,           // resource path to use (overrides model name)
    action: null,             // action to use for the given resource ([resource]/run)
    query: {},                // query parameters to provide with all GET requests
    methods: {    
    
                              // overrides default HTTP methods used for each CRUD action
     //BASIC
      create: 'post',
      find: 'get',
      update: 'put',
      destroy: 'del',
      
      // Lets get 'nodey'
      transaction: 'post',
      root: 'get',
      getAllKeysGlobal: 'get',
      createNode: 'post',
      createNodeWithProps: 'post',
      getNode: 'get',
      getFakeNode: 'get', //expect a '404' returned
      deleteNode: 'del',
      getRelById: 'get',
      createRel: 'post',
      createRelWithProps: 'post',
      remove_relationship: 'del',
      remove_relationship_properties: 'del', //this deletes the relationship properties, not a relationship WITH properties
      set_all_the_props_on_the_rel: 'put',
      get_rel_with_single_property: 'get',
      get_all_the_rels_for_this_node: ' get',
      get_all_the_inbound_rels: 'get',
      get_all_the_outbound_rels: 'get',
      get_me_rels_with_the_type: 'get', //get a single typed relationship for a node i.e. LOVES
      get_me_rel_with_these_types: 'get', //same as above but multiple = use the '%26' instead of '&' to join LOVES&HATES
      show_me_every_type_of_rel: 'get',
      set_this_node_property: 'put',
      update_these_node_properties: 'put',
      get_the_properties_for_this_node: 'get',
      delete_the_properties_from_this_node: 'del',
      delete_the_node_property: 'del',
      
      
      //##//relattionship property management
      
      update_relProperties: 'put',
      remove_relProperties: 'del',
      remove_the_relProperty: 'del,
      add_nodeLabel: 'post',
      add_nodeLabels: 'post',
      change_nodeLabel: 'put',
      change_nodeLabels: 'put',
      drop_nodeLabel: 'del',
      list_nodeLabls: 'get',
      getNodes_withLabel: 'get',
      getNodes_withLabel_andProperty: 'get',
      showMe_allLabelsGlobal: 'get',
      
      //Index stuff
      
      createIndex_onProperty: 'post',
      showMe_allIndexes_forLabel: 'get',
      dropIndex: 'del',
      
      
      //Contstraints and uniques
      
      createUnique_onLabel_onProperty: 'post',
      getUnique_onLabel_onProperty: 'get',
      getAllUniques_onLabel: 'get',
      getAllUniques_global: 'get',
      dropUnique_onLabel_onProperty: 'del',
      
      
      //traversals
      
      walkTheGraph_goWide_fromNode: 'post',
      walkTheGraph_goWide_fromNode_andReturn: 'post',
      walkTheGraph_goWide_fromNode_andReturn_withTheMap: 'post',
      
      
      walkTheGraph_startDeep_fromNode_andReturn_withTheMap: 'post',
      
      walkTheGraph_goWide_fromNode_andReturn_withTheBook: 'post',  //paged traversal
      
      read_theBook: 'get',  //read the returned paged traversal
      write_theBook_withPageSize: 'post',  //set the size of the page DEFAULT is 50
      
      write_thBook_withTimer: 'post'
      
      //path finding
      
      writeTheMap_andKeepItShort: 'post',
      
      writeTheMap_andKeeptItShort_andSweet: 'post', //find ONE OF the shortest paths
      
      writeTheMap_withAPrice: 'post', //Dijkstra algo execution
      
      writeTheMap_withAPrice_CommunistStyle: 'post', //Dijkstra algo even weights
      
      writeTheMap_withAPrice_ChooseYourOwnAdventure: 'post', //Dijkstra multi path
      
      
      
      
      
    },
    beforeFormatResult: function(result){return result},    // alter result prior to formatting
    afterFormatResult: function(result){return result},     // alter result after formatting
    beforeFormatResults: function(results){return results}, // alter results prior to formatting
    afterFormatResults: function(results){return results},  // alter results after formatting
    cache: {                  // optional cache engine
      engine : require('someCacheEngine')
    }
  }

};
```

## Caching

To cache API responses, you can add an object to the adapter's configuration with an attribute named `engine` that responds
to the following methods:

* `get(key)` - get cache key
* `set(key, value)` - set value on key
* `del(key)` - delete value identified by `key` from cache

An example caching configuration using LRU cache (`npm install --save lru-cache`) will look something like:

```javascript
// under config/cache.js

var LRU = require("lru-cache"),
    options = {
      max: 500,
      maxAge: 1000 * 10
    },
    cache = LRU(options);
module.exports = cache;

// under config/adapters.js

  rest: {
    module   : 'sails-rest',
    cache    : {
      engine : require('./cache')
    }
  }
```

Cache keys are computed from the API request URLs. This means that each unique URL will have its own cache key.

At the moment, cache busting is done by exact key only. This means that if you fetch `/users` and then update
`/users/1`, the cached objects under `/users` will not be purged to reflect your changes.

## TODO

* Improve cache busting
* Add support for async cache engines (eg. redis_client)

## Change Log

### v0.0.3

## Contributors

[Christopher M. Mitchell](https://github.com/divThis)

## MIT License

Copyright (c) 2014 Brian Bagdasarian - Conceptu.li

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
