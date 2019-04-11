# @nbrowser/nb-chan
[![npm (scoped)](https://img.shields.io/npm/v/@nbrowser/nb-chan.svg)](https://github.com/nbrowser/nb-chan)
[![npm bundle size (minified)](https://img.shields.io/bundlephobia/min/@nbrowser/nb-chan.svg)](https://github.com/nbrowser/nb-chan)

nb-chan is a communication channel for Nucleome Browser.

nb-chan has the same function "on" and "call" as [d3-dispatch](https://github.com/d3/d3-dispatch).

if user installed [Nucleome Bridge](https://chrome.google.com/webstore/detail/djcdicpaejhpgncicoglfckiappkoeof) (Nbrowser chrome extension) , it will use Nbridge, otherwise, it will use local web browser's BroadCast channel to send command and genome coordinates between tabs.


```js
import {chan} from "@nbrowser/nb-chan";
var c = chan()
var callback = function(status){
  console.log(status)
}
c.connect(function(status){
  console.log(status)
})

//receive message from nbrowser bridge or BroadCast Channel.

c.on("update",function(d){
  //TODO  (user navigate to these genome coordinates in other tab, respond accordingly )
})
c.on("brush",function(d){
  //TODO  (user brush these genome coordinates in other tab, respond accordingly )
})

//send message to other nbrowser tabs

var regions = [{"genome":"hg38","chr":"chr1","start":1,"end":1000000}]

c.call("update",this,regions)

c.call("brush",this,regions)
```



