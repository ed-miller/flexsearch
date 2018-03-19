<p align="center">
    <br>
    <img src="http://nextapps.de/img/flexsearch.svg" alt="Search Library" width="50%">
    <br><br>
    <a target="_blank" href="https://www.npmjs.com/package/flexsearch"><img src="https://img.shields.io/npm/v/flexsearch.svg"></a>
    <img src="https://img.shields.io/badge/status-BETA-orange.svg">
    <a target="_blank" href="https://travis-ci.org/nextapps-de/flexsearch"><img src="https://travis-ci.org/nextapps-de/flexsearch.svg?branch=master"></a>
    <a target="_blank" href="https://coveralls.io/github/nextapps-de/flexsearch?branch=master"><img src="https://coveralls.io/repos/github/nextapps-de/flexsearch/badge.svg?branch=master"></a>
    <a target="_blank" href="https://github.com/nextapps-de/flexsearch/issues"><img src="https://img.shields.io/github/issues/nextapps-de/xone.svg"></a>
    <img src="https://badges.greenkeeper.io/nextapps-de/flexsearch.svg">
    <a target="_blank" href="https://github.com/nextapps-de/flexsearch/blob/master/LICENSE.md"><img src="https://img.shields.io/npm/l/xone.svg"></a>
</p>

<h1></h1>
<h3>World's fastest and most memory efficient full text search library with zero dependencies.</h3>

When it comes to raw search speed <a href="https://jsperf.com/compare-search-libraries" target="_blank">FlexSearch outperforms every single searching library out there</a> and also provides flexible search capabilities like multi-word matching, phonetic transformations or partial matching. It also has the __most memory-efficient index__. Keep in mind that updating existing items from the index has a significant cost. When your index needs to be updated continuously then <a href="bulksearch/" target="_blank">BulkSearch</a> may be a better choice. FlexSearch also provides you a non-blocking asynchronous processing model as well as web workers to perform any updates on the index as well as queries through dedicated threads. 

Benchmark:
- Library Comparison: <a href="https://jsperf.com/compare-search-libraries" target="_blank">https://jsperf.com/compare-search-libraries</a>
- BulkSearch vs. FlexSearch: <a href="https://jsperf.com/flexsearch" target="_blank">https://jsperf.com/flexsearch</a>

Supported Platforms:
- Browser
- Node.js

Supported Module Definitions:
- AMD (RequireJS)
- CommonJS (Node.js)
- Closure (Xone)
- Global (Browser)

All Features:
<ul>
    <li><a href="#webworker">Web-Worker Support</a> (not available in Node.js)</li>
    <li>Partial Matching</li>
    <li>Multiple Words</li>
    <li><a href="#phonetic">Phonetic Search</a></li>
    <li>Relevance-based Scoring</li>
    <li>Contextual Indexes</li>
    <li>Limit Results</li>
    <li>Caching</li>
    <li>Asynchronous Mode</li>
    <li>Custom Matchers</li>
    <li>Custom Encoders</li>
</ul>

<a name="contextual"></a>
#### Contextual Search

FlexSearch introduce a new scoring mechanism called __Contextual Search__ which was invented by Thomas Wilkerling, the author of this library. A Contextual Search __incredibly boost up queries to a complete new level__.
The basic idea of this concept is to limit relevance by its context instead of calculating relevance through the whole (unlimited) distance.
Imagine you add a text block of some sentences to an index ID. Assuming the query includes a combination of first and last word from this text block, are they really relevant to each other?
In this way contextual search also improves the results of relevance-based queries on large amount of text data.

<p align="center">
    <img src="https://rawgithub.com/nextapps-de/flexsearch/master/contextual_index.svg">
</p>

__Note:__ This feature is actually not enabled by default.

<a name="webworker"></a>
#### Web-Worker Support

Workers get its own dedicated memory. Especially for larger indexes, web worker improves speed and available memory a lot. FlexSearch index was tested with a 250 Mb text file including 10 Million words. The indexing was done silently in background by multiple parallel running workers in about 7 minutes. The final index reserves ~ 8.2 Mb memory/space. The search result took ~ 0.25 ms.

