# @nbrowser/nb-chan
[![npm (scoped)](https://img.shields.io/npm/v/@nbrowser/nb-chan.svg)](https://www.npmjs.com/package/@nbrowser/nb-chan)
[![npm bundle size (minified)](https://img.shields.io/bundlephobia/min/@nbrowser/nb-chan.svg)](https://github.com/nbrowser/nb-chan)

nb-chan is a communication channel for [Nucleome Browser](https://genome.compbio.cs.cmu.edu).

nb-chan has the same function interface "on" and "call" as [d3-dispatch](https://github.com/d3/d3-dispatch). It is an [event emitter](https://nodejs.org/api/events.html) system between the tabs.

if user installed [Nucleome Bridge](https://chrome.google.com/webstore/detail/djcdicpaejhpgncicoglfckiappkoeof) (NBrowser chrome extension) , it will use NBridge, otherwise, it will use local web browser's BroadCast Channel to send command and genome coordinates to other tabs.


```js
import {chan} from "@nbrowser/nb-chan";
var c = chan()
var callback = function(status){
  console.log(status)
}
c.connect(callback)
```
Listen to other NBrowser tabs.
```js
//receive message from nbrowser bridge or BroadCast Channel.

c.on("update",function(d){
  //Add your code (user navigate to these genome coordinates in other tab, respond accordingly )
})
c.on("brush",function(d){
  //Add your code  (user brush these genome coordinates in other tab, respond accordingly )
})
```
Send messages to other NBrowser tabs
```js

var regions = [{genome:"hg38",chr:"chr1",start:1,end:10000},{genome:"hg38",chr:"chr2",start:1,end:1000}]

c.call("update",this,regions)

c.call("brush",this,regions)
```


## Installing
If you use NPM, `npm install @nbrowser/nb-chan`.
If you use in your webpage,
```html
<script src="https://genome.compbio.cs.cmu.edu/static/lib/nb-chan.min.js"></script>
<script>

var c = nb.chan();
c.connect(function(status){
    //add your code callback.
});

</script>

```
## Connect your web page with NBridge 
Nucleome Bridge now support communication with UCSC Genome Browser and WashU EpiGenome Browser.
Due to the web safety reason, any web sites which want to connect with NBridge needs to satisfty the pattern specify in NBridge manifest.json file.

Currently, any local website such as `*://127.0.0.1:*/*` is connectable with NBridge.
And for user test , `https://bl.ocks.org/*` is also connectable with NBridge. User can test their web page in this website by submit their code to gist. For futhur information. Please read [this webpage](https://bl.ocks.org/-/about).
We also have some demos in bl.ocks.org, here is [the link](https://bl.ocks.org/nb1page).

If you want to your website can be connectable with NBridge, Please contact [us](mailto:zhuxp@cmu.edu). 


## API Reference

Check the connection status of channel
```js
  var c = nb.chan()
  c.connect(function(){
    console.log(c.status())
  }
```
