# @nbrowser/nb-chan
[![npm (scoped)](https://img.shields.io/npm/v/@nbrowser/nb-chan.svg)](https://www.npmjs.com/package/@nbrowser/nb-chan)
[![npm bundle size (minified)](https://img.shields.io/bundlephobia/min/@nbrowser/nb-chan.svg)](https://github.com/nbrowser/nb-chan)

nb-chan is a cross-domain [event emitter](https://nodejs.org/api/events.html) for [Nucleome Platform](http://doc.nucleome.org). nb-chan has the same initialize method and the same function interface "on" and "call" as [d3-dispatch](https://github.com/d3/d3-dispatch). 

if user install our chrome extension [*Nucleome Bridge*](https://chrome.google.com/webstore/detail/djcdicpaejhpgncicoglfckiappkoeof), nb-chan will use *Nucleome Bridge* to send event emittion between tabs from multiple domains, otherwise, it will use web browser's *BroadCast Channel* to send event emittion between tabs within the same domain.

Our Web Application [Nucleome Browser](https://vis.nucleome.org) is connected with nb-chan.

## Connect 

Connect to channel.
```javascript
import {chan} from "@nbrowser/nb-chan";
var c = chan("update","brush")
var callback = function(status){
  console.log(status)
}
c.connect(callback)
```
Listen to events in all chan connected tabs.
```javascript
//receive message from nbrowser bridge or BroadCast Channel.

c.on("update",function(d){
  //Add your code (user navigate to these genome coordinates in other tab, respond accordingly )
})
c.on("update.anything",function(d){
  //register multiple listeners as d3-dispatch
  //Add your other code (user navigate to these genome coordinates in other tab, respond accordingly )
})
c.on("brush",function(d){
  //Add your code  (user brush these genome coordinates in other tab, respond accordingly )
})
```
Emit events to all chan connected tabs.
```javascript

var regions = [{genome:"hg38",chr:"chr1",start:1,end:10000},{genome:"hg38",chr:"chr2",start:1,end:1000}]

c.call("update",this,regions)

c.call("brush",this,regions)
```


## Installing
If you use NPM, `npm install @nbrowser/nb-chan`.
If you use in your webpage,
```html
<script src="https://vis.nucleome.org/static/lib/nb-chan.min.js"></script>
<script>

var c = nb.chan();
c.connect(function(status){
    //add your code callback.
});

</script>

```
## Connect your web page with *Nucleome Bridge*
*Nucleome Bridge* now support communication with UCSC Genome Browser and WashU EpiGenome Browser.

Due to the web safety reason, any web sites which want to connect with *Nucleome Bridge* needs to satisfty the pattern specify in *Nucleome Bridge* manifest.json file.

Currently, any local website, whose IP address is such as `*://127.0.0.1:*/*`, is connectable with *Nucleome Bridge*.

For user sharing their web pages, `https://bl.ocks.org/*` is also connectable with *Nucleome Bridge*. User can share their web page in this website by submit their code to gist. For futhur information. Please read [this webpage](https://bl.ocks.org/-/about).
We also have some demos in bl.ocks.org, here is [the link](https://bl.ocks.org/nb1page).

If you want to your website can be connectable with *Nucleome Bridge*, Please contact [us](mailto:zhuxp@cmu.edu). 


## API Reference

Check the connection status of channel
```javascript
  var c = nb.chan()
  c.connect(function(){
    console.log(c.status())
  }
```
Currently, standard communication code
```
event name: update and brush.
pass data: regions = [region... ]
           region format {genome:string,chr:string,start:int,end:int}
```
Start and end is defined as same as [bed format](https://genome.ucsc.edu/FAQ/FAQformat.html#format1). Start is 0-index. and end is not included in this region.