__Note:__ It is slightly faster to use no web worker when the index or query isn't too big (index < 500,000 words, query < 25 words).

#### Compare BulkSearch vs. FlexSearch

<table>
    <tr></tr>
    <tr>
        <td align="left">Description</th>
        <td align="left">BulkSearch</th>
        <td align="left">FlexSearch</th>
    </tr>
    <tr>
        <td>Access</td>
        <td>Read-Write optimized index</td>
        <td>Read-Memory optimized index</td>
    </tr>
    <tr></tr>
    <tr>
        <td>Memory</td>
        <td>Large: ~ 5 Mb per 100,000 words</td>
        <td>Tiny: ~ 100 Kb per 100,000 words</td>
    </tr>
    <tr></tr>
    <tr>
        <td>Usecase</td>
        <td><ul><li>Limited content</li><li>Use when existing items of the index needs to be updated continously (update, remove)</li><li>Supports pagination</li></ul></td>
        <td><ul><li>Fastest possible search</li><li>Existing items of the index does not need to be updated continously (update, remove)</li><li>Max out memory capabilities</li></ul></td>
    </tr>
    <tr></tr>
    <tr>
        <td>Pagination</td>
        <td>Yes</td>
        <td>No</td>
    </tr>
    <tr></tr>
    <tr>
        <td>Ranked Searching</td>
        <td>No</td>
        <td>Yes</td>
    </tr>
    <tr></tr>
    <tr>
        <td>Contextual Index</td>
        <td>No</td>
        <td>Yes</td>
    </tr>
    <tr></tr>
    <tr>
        <td>WebWorker</td>
        <td>No</td>
        <td>Yes</td>
    </tr>
</table>

## Installation

##### HTML / Javascript

```html
<html>
<head>
    <script src="js/flexsearch.min.js"></script>
</head>
...
```
__Note:__ Use _flexsearch.min.js_ for production and _flexsearch.js_ for development.

Use latest from CDN:
```html
<script src="https://cdn.rawgit.com/nextapps-de/flexsearch/master/flexsearch.min.js"></script>
```

##### Node.js

```npm
npm install flexsearch
```

In your code include as follows:

```javascript
var FlexSearch = require("flexsearch");
```

Or pass in options when requiring:

```javascript
var index = require("flexsearch").create({/* options */});
```

__AMD__

```javascript
var FlexSearch = require("./flexsearch.js");
```

## API Overview

Global methods:
- <a href="#flexsearch.create">FlexSearch.__create__(\<options\>)</a>
- <a href="#flexsearch.addmatcher">FlexSearch.__addMatcher__({_KEY: VALUE_})</a>
- <a href="#flexsearch.register">FlexSearch.__register__(name, encoder)</a>
- <a href="#flexsearch.encode">FlexSearch.__encode__(name, string)</a>

Index methods:
- <a href="#index.add">Index.__add__(id, string)</a>
- <a href="#index.search">Index.__search__(string, \<limit\>, \<callback\>)</a>
- <a href="#index.update">Index.__update__(id, string)</a>
- <a href="#index.remove">Index.__remove__(id)</a>
- <a href="#index.reset">Index.__reset__()</a>
- <a href="#index.destroy">Index.__destroy__()</a>
- <a href="#index.init">Index.__init__(\<options\>)</a>
- <a href="#index.info">Index.__info__()</a>
- <a href="#index.addmatcher">Index.__addMatcher__({_KEY: VALUE_})</a>
- <a href="#index.encode">Index.__encode__(string)</a>

## Usage
<a name="flexsearch.create"></a>
#### Create a new index

> FlexSearch.__create(\<options\>)__

```js
var index = new FlexSearch();
```

alternatively you can also use:

```js
var index = FlexSearch.create();
```

##### Create a new index with custom options

```js
var index = new FlexSearch({

    // default values:

    encode: "icase",
    mode: "ngram",
    async: false,
    cache: false
});
```

__Read more:__ <a href="#phonetic">Phonetic Search</a>, <a href="#compare">Phonetic Comparison</a>, <a href="#memory">Improve Memory Usage</a>
<a name="index.add"></a>
#### Add items to an index

