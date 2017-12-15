---
title: Sortable DFP Wrapper API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript


toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This is experimental documentation for the Sortable Enterprise API

The following is a list of function calls to 'swap out'. The original functions can be found in the [GPT Reference Document](https://developers.google.com/doubleclick-gpt/reference)

# API Calls

## deployads.gpt.pubadsDisplay

This is a replacement for googletag.pubads().display()

> Example:

```javascript
defineAndDisplaySlot = function(a) {
  var slot = '/1234567/sports',
      size = [728, 90],
      element = 'div-3';
    deployads.push(function() {
      window.deployads.gpt.pubadsDisplay(slot, size, element);
    });
  };
}
```
> Notice that the call to `deployads.gpt.pubadsDisplay` is wrapped by `deployads.push(function() {...})`

### pubads().display()
`deployads.gpt.pubadsDisplay(adUnitPath, size, opt_div, opt_clickUrl)`

### Query Parameters

Parameter | Type |  Description
--------- | ---- |  -----------
adUnitPath | `string` | Ad unit path of slot to be rendered.
size | <a href='https://developers.google.com/doubleclick-gpt/reference#googletag.GeneralSize'>!googletag.GeneralSize</a> | Width and height of the slot.
opt_div | (string \!Element)= | Either the ID of the div containing the slot or the div element itself.
opt_clickUrl | string= | The click URL to use on this slot.


## deployads.gpt.pubadsRefresh

This is a replacement for googletag.pubads().refresh()

> Example:

```javascript
defineAndDisplaySlot = function(a) {
  var slot = '/1234567/sports',
      size = [728, 90],
      element = 'div-3';
    deployads.push(function() {
      window.deployads.gpt.pubadsDisplay(slot, size, element);
    });
  };
...
doRefresh = function() {
    deployads.push(function() {
      window.deployads.gpt.pubadsRefresh(); // refreshes all slots, in this case just '/1234567/sports'
    });
};

```


### pubads().refresh()
`deployads.gpt.pubadsRefresh(opt_slots, opt_options)`

### Parameters

Parameter | Type |  Description
--------- | ---- |  -----------
opt_slots | Array<!<a href='https://developers.google.com/doubleclick-gpt/reference#googletag.Slot'>googletag.Slot</a>>= | The slots to refresh. Array is optional; all slots will be refreshed if it is unspecified.
opt_options | {changeCorrelator: boolean}= | Configuration options associated with this refresh call. changeCorrelator specifies whether or not a new correlator is to be generated for fetching ads. Our ad servers maintain this correlator value briefly (currently for 30 seconds, but subject to change), such that requests with the same correlator received close together will be considered a single page view. By default a new correlator is generated for every refresh.

<a href='https://developers.google.com/doubleclick-gpt/reference#googletag.PubAdsService_refresh'>GPT Reference</a>


## deployads.gpt.display
`deployads.gpt.display(div)`


### Parameters

Parameter | Type |  Description
--------- | ---- |  -----------
div | string l !Element | Either the ID of the div element containing the ad slot or the div element itself. If a div element is provided, it must have an 'id' attribute which matches the ID passed into `googletag.defineSlot()`.

> Example:

```html
<div id="div-1" style="width: 728px; height: 90px">
  <script type="text/javascript">
    googletag.cmd.push(function() {
      deployads.push(function() {
        deployads.gpt.display('div-1');
     });
    });
  </script>
</div>
```



# Hijack

```javascript
(function(){var a=window.googletag=window.googletag||{};a.cmd=a.cmd||[];var c=window.deployads=window.deployads||[],g=setTimeout(function(){if(!c.apiReady){c.apiTimeout=!0;c.gpt={display:function(d){a.cmd.push(function(){a.display(d)})},pubadsRefresh:function(d,b){a.cmd.push(function(){a.pubads().refresh(d,b)})},pubadsDisplay:function(d,b,e,f){a.cmd.push(function(){a.pubads().display(d,b,e,f)})}};for(var b=0;b<c.length;b++)try{c[b]()}catch(d){}}},2E3);c.push(function(){clearTimeout(g)});})();
```

The snippet to the right illustrates example 'hijack' code to be used for testing.

# Working example



```html
<!DOCTYPE html>
<html>
  <head>
    <script async='async' src='https://www.googletagservices.com/tag/js/gpt.js'></script>
    <script async='async' src='/deployads/testpub.js'></script>
    <script>
(function(){var a=window.googletag=window.googletag||{};a.cmd=a.cmd||[];var c=window.deployads=window.deployads||[],g=setTimeout(function(){if(!c.apiReady){c.apiTimeout=!0;c.gpt={display:function(d){a.cmd.push(function(){a.display(d)})},pubadsRefresh:function(d,b){a.cmd.push(function(){a.pubads().refresh(d,b)})},pubadsDisplay:function(d,b,e,f){a.cmd.push(function(){a.pubads().display(d,b,e,f)})}};for(var b=0;b<c.length;b++)try{c[b]()}catch(d){}}},2E3);c.push(function(){clearTimeout(g)});})();</script>


    <script>
      googletag.cmd.push(function() {
      googletag.defineSlot('/238158472/banner', [728, 90], 'div-gpt-ad-0').addService(googletag.pubads());
      googletag.pubads().enableSingleRequest();
      googletag.enableServices();
      });
    </script>
</head>
<body>
<!-- Your Page Source ... -->

<div id="div-1" style="width: 728px; height: 90px">
  <script type="text/javascript">
    googletag.cmd.push(function() {
      deployads.push(function() {
        deployads.gpt.display('div-1');
     });
    });
  </script>
</div>

</body>
</html>
```

The snippet to the right is meant to illustrate a working example using the API specified above. It is a fully functioning HTML page that displays one ad.