> Index.__add___(id, string)

```js
index.add(10025, "John Doe");
```
<a name="index.search"></a>
#### Search items

> Index.__search(string|options, \<limit\>, \<callback\>)__

```js
index.search("John");
```

Limit the result:

```js
index.search("John", 10);
```

Perform queries asynchronously:

```js
index.search("John", function(result){
    
    // array of results
});
```
<a name="index.update"></a>
#### Update item to the index

> Index.__update__(id, string)

```js
index.update(10025, "Road Runner");
```
<a name="index.remove"></a>
#### Remove item to the index

> Index.__remove__(id)

```js
index.remove(10025);
```
<a name="index.reset"></a>
#### Reset index

```js
index.reset();
```
<a name="index.destroy"></a>
#### Destroy the index

```js
index.destroy();
```
<a name="index.init"></a>
#### Re-Initialize index

> Index.__init(\<options\>)__

__Note:__ Re-initialization will also destroy the old index!

Initialize (with same options):
```js
index.init();
```

Initialize with new options:
```js
index.init({

    /* options */
});
```
<a name="flexsearch.addmatcher"></a>
#### Add custom matcher

> FlexSearch.__addMatcher({_REGEX: REPLACE_})__

Add global matchers for all instances:
```js
FlexSearch.addMatcher({

    'ä': 'a', // replaces all 'ä' to 'a'
    'ó': 'o',
    '[ûúù]': 'u' // replaces multiple
});
```
<a name="index.addmatcher"></a>
Add private matchers for a specific instance:
```js
index.addMatcher({

    'ä': 'a', // replaces all 'ä' to 'a'
    'ó': 'o',
    '[ûúù]': 'u' // replaces multiple
});
```
<a name="flexsearch.encoder"></a>
#### Add custom encoder

Define a private custom encoder during creation/initialization:
```js
var index = new FlexSearch({

    encode: function(str){
    
        // do something with str ...
        
        return str;
    }
});
```
<a name="flexsearch.register"></a>
##### Register a global encoder to be used by all instances

> FlexSearch.__register(name, encoder)__

```js
FlexSearch.register('whitespace', function(str){

    return str.replace(/ /g, '');
});
```

Use global encoders:
```js
var index = new FlexSearch({ encode: 'whitespace' });
```
<a name="index.encode"></a>
##### Call encoders directly

Private encoder:
```js
var encoded = index.encode("sample text");
```
<a name="flexsearch.encode"></a>
Global encoder:
```js
var encoded = FlexSearch.encode("whitespace", "sample text");
```

##### Mixup/Extend multiple encoders

```js
FlexSearch.register('mixed', function(str){
  
    str = this.encode("icase", str);  // built-in
    str = this.encode("whitespace", str); // custom
    
    return str;
});
```
```js
FlexSearch.register('extended', function(str){
  
    str = this.encode("custom", str);
    
    // do something additional with str ...

    return str;
});
```

<a name="index.info"></a>
#### Get info

```js
index.info();
```

Returns information about the index, e.g.:

```json
{
    "bytes": 3600356288,
    "id": 0,
    "matchers": 0,
    "size": 10000,
    "status": false
}
```
<a name="chaining"></a>
#### Chaining

Simply chain methods like:

```js
var index = FlexSearch.create()
                      .addMatcher({'â': 'a'})
                      .add(0, 'foo')
                      .add(1, 'bar');
```

```js
index.remove(0).update(1, 'foo').add(2, 'foobar');
```

#### Enable Contextual Index

Create context-enabled index and also set the limit of relevance (depth):
```js
var index = new FlexSearch({

    encode: "icase",
    mode: "strict",
    depth: 3
});
```

#### Use WebWorker (Browser only)

Create worker-enabled index and also set the count of parallel threads:
```js
var index = new FlexSearch({

    encode: "icase",
    mode: "full",
    async: true,
    worker: 4
});
```

Adding items to worker index as usual (async enabled):
```js
index.add(10025, "John Doe");
```

Perform search and simply pass in callback like:
```js
index.search("John Doe", function(results){

    // do something with array of results
});
```

<a name="options"></a>
## Options

FlexSearch ist highly customizable. Make use of the the right options can really improve your results as well as memory economy or query time.

<table>
    <tr></tr>
    <tr>
        <td align="left">Option</td>
        <td align="left">Values</td>
        <td align="left">Description</td>
    </tr>
    <tr>
        <td align="top">mode<br><br><br><br><br></td>
        <td vertical="top" vertical-align="top">
            "strict"<br>
            "foward"<br>
            "reverse"<br>
            "ngram"<br>
            "full"
        </td>
        <td vertical-align="top">
            The <a href="#tokenizer">indexing mode (tokenizer)</a>.<br>
        </td>
    </tr>
    <tr></tr>
    <tr>
        <td align="top">encode<br><br><br><br><br><br></td>
        <td>
            false<br>
            "icase"<br>
            "simple"<br>
            "advanced"<br>
            "extra"<br>
            function()
        </td>
        <td>The encoding type.<br><br>Choose one of the <a href="#phonetic">built-ins</a> or pass a <a href="#flexsearch.encoder">custom encoding function</a>.</td>
    </tr>
    <!--
    <tr></tr>
    <tr>
        <td align="top">boolean<br><br></td>
        <td>
            "and"<br>
            "or"
        </td>
        <td>The applied boolean model when comparing multiple words. <b>Note:</b> When using "or" the first word is also compared with "and". Example: a query with 3 words, results has either: matched word 1 & 2 and matched word 1 & 3.</td>
    </tr>
    <tr></tr>
    <tr>
        <td align="top">multi<br><br></td>
        <td>
            true<br>
            false
        </td>
        <td>Enable multi word processing.</td>
    </tr>
    -->
    <tr></tr>
    <tr>
        <td align="top">cache<br><br></td>
        <td>
            true<br>
            false
        </td>
        <td>Enable/Disable caching.</td>
    </tr>
    <tr></tr>
    <tr>
        <td align="top">async<br><br></td>
        <td>
            true<br>
            false
        </td>
        <td>Enable/Disable asynchronous processing.</td>
    </tr>
    <tr></tr>
    <tr>
        <td align="top">worker<br><br></td>
        <td>
            false<br>
            {number}
        </td>
        <td>Enable/Disable and set count of running worker threads.</td>
    </tr>
    <tr></tr>
    <tr>
        <td align="top">depth<br><br></td>
        <td>
            false<br>
            {number}
        </td>
        <td>Enable/Disable <a href="#contextual">contextual indexing</a> and also sets relevance depth (experimental).</td>
    </tr>
</table>

<a name="tokenizer"></a>
## Tokenizer

Tokenizer effects the required memory also as query time and flexibility of partial matches. Try to choose the most upper of these tokenizer which fits your needs:

<table>
    <tr></tr>
    <tr>
        <td align="left">Option</td>
        <td align="left">Description</td>
        <td align="left">Example</td>
        <td align="left">Memory Factor (n = length of word)</td>
    </tr>
    <tr>
        <td><b>"strict"</b></td>
        <td>index whole words</td>
        <td><b>foobar</b></td>
        <td>* 1</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"ngram"</b> (default)</td>
        <td>index words partially through phonetic n-grams</td>
        <td><b>foo</b>bar<br>foo<b>bar</b></td>
        <td>* n / 3.5</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"foward"</b></td>
        <td>incrementally index words in forward direction</td>
        <td><b>fo</b>obar<br><b>foob</b>ar<br></td>
        <td>* n</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"reverse"</b></td>
        <td>incrementally index words in both directions</td>
        <td>foob<b>ar</b><br>fo<b>obar</b></td>
        <td>* 2n - 1</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"full"</b></td>
        <td>index every possible combination</td>
        <td>fo<b>oba</b>r<br>f<b>oob</b>ar</td>
        <td>* n * (n - 1)</td>
    </tr>

</table>

<a name="phonetic"></a>
## Phonetic Encoding

Encoding effects the required memory also as query time and phonetic matches. Try to choose the most upper of these encoders which fits your needs, or pass in a <a href="#flexsearch.encoder">custom encoder</a>:

<table>
    <tr></tr>
    <tr>
        <td align="left">Option</td>
        <td align="left">Description</td>
        <td align="left">False-Positives</td>
        <td align="left">Compression</td>
    </tr>
    <tr>
        <td><b>false</b></td>
        <td>Turn off encoding</td>
        <td>no</td>
        <td>no</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"icase"</b> (default)</td>
        <td>Case in-sensitive encoding</td>
        <td>no</td>
        <td>no</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"simple"</b></td>
        <td>Phonetic normalizations</td>
        <td>no</td>
        <td>~ 7%</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"advanced"</b></td>
        <td>Phonetic normalizations + Literal transformations</td>
        <td>no</td>
        <td>~ 35%</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>"extra"</b></td>
        <td>Phonetic normalizations + Soundex transformations</td>
        <td>yes</td>
        <td>~ 60%</td>
    </tr>
    <tr></tr>
    <tr>
        <td><b>function()</b></td>
        <td>Pass custom encoding: function(string):string</td>
        <td></td>
        <td></td>
    </tr>
</table>

<a name="compare" id="compare"></a>
#### Comparison (Matches)

> Reference String: __"Björn-Phillipp Mayer"__

<table>
    <tr></tr>
    <tr>
        <td align="left">Query</td>
        <td align="left">iCase</td>
        <td align="left">Simple</td>
        <td align="left">Advanced</td>
        <td align="left">Extra</td>
    </tr>
    <tr>
        <td>björn</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>björ</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>bjorn</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>bjoern</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>philipp</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>filip</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>björnphillip</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>meier</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>björn meier</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>meier fhilip</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
        <td><b>yes</b></td>
    </tr>
    <tr></tr>
    <tr>
        <td>byorn mair</td>
        <td>no</td>
        <td>no</td>
        <td>no</td>
        <td><b>yes</b></td>
    </tr>
    <tr>
        <td><i>(false positives)</i></td>
        <td><b>no</b></td>
        <td><b>no</b></td>
        <td><b>no</b></td>
        <td>yes</td>
    </tr>
</table>

<a name="memory" id="memory"></a>
## Memory Usage

The required memory for the index depends on several options:

<table>
    <tr></tr>
    <tr>
        <td align="left">Encoding</td>
        <td align="left">Memory usage of every ~ 100,000 indexed word</td>
    </tr>
    <tr>
        <td>false</td>
        <td>260 kb</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"icase" (default)</td>
        <td>210 kb</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"simple"</td>
        <td>190 kb</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"advanced"</td>
        <td>150 kb</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"extra"</td>
        <td>90 kb</td>
    </tr>
    <tr>
        <td align="left">Mode</td>
        <td align="left">Multiplied with: (n = <u>average</u> length of indexed words)</td>
    </tr>
    <tr>
        <td>"strict"</td>
        <td>* 1</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"ngram" (default)</td>
        <td>* n / 3.5</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"forward"</td>
        <td>* n</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"reverse"</td>
        <td>* 2n - 1</td>
    </tr>
    <tr></tr>
    <tr>
        <td>"full"</td>
        <td>* n * (n - 1)</td>
    </tr>
    <tr>
        <td align="left">Contextual Index</td>
        <td align="left">Multiply the sum above with:</td>
    </tr>
    <tr>
        <td></td>
        <td>* (depth * 2 + 1)</td>
    </tr>
</table>

## Example Options

Memory-optimized:

```js
{
    encode: "extra",
    mode: "strict",
    threshold: 5
}
```

Speed-optimized:

```js
{
    encode: "icase",
    mode: "strict",
    threshold: 5,
    depth: 2
}
```

Matching-tolerant:

```js
{
    encode: "extra",
    mode: "full"
}
```

Balanced:

```js
{
    encode: "simple",
    mode: "ngram",
    threshold: 3,
    depth: 3
}
```

---

Author FlexSearch: Thomas Wilkerling<br>
License: <a href="http://www.apache.org/licenses/LICENSE-2.0.html" target="_blank">Apache 2.0 License</a><br>
