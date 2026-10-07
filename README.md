<p align="right">
  <a href="https://npmjs.org/package/deck.gl">
    <img src="https://img.shields.io/npm/v/deck.gl.svg?style=flat-square" alt="version" />
  </a>
  <a href="https://github.com/visgl/deck.gl/actions?query=workflow%3Atest+branch%3Amaster">
    <img src="https://github.com/visgl/deck.gl/workflows/test/badge.svg?branch=master" alt="build" />
  </a>
  <a href="https://npmjs.org/package/deck.gl">
    <img src="https://img.shields.io/npm/dm/@deck.gl/core.svg?style=flat-square" alt="downloads" />
  </a>
  <a href='https://coveralls.io/github/visgl/deck.gl?branch=master'>
    <img src='https://img.shields.io/coveralls/visgl/deck.gl.svg?style=flat-square' alt='Coverage Status' />
  </a>
</p>

<h1 align="center">deck.gl | <a href="https://deck.gl">Website</a></h1>

<h5 align="center"> GPU-powered, highly performant large-scale data visualization</h5>

[![docs](http://i.imgur.com/mvfvgf0.jpg)](https://visgl.github.io/deck.gl)


deck.gl is designed to simplify high-performance, WebGL2/WebGPU based visualization of large data sets. Users can quickly get impressive visual results with minimal effort by composing existing layers, or leverage deck.gl's extensible architecture to address custom needs.

deck.gl maps **data** (usually an array of JSON objects) into a stack of visual **layers** - e.g. icons, polygons, texts; and look at them with **views**: e.g. map, first-person, orthographic.

deck.gl handles a number of challenges out of the box:

* Performant rendering and updating of large data sets
* Interactive event handling such as picking, highlighting and filtering
* Cartographic projections and integration with major basemap providers
* A catalog of proven, well-tested layers

Deck.gl is designed to be highly customizable. All layers come with flexible APIs to allow programmatic control of each aspect of the rendering. All core classes such are easily extendable by the users to address custom use cases.

## Flavors

### Script Tag

```html
<script src="https://unpkg.com/deck.gl@latest/dist.min.js"></script>
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Global ops touch chart</title>
<style>
:root{
  --bg:#0B1620; --panel:#102230; --panel2:#17303F; --line:#2A4A5C;
  --text:#DCE8EE; --dim:#8FA9B8; --cyan:#52C7E8; --coral:#FF6B57; --amber:#F2B84B;
  --t:64px;
  --font:Bahnschrift,"DIN Alternate","Segoe UI",system-ui,-apple-system,sans-serif;
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;height:100%;background:var(--bg);color:var(--text);font-family:var(--font);overflow:hidden;touch-action:none;user-select:none;-webkit-user-select:none}
#map{position:absolute;inset:0}
.maplibregl-ctrl-attrib{font-size:10px}
.maplibregl-ctrl-scale{background:rgba(16,34,48,.8);color:var(--text);border-color:var(--dim);font-family:var(--font)}
body.noreadout .maplibregl-ctrl-scale,body.noreadout #coordbar{display:none}
button{font:inherit;color:inherit;cursor:pointer}
.num{font-variant-numeric:tabular-nums}

/* top bars */
#chip{position:absolute;left:12px;top:12px;display:flex;align-items:center;gap:10px;background:rgba(16,34,48,.93);border:1px solid var(--line);border-radius:14px;padding:8px 14px;min-height:48px;z-index:5}
#chip .dot{width:12px;height:12px;border-radius:50%;background:var(--cyan);box-shadow:0 0 0 4px rgba(82,199,232,.18)}
#chip.lost .dot{background:var(--amber);box-shadow:0 0 0 4px rgba(242,184,75,.2)}
#chip b{font-size:15px;font-weight:600;display:block;line-height:1.1}
#chip small{font-size:12px;color:var(--dim)}
#coordbar{position:absolute;left:50%;top:12px;transform:translateX(-50%);display:flex;align-items:center;gap:10px;background:rgba(16,34,48,.93);border:1px solid var(--line);border-radius:14px;padding:4px 6px 4px 14px;min-height:48px;z-index:5;font-size:13px;color:var(--dim)}
#goto{width:150px;min-height:38px;border-radius:10px;border:1px solid var(--line);background:#0B1620;color:var(--text);padding:0 10px;font:inherit;font-size:13px;user-select:text;-webkit-user-select:text}
@media (max-width:760px){#coordbar{left:12px;top:68px;transform:none}}

/* hud + zoom */
#hud{position:absolute;left:12px;bottom:112px;background:rgba(16,34,48,.93);border:1px solid var(--line);border-radius:14px;padding:10px 14px;font-size:12px;line-height:1.55;color:var(--dim);z-index:5;max-width:min(300px,70vw)}
#hud b{color:var(--text);font-weight:600}
#hud .k{display:flex;gap:12px;margin-top:6px;flex-wrap:wrap}
#hud .k i{display:inline-block;width:9px;height:9px;border-radius:50%;margin-right:5px}
#hud.off{display:none}
.fab{position:absolute;right:12px;width:56px;height:56px;border-radius:16px;background:rgba(16,34,48,.95);border:1px solid var(--line);font-size:26px;line-height:1;display:grid;place-items:center;z-index:5}
.fab:active{background:var(--panel2)}
#zoomIn{bottom:240px}#zoomOut{bottom:176px}#home{bottom:112px;font-size:13px}

/* dock */
#dock{position:absolute;left:50%;bottom:28px;transform:translateX(-50%);display:flex;gap:4px;padding:6px;background:rgba(16,34,48,.96);border:1px solid var(--line);border-radius:20px;z-index:8;max-width:calc(100vw - 16px);overflow-x:auto;touch-action:pan-x}
.dk{position:relative;width:66px;min-height:60px;border:0;border-radius:14px;background:transparent;color:var(--dim);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:4px;font-size:11px;flex:none}
.dk svg{width:24px;height:24px;fill:none;stroke:currentColor;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round}
.dk.on{background:var(--panel2);color:var(--cyan)}
.dk:hover{color:var(--text)}
.dk .bd{position:absolute;top:4px;right:10px;min-width:18px;height:18px;border-radius:9px;background:var(--coral);color:#fff;font-size:11px;font-weight:600;display:grid;place-items:center;padding:0 5px}
.dk .bd:empty{display:none}
@media (max-width:540px){.dk{width:54px}}

/* popover */
#pop{position:absolute;bottom:104px;width:min(370px,calc(100vw - 16px));max-height:min(62vh,540px);overflow-y:auto;background:var(--panel);border:1px solid var(--line);border-radius:16px;z-index:9;display:none;touch-action:pan-y;-webkit-overflow-scrolling:touch}
#pop.on{display:block}
.ph{display:flex;justify-content:space-between;align-items:center;padding:6px 8px 6px 16px;font-weight:600;font-size:15px;position:sticky;top:0;background:var(--panel);border-bottom:1px solid var(--line);z-index:1}
.ph button{width:44px;height:44px;border-radius:12px;border:0;background:transparent;font-size:22px;color:var(--dim)}
.opt{display:flex;align-items:center;gap:14px;min-height:52px;padding:8px 16px;border-bottom:1px solid rgba(42,74,92,.5);cursor:pointer}
.opt:hover{background:rgba(23,48,63,.7)}
.opt input{width:22px;height:22px;accent-color:#52C7E8;flex:none;margin:0}
.opt .t b{display:block;font-weight:500;font-size:15px}
.opt .t small{display:none;font-size:12px;color:var(--dim);margin-top:2px;line-height:1.35}
@media (hover:none){.opt .t small{display:block}}
.opt.choice{cursor:default;flex-wrap:wrap;justify-content:space-between}
.seg{display:flex;gap:4px;flex:none}
.seg button{min-width:50px;min-height:42px;padding:0 10px;border-radius:10px;border:1px solid var(--line);background:transparent;font-size:13px}
.seg button.on{background:var(--cyan);border-color:var(--cyan);color:#06222C;font-weight:600}
.btns{display:flex;gap:8px;padding:12px 14px 4px;flex-wrap:wrap}
.btns button,.wide{flex:1;min-width:100px;min-height:50px;border-radius:12px;border:1px solid var(--line);background:var(--panel2);font-size:14px}
.btns button:active,.wide:active{border-color:var(--cyan)}
.note{padding:6px 16px 12px;font-size:12px;color:var(--dim);line-height:1.45}
.inc{display:flex;align-items:center;gap:12px;width:100%;min-height:58px;padding:8px 16px;border:0;border-bottom:1px solid rgba(42,74,92,.5);background:transparent;text-align:left}
.inc:hover{background:rgba(23,48,63,.7)}
.inc i{width:12px;height:12px;border-radius:50%;flex:none}
.inc b{display:block;font-weight:500;font-size:14px}
.inc small{color:var(--dim);font-size:12px}
.chk{padding:6px 16px;font-size:13px;line-height:1.5}
.chk .ok{color:var(--cyan)}.chk .bad{color:var(--coral)}
textarea{width:calc(100% - 28px);margin:6px 14px 12px;height:110px;background:#0B1620;color:var(--dim);border:1px solid var(--line);border-radius:10px;font:12px/1.4 ui-monospace,Consolas,monospace;padding:8px;user-select:text;-webkit-user-select:text}
#tip{position:fixed;max-width:250px;background:#06121A;border:1px solid var(--line);border-radius:10px;padding:9px 12px;font-size:13px;line-height:1.4;z-index:40;display:none;pointer-events:none}

/* selection drawer */
#drawer{position:absolute;top:0;right:0;bottom:0;width:min(400px,88vw);background:var(--panel);border-left:1px solid var(--line);transform:translateX(102%);transition:transform .22s ease;z-index:20;display:flex;flex-direction:column}
#drawer.open{transform:none}
#dhead{display:flex;align-items:center;justify-content:space-between;padding:8px 8px 8px 18px;border-bottom:1px solid var(--line);font-weight:600}
#dclose{width:48px;height:48px;border-radius:12px;border:1px solid var(--line);background:transparent;font-size:22px}
#dbody{flex:1;overflow-y:auto;padding:6px 12px 28px;touch-action:pan-y}
.sel h2{margin:14px 4px 2px;font-size:22px;font-weight:600}
.sel p{margin:0 4px 10px;color:var(--dim);font-size:13px}
.kv{display:grid;grid-template-columns:auto 1fr;gap:6px 14px;margin:8px 4px 14px;font-size:14px}
.kv span:nth-child(odd){color:var(--dim)}
.tools{display:flex;gap:8px;margin:10px 0;flex-wrap:wrap}
.tools button{flex:1;min-width:120px;min-height:52px;border-radius:12px;border:1px solid var(--line);background:var(--panel2);font-size:14px}
.tools button.pri{background:var(--cyan);border-color:var(--cyan);color:#06222C;font-weight:600}
.li{display:flex;justify-content:space-between;align-items:center;width:100%;min-height:52px;padding:6px 8px;border:0;border-bottom:1px solid rgba(42,74,92,.6);background:transparent;text-align:left;font-size:14px}
.li small{color:var(--dim)}
.empty{margin:30px 6px;color:var(--dim);font-size:14px;line-height:1.5}
h3{margin:18px 4px 6px;font-size:13px;font-weight:600;color:var(--cyan)}

/* radial */
#ringHost{position:absolute;inset:0;pointer-events:none;z-index:10}
.ring{position:absolute;width:0;height:0}
.ring::before{content:"";position:absolute;left:-6px;top:-6px;width:12px;height:12px;border-radius:50%;background:var(--cyan)}
.ring button{position:absolute;left:0;top:0;width:var(--t);height:var(--t);margin:calc(var(--t) / -2) 0 0 calc(var(--t) / -2);border-radius:50%;border:1.5px solid var(--cyan);background:rgba(11,22,32,.96);pointer-events:auto;display:grid;place-items:center;font-size:calc(var(--t) * .4);line-height:1;transform:translate(0,0) scale(.3);opacity:0;transition:transform .16s ease,opacity .12s ease}
.ring.on button{transform:translate(var(--dx),var(--dy)) scale(1);opacity:1}
.ring button:active{background:var(--cyan);color:#06222C}
.ring .l{position:absolute;top:100%;left:50%;transform:translateX(-50%);margin-top:4px;font-size:12px;white-space:nowrap;text-shadow:0 1px 3px #000,0 0 6px #000;color:var(--text)}
#toast{position:absolute;left:50%;bottom:112px;transform:translate(-50%,20px);background:var(--panel2);border:1px solid var(--line);border-radius:12px;padding:12px 18px;font-size:14px;opacity:0;pointer-events:none;transition:all .2s;z-index:30;max-width:90vw;text-align:center}
#toast.on{opacity:1;transform:translate(-50%,0)}
#err{display:none;position:absolute;left:12px;right:12px;top:128px;z-index:60;background:#3a1612;border:1px solid var(--coral);border-radius:12px;padding:10px 14px;font-size:12px;line-height:1.5;color:#ffd9d2;user-select:text;-webkit-user-select:text;max-height:40vh;overflow:auto}
#fail{display:none;position:absolute;inset:0;place-items:center;padding:30px;text-align:center;z-index:50;background:var(--bg);color:var(--amber);font-size:15px;line-height:1.5}
</style>
</head>
<body>
<div id="map"></div>
<div id="chip"><span class="dot"></span><div><b id="chipA">Starting</b><small id="chipB" class="num"></small></div></div>
<div id="coordbar" class="num"><span id="coordtxt"></span><input id="goto" placeholder="Go to lat, lon" inputmode="text" aria-label="Go to coordinates"></div>
<button id="zoomIn" class="fab" aria-label="Zoom in">+</button>
<button id="zoomOut" class="fab" aria-label="Zoom out">−</button>
<button id="home" class="fab" aria-label="Reset view">World</button>
<div id="hud" class="num"></div>
<div id="ringHost"></div>
<div id="pop"></div>
<nav id="dock" aria-label="Map options"></nav>
<div id="tip"></div>
<div id="toast"></div>
<aside id="drawer" aria-label="Details">
  <div id="dhead"><span>Details</span><button id="dclose" aria-label="Close details">×</button></div>
  <div id="dbody"></div>
</aside>
<div id="err"></div>
<div id="fail" style="display:grid">Loading map libraries...</div>

<script>
window.__start=function(){
'use strict';
const $=s=>document.querySelector(s);
if(!window.maplibregl||!window.deck){
  const f=$('#fail');f.style.display='grid';
  f.textContent='The map libraries did not load. Check that this page can reach cdnjs.cloudflare.com, then reload.';
  return;
}

/* ================= settings: everything on by default ================= */
const S={
  basemapStyle:'dark', basemap:true, baseEngine:'img', outline:true, placeNames:true, graticule:true, hubs:true, readout:true,
  links:true, alerts:true, pins:true, trails:true, assetLabels:true,
  delta:true, interp:true, transMs:3000, staleDim:true, speed:60,
  target:64, radial:true, cluster:true, clusterZoom:true, trackRecenter:true, lockRot:true,
  pulse:true, pulseFps:8, dprCap:true, idleReset:true, idleSec:300, retention:6, trailLen:12, hud:true
};
const PRESETS={
  all:{pulse:true,pulseFps:15,trails:true,trailLen:24,links:true,alerts:true,pins:true,assetLabels:true,graticule:true,hubs:true,interp:true,transMs:3000,staleDim:true,cluster:true,dprCap:true,idleReset:true,delta:true},
  balanced:{pulse:false,trails:true,trailLen:12,links:true,alerts:true,pins:true,assetLabels:true,graticule:true,hubs:true,interp:true,transMs:3000,staleDim:true,cluster:true,dprCap:true,idleReset:true,delta:true},
  stable:{pulse:false,trails:false,trailLen:6,links:false,alerts:true,pins:true,assetLabels:false,graticule:false,hubs:true,interp:true,transMs:1000,staleDim:true,cluster:true,dprCap:true,idleReset:true,delta:true}
};
const ICON={
  map:'M9 3 3 5.5v15L9 18l6 3 6-2.5v-15L15 6 9 3z M9 3v15 M15 6v15',
  layers:'M12 3 3 8l9 5 9-5-9-5z M3 12.5l9 5 9-5 M3 16.5l9 5 9-5',
  data:'M20 12a8 8 0 1 1-2.3-5.7 M20 4v5h-5',
  touch:'M9 11V5a1.5 1.5 0 0 1 3 0v5 M12 10V8.5a1.5 1.5 0 0 1 3 0V11 M15 10.5a1.5 1.5 0 0 1 3 0V15a6 6 0 0 1-6 6h-1a6 6 0 0 1-5-2.7L4 14.5a1.5 1.5 0 0 1 2.4-1.7L9 15',
  shield:'M12 3 4 6v6c0 5 3.5 8 8 9 4.5-1 8-4 8-9V6l-8-3z M9 12l2 2 4-4',
  alert:'M12 3 2 20h20L12 3z M12 10v4.5 M12 17.5v.5',
  sliders:'M4 7h10 M18 7h2 M4 17h2 M10 17h10 M14 4v6 M6 14v6'
};
const CATS=[
 {id:'map',name:'Map',icon:ICON.map,opts:[
  ['basemapStyle','choice','Base map','Switch the ground between dark, street map and satellite imagery.',[['dark','Dark'],['streets','Streets'],['satellite','Satellite']]],
  ['basemap','toggle','Map imagery','Shows map imagery under your data; off leaves a plain dark ground.'],
  ['baseEngine','choice','Imagery loader','Tiles loads map pictures directly, Engine uses the alternate loader, and Outline shows only the built-in world shapes.',[['img','Tiles'],['gl','Engine'],['off','Outline']]],
  ['outline','toggle','World outline','Draws built-in continents and the US underneath so you always see a map, even if imagery is blocked.'],
  ['placeNames','toggle','Place names','Shows city, road and neighbourhood names on the map.'],
  ['graticule','toggle','Grid lines','Draws latitude and longitude lines for reference.'],
  ['hubs','toggle','Hub labels','Labels the main operating hubs such as London and Tokyo.'],
  ['readout','toggle','Coordinates bar','Shows centre coordinates, zoom level, a scale bar and a go-to box.']
 ]},
 {id:'layers',name:'Layers',icon:ICON.layers,opts:[
  ['alerts','toggle','Threat zones','Shades the broad zone around each open alert when zoomed out.'],
  ['pins','toggle','Alert pins','Marks the exact epicenter of each open alert so you can pinpoint it up close.'],
  ['links','toggle','Network links','Draws connections between hubs, with amber for degraded ones.'],
  ['trails','toggle','Asset trails','Shows the recent path behind each moving asset.'],
  ['assetLabels','toggle','Asset labels','Prints asset IDs next to icons once you zoom into a city.']
 ]},
 {id:'data',name:'Data',icon:ICON.data,opts:[
  ['delta','toggle','Delta updates','Fetches only assets that changed since the last sync instead of everything.'],
  ['interp','toggle','Glide between positions','Slides icons to their new position instead of jumping.'],
  ['transMs','choice','Glide duration','How long each slide takes.',[[1000,'1 s'],[3000,'3 s'],[6000,'6 s']]],
  ['staleDim','toggle','Dim overdue assets','Fades an asset when it misses its expected report time.'],
  ['speed','choice','Demo clock speed','Real updates arrive every 5 to 25 minutes; speed it up to watch the cycle.',[[1,'1×'],[60,'60×'],[300,'300×']]]
 ]},
 {id:'touch',name:'Touch',icon:ICON.touch,opts:[
  ['target','choice','Touch target size','Sets the tap area around icons and the size of menu buttons.',[[44,'Std'],[64,'Large'],[88,'XL']]],
  ['radial','toggle','Radial menu','Opens a circle of actions around your fingertip when you tap an asset.'],
  ['cluster','toggle','Group nearby assets','Bundles crowded icons into one tappable group.'],
  ['clusterZoom','toggle','Group tap zooms in','Tapping a group zooms in; off opens a list in the side panel.'],
  ['trackRecenter','toggle','Keep tracked asset centered','Follows a tracked asset each time it reports.'],
  ['lockRot','toggle','Lock rotation and tilt','Stops accidental two-finger twists.']
 ]},
 {id:'stability',name:'Stability',icon:ICON.shield,opts:[
  ['pulse','toggle','Pulse open alerts','Animates open alerts so they stand out; costs extra drawing.'],
  ['pulseFps','choice','Pulse frame rate','Lower is lighter on the display.',[[8,'8'],[15,'15']]],
  ['dprCap','toggle','Cap sharpness','Limits pixel density to 1.5× on dense screens for speed.'],
  ['idleReset','toggle','Reset view when idle','Returns to the world view after no touches.'],
  ['idleSec','choice','Idle time','How long without touches before the reset.',[[60,'1 min'],[120,'2 min'],[300,'5 min']]],
  ['retention','choice','Keep history for','Older trail points and closed alerts are purged from memory.',[[2,'2 h'],[6,'6 h'],[24,'24 h']]],
  ['trailLen','choice','Trail length','Number of past positions kept per asset.',[[6,'6'],[12,'12'],[24,'24']]],
  ['hud','toggle','Performance readout','Shows frame rate, sync size and memory at the bottom left.']
 ]},
 {id:'incidents',name:'Incidents',icon:ICON.alert,list:true},
 {id:'tools',name:'Tools',icon:ICON.sliders,tools:true}
];
const KEYS=Object.keys(S);

/* ================= simulated back end ================= */
const HUBS=[['New York',-74.0,40.7],['London',-0.13,51.5],['Frankfurt',8.68,50.1],['Dubai',55.3,25.2],['Singapore',103.8,1.35],['Tokyo',139.7,35.7],['Sydney',151.2,-33.9],['São Paulo',-46.6,-23.5],['Johannesburg',28.0,-26.2],['Los Angeles',-118.2,34.05],['Mumbai',72.9,19.1],['Reykjavik',-21.9,64.1],['Anchorage',-149.9,61.2],['Nairobi',36.8,-1.29],['Seoul',127,37.6]];
const T0=Date.UTC(2026,9,7,8,0,0);
const DELTA_WINDOW=45*60e3;
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
function mulberry(a){return function(){a|=0;a=a+0x6D2B79F5|0;let t=Math.imul(a^a>>>15,1|a);t=t+Math.imul(t^t>>>7,61|t)^t;return((t^t>>>14)>>>0)/4294967296}}
const rnd=mulberry(20261007);
const gauss=()=>(rnd()+rnd()+rnd()-1.5)*1.6;
const fmt=ms=>new Date(ms).toISOString().slice(11,16)+'Z';
const esc=s=>String(s).replace(/[&<>"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
function hav(lon1,lat1,lon2,lat2){const r=Math.PI/180,dLat=(lat2-lat1)*r,dLon=(lon2-lon1)*r,a=Math.sin(dLat/2)**2+Math.cos(lat1*r)*Math.cos(lat2*r)*Math.sin(dLon/2)**2;return 12742*Math.asin(Math.sqrt(a))}
function nearHub(lon,lat){let best=null,bd=1e9;for(const h of HUBS){const d=hav(lon,lat,h[1],h[2]);if(d<bd){bd=d;best=h[0]}}return[best,Math.round(bd)]}
function dms(v,pos,neg){const h=v>=0?pos:neg;v=Math.abs(v);const d=Math.floor(v),m=Math.floor((v-d)*60),s=((v-d)*60-m)*60;return d+'° '+m+"' "+s.toFixed(1)+'" '+h}

const srv={rev:0,assets:[],alerts:[],byId:new Map(),nextAlert:T0+25*60e3};
let alertSeq=0;
for(let i=0;i<360;i++){
  const h=HUBS[i%HUBS.length], r=rnd();
  const type=r<.5?'vehicle':r<.8?'personnel':'sensor';
  const spread=type==='vehicle'?7:2.5, every=5+Math.floor(rnd()*21);
  const a={id:'A-'+String(i+1).padStart(3,'0'),type,hub:h[0],
    lon:clamp(h[1]+gauss()*spread,-178,178),lat:clamp(h[2]+gauss()*spread,-68,75),
    hd:rnd()*360,spd:type==='vehicle'?150+rnd()*450:type==='personnel'?5:0,
    every,next:T0+rnd()*every*60e3,upd:T0-rnd()*every*60e3,rev:0,iso:false};
  srv.assets.push(a);srv.byId.set(a.id,a);
}
const ALERT_TITLES=['Credential stuffing burst','Beacon to flagged host','Sensor tamper signal','Unusual egress volume','Firmware hash mismatch'];
const ALERT_SRC=['Splunk sec-core','Splunk net-edge','Splunk iot-field'];
function spawnAlert(t){
  const a=srv.assets[Math.floor(rnd()*srv.assets.length)], sev=1+Math.floor(rnd()*3);
  srv.alerts.push({id:'AL-'+(++alertSeq),lon:clamp(a.lon+gauss()*.4,-178,178),lat:clamp(a.lat+gauss()*.4,-70,78),sev,created:t,
    closeAt:t+(2+rnd()*4)*3600e3,closed:false,rev:++srv.rev,rKm:150+sev*110,
    title:ALERT_TITLES[Math.floor(rnd()*ALERT_TITLES.length)],src:ALERT_SRC[Math.floor(rnd()*ALERT_SRC.length)]});
}
for(let i=0;i<6;i++)spawnAlert(T0-rnd()*3*3600e3);
srv.rev=0;srv.alerts.forEach(a=>a.rev=0);
function serverAdvance(now){
  for(const a of srv.assets){
    while(a.next<=now){
      const km=a.spd*(a.every/60);
      a.hd+=(rnd()-.5)*30;
      const rad=a.hd*Math.PI/180;
      a.lat=clamp(a.lat+Math.cos(rad)*km/111,-70,78);
      a.lon+=Math.sin(rad)*km/(111*Math.max(.25,Math.cos(a.lat*Math.PI/180)));
      if(a.lon>178||a.lon<-178){a.lon=clamp(a.lon,-178,178);a.hd=-a.hd}
      a.upd=a.next;a.rev=++srv.rev;a.next+=a.every*60e3;
    }
  }
  while(srv.nextAlert<=now){spawnAlert(srv.nextAlert);srv.nextAlert+=(20+rnd()*40)*60e3}
  for(const al of srv.alerts){if(!al.closed&&al.closeAt<=now){al.closed=true;al.closedAt=al.closeAt;al.rev=++srv.rev}}
  if(srv.alerts.length>80)srv.alerts=srv.alerts.filter(a=>!a.closed||now-a.closedAt<3*3600e3);
}

/* ================= client state ================= */
let simNow=T0, linkUp=true, synced=false, sinceRev=-1, lastSyncT=T0, lastPayload='', lastFullBytes=0, lastDeltaBytes=0, tileErrors=0, tilesOk=0, tilesBad=0, glOk=0;
const assets=new Map(); let assetsArr=[], alertsArr=[], trailArr=[];
const alertsOpen=new Map(); const closedLog=[];
let singles=[], clusters=[], epoch=0, wasClustered=false, staleTick=0;
let selId=null, trackId=null, selAlert=null, selLink=null, selCluster=null, pinned=null;

function pull(){
  if(!linkUp){setChip();return}
  const gap=simNow-lastSyncT;
  const full=!S.delta||!synced||gap>DELTA_WINDOW;
  const pa=full?srv.assets:srv.assets.filter(a=>a.rev>sinceRev);
  const pl=full?srv.alerts:srv.alerts.filter(a=>a.rev>sinceRev);
  const payload={full,assets:pa.map(a=>full?{id:a.id,type:a.type,hub:a.hub,lon:a.lon,lat:a.lat,upd:a.upd,every:a.every,iso:a.iso}:{id:a.id,lon:a.lon,lat:a.lat,upd:a.upd,iso:a.iso}),
    alerts:pl.map(a=>({id:a.id,lon:a.lon,lat:a.lat,sev:a.sev,created:a.created,closed:a.closed,closedAt:a.closedAt,rKm:a.rKm,title:a.title,src:a.src,closeAt:a.closeAt}))};
  const bytes=JSON.stringify(payload).length;
  const resync=full&&synced&&S.delta;
  if(full)lastFullBytes=bytes;else lastDeltaBytes=bytes;
  for(const p of payload.assets){
    let a=assets.get(p.id);
    if(!a){a={id:p.id,type:p.type,hub:p.hub,every:p.every,pos:[p.lon,p.lat],upd:p.upd,iso:p.iso,trail:[[p.lon,p.lat]],tt:[p.upd]};assets.set(p.id,a)}
    else{
      if(a.pos[0]!==p.lon||a.pos[1]!==p.lat){a.pos=[p.lon,p.lat];a.trail.push([p.lon,p.lat]);a.tt.push(p.upd)}
      a.upd=p.upd;a.iso=p.iso;
    }
    while(a.trail.length>S.trailLen){a.trail.shift();a.tt.shift()}
  }
  for(const p of payload.alerts){
    if(p.closed){if(alertsOpen.delete(p.id)){closedLog.push({id:p.id,closedAt:p.closedAt||simNow});if(selAlert&&selAlert.id===p.id)selAlert=null;if(pinned&&pinned.id===p.id)pinned=null}}
    else alertsOpen.set(p.id,p);
  }
  sinceRev=srv.rev;lastSyncT=simNow;synced=true;
  lastPayload=(full?'Full set ':'Delta ')+payload.assets.length+' assets, '+(bytes/1024).toFixed(1)+' KB'+(resync?' (resync)':'');
  setChip();
  if(!payload.assets.length&&!payload.alerts.length&&!full)return;
  assetsArr=Array.from(assets.values());
  alertsArr=Array.from(alertsOpen.values()).sort((a,b)=>b.sev-a.sev||b.created-a.created);
  trailArr=assetsArr.filter(a=>a.trail.length>1);
  computeClusters();
  updateBadge();
  scheduleRender();
  if(trackId&&S.trackRecenter){const t=assets.get(trackId);if(t)map.easeTo({center:t.pos,duration:1500,essential:true})}
  if(openCat==='incidents')renderPop();
  if($('#drawer').classList.contains('open'))renderSel();
}

/* ================= clustering ================= */
const CLUSTER_MAX_ZOOM=5.5;
function computeClusters(){
  const z=map.getZoom();
  const on=S.cluster&&z<CLUSTER_MAX_ZOOM;
  if(on!==wasClustered){epoch++;wasClustered=on}
  if(!on){singles=assetsArr;clusters=[];return}
  const size=64, scale=512*Math.pow(2,z), cells=new Map();
  for(const a of assetsArr){
    const x=(a.pos[0]+180)/360*scale, s=Math.sin(a.pos[1]*Math.PI/180);
    const y=(0.5-Math.log((1+s)/(1-s))/(4*Math.PI))*scale;
    const k=Math.floor(x/size)+':'+Math.floor(y/size);
    let c=cells.get(k);if(!c){c={items:[],sx:0,sy:0};cells.set(k,c)}
    c.items.push(a);c.sx+=a.pos[0];c.sy+=a.pos[1];
  }
  singles=[];clusters=[];
  for(const c of cells.values()){
    const n=c.items.length;
    if(n===1)singles.push(c.items[0]);
    else clusters.push({pos:[c.sx/n,c.sy/n],count:n,items:c.items});
  }
  epoch++;
}

/* ================= map ================= */
const CARTO=['a','b','c'].map(s=>'https://'+s+'.basemaps.cartocdn.com/');
const rs=(tiles,attr)=>({type:'raster',tiles,tileSize:256,maxzoom:19,attribution:attr});
const CA='© OpenStreetMap contributors © CARTO', EA='Imagery © Esri, Maxar, Earthstar Geographics';
const ESRI='https://server.arcgisonline.com/ArcGIS/rest/services/';
const style={version:8,
  sources:{
    dark:rs(CARTO.map(u=>u+'dark_all/{z}/{x}/{y}.png'),CA),
    darknl:rs(CARTO.map(u=>u+'dark_nolabels/{z}/{x}/{y}.png'),CA),
    voy:rs(CARTO.map(u=>u+'rastertiles/voyager/{z}/{x}/{y}.png'),CA),
    voynl:rs(CARTO.map(u=>u+'rastertiles/voyager_nolabels/{z}/{x}/{y}.png'),CA),
    sat:rs([ESRI+'World_Imagery/MapServer/tile/{z}/{y}/{x}'],EA),
    satlab:rs([ESRI+'Reference/World_Boundaries_and_Places/MapServer/tile/{z}/{y}/{x}'],EA)
  },
  layers:[{id:'bg',type:'background',paint:{'background-color':'#0B1620'}}].concat(
    ['dark','darknl','voy','voynl','sat','satlab'].map(id=>({id:'b_'+id,type:'raster',source:id,layout:{visibility:'none'},paint:{'raster-fade-duration':0}})))
};
function homeView(){const w=innerWidth;return{center:[12,22],zoom:Math.max(.6,Math.log2(w/512)+.15)}}
const hv=homeView();
const map=new maplibregl.Map({container:'map',style,center:hv.center,zoom:hv.zoom,minZoom:.5,maxZoom:20,renderWorldCopies:false,attributionControl:{compact:true},fadeDuration:0});
map.addControl(new maplibregl.ScaleControl({maxWidth:110,unit:'metric'}),'bottom-left');
const overlay=new deck.MapboxOverlay({interleaved:false,layers:[],pickingRadius:S.target/2,onClick:onPick});
map.addControl(overlay);

function applyBase(){
  if(!map.getLayer('b_dark'))return;
  const st=S.basemapStyle,lab=S.placeNames,on=S.basemap&&S.baseEngine==='gl';
  const vis={b_dark:on&&st==='dark'&&lab,b_darknl:on&&st==='dark'&&!lab,b_voy:on&&st==='streets'&&lab,b_voynl:on&&st==='streets'&&!lab,b_sat:on&&st==='satellite',b_satlab:on&&st==='satellite'&&lab};
  for(const k in vis)map.setLayoutProperty(k,'visibility',vis[k]?'visible':'none');
  map.setPaintProperty('bg','background-color',S.basemap&&st==='streets'?'#E3EAEF':'#0B1620');
}


/* ---- built-in coarse world outline (works with no network) ---- */
const P=a=>{const r=[];for(let i=0;i<a.length;i+=2)r.push([a[i],a[i+1]]);return r};
const LAND_RAW=[
 [0,[-168,65.5,-162,70,-156,71.3,-141,69.7,-128,70,-115,68.5,-108,68,-95,68.5,-88,68,-82,66,-86,64,-93,61,-94,58.5,-90,57,-82,55,-79,51.5,-78.5,57,-77,60.5,-72,61.5,-65,60.5,-61,56,-57,53,-56,51.5,-60,50,-66,50.2,-64,47,-61,46,-66,44.5,-70,43.7,-70,41.8,-74,40.5,-76,38,-75.5,35.3,-78,33.8,-81,31.5,-80,27,-80.3,25.3,-81.8,26,-82.7,28.5,-84,30,-86,30.3,-89,30.2,-90,29.1,-94,29.5,-97.3,27.5,-97.5,24,-97.7,21,-96,19,-94.5,18.2,-91,18.8,-90.5,21,-87,21.5,-87.5,18,-88.2,16,-84,15.8,-83.3,14,-83.7,11,-82,9,-79.5,9.5,-77.5,8.7,-78,7.5,-80.5,7.3,-83,8.3,-85.7,10,-87.5,13,-91,14,-94,16,-97,15.8,-101,17.3,-105.5,20,-105.5,22.5,-108,25.5,-112.5,29.5,-114.7,31.7,-114.5,29,-112,26,-110,23,-112,24.5,-115,28,-117,32.5,-120.5,34.5,-122.5,37.5,-124.2,40.5,-124,46,-124.7,48.4,-123,48.5,-123,50,-127,51,-130,54.5,-134,57,-138,59,-144,60,-150,59.5,-152,58.5,-157,57,-163,54.7,-158,58.5,-162,60,-165,62,-161,64,-166,64.5]],
 [0,[-77.5,8.7,-75,11,-72,12,-68,10.7,-62,10.7,-60,8.5,-57,6,-52,5,-50,1,-48,-1,-44,-2.5,-40,-3,-35,-5.5,-35,-9,-39,-14,-39,-18,-41,-22,-45,-23.5,-48.5,-26,-48.8,-28.5,-52,-32,-54,-34.5,-57,-35,-57.5,-38,-62,-39,-65,-41,-65,-45,-67.5,-46.5,-66,-48,-69,-51,-68.5,-53,-71,-54.5,-74,-52,-75.5,-47,-73.5,-42,-73.7,-37,-71.5,-30,-70.3,-18.3,-75,-15,-78.5,-10,-81.2,-6,-80,-3,-80.5,-1,-79,1.5,-77.5,4,-77.4,6.5]],
 [0,[-17,21,-16,16,-17.5,14.7,-15,11,-13,8.5,-8,4.5,-4,5.2,1,6,4.5,6.3,9,4,9.5,1,9,-2,12,-5.5,13.5,-11,12,-17,14.5,-22.5,16,-28.5,18.3,-34,20,-35,25,-34,30,-31,32.5,-28,35,-24,35.5,-20,39,-16,40.5,-11,39,-6,41,-2,44,1,48,5,51,10.8,48,11.5,43.3,12.5,41,15,37.5,18,35,24,32.5,29.5,32.3,31.2,27,31.5,20,31,15,32.3,11,33,10,37,3,36.8,-2,35.2,-6,35.8,-9.5,32,-10,29,-13,27.5,-16,23.5]],
 [0,[-9.5,37,-9,43,-2,43.5,-1.5,46,-4.5,48.3,-1.5,48.8,1.5,50.8,4,51.5,8.5,53.6,8.5,57,10.5,57.7,10.5,54.5,14,54,19,54.5,21,56,24,57.5,24,59.5,30,60,34,64.5,40,66,44,68.5,53,68.5,60,69,68,68.5,73,72.5,80,73.5,87,75,100,77.5,105,77.5,113,73.7,129,72,140,72.5,150,71.5,160,69.7,170,70,180,69,180,65,177,64.5,179,62.5,173,61,165,60,163,57.5,162,54.5,156.5,51,156,57,161,60.5,155,59,143,59.3,137,54,141,52,140.5,48,135,43.5,131,42.5,129.7,41,128,39,129.5,36,126.5,34.5,126,37.5,125,39.5,121.5,40.8,119,39,122,37,119.5,35,121.8,31.3,121.5,28,119,25,114,22.3,110,21,108,21.5,106.5,19.5,109,15,109,11.5,105,8.7,104.5,10.3,100.8,13.5,100,9,102,6,104,1.3,101,3,98.5,8,98.5,13,97.5,16.5,94.5,16,94,19,91.5,22.5,87,21.5,85,19.5,80.3,15.5,80,10.3,77.5,8,76,10.5,73,17,72.8,21,70,20.8,68.5,23.5,66.5,25.3,61.5,25.2,57,25.8,56.5,27,52,28,50,30,48.5,30,48,28.5,50.2,26.5,51.5,24.5,54,24,56.3,26.2,56.8,24.2,59.8,22.5,57.5,18.5,53,16.7,48,14,43.5,12.7,42.8,15,39,21.5,35,28,34.9,29.5,34.3,31.3,35.5,34,36,36.5,32,36.2,28,36.7,26.5,38.5,26.5,40.2,28,41.2,26,40.8,23.5,40.2,24,38,22,36.5,21,38.5,19.5,40.5,19,42,15.5,45,13.7,45.2,12.3,45.3,12.5,44,14,42.5,16,41.9,18.5,40.2,17,39,16.5,38,15.7,38,16,40,12.5,41.5,10.5,43,8.8,44.3,6.5,43.2,3.2,43.2,3,42,0.5,40.5,-0.3,38.7,-2,36.8,-5.5,36.1,-6.5,37,-9,37]],
 [0,[5,58.5,5.5,62,10,64.5,14,67.5,19,70,25,71,31,70,30,67,29,63,28,60.5,24,60,22,60.2,21.5,62.5,25,65,21.7,65.7,17.5,62.5,18.5,60,16.7,57.7,16,56.2,13,55.4,12.5,56.5,11.2,59,8,58.2]],
 [0,[-5.5,50,1.3,51.2,1.7,52.7,-0.2,53.5,-1.5,55.5,-2,57.6,-3.8,57.6,-5,58.6,-6,56.5,-5,55,-3,54.8,-3,53.4,-4.7,52.8,-5.2,51.7,-3,51.3]],
 [0,[-6,52,-6,54,-7.5,55.3,-10,54,-10,52,-8,51.5]],
 [0,[-24,65.5,-22,66.4,-16,66.5,-13.5,65,-18,63.4,-22.5,63.8]],
 [0,[-73,78.5,-60,82,-30,83.5,-20,81.5,-18,76,-22,70,-30,68,-40,65,-43,60,-50,62,-53,67,-56,72,-68,76]],
 [0,[130.8,31.2,132,33.5,135,33.5,137,34.6,140,35,141,38,142,40.5,141.5,41.5,140,40.8,139.5,38,137,37,136,35.7,133,35.5,131,34.5]],
 [0,[140,42,141.5,45.4,145.5,43.3,143.3,42]],
 [0,[120,18.5,122,18.3,122,14,124,13,121,13.5,120.5,15]],
 [0,[122,7,126.5,7.5,126,9.5,124,9]],
 [0,[95.3,5.6,98,4,104,-1,106,-5.8,102,-4,98,0.5]],
 [0,[105.3,-6.8,114.5,-7.7,114.5,-8.5,106,-7.5]],
 [0,[109,1.5,111,2,115,5,117.5,7,119,5,117.5,1,116,-3.5,111,-3,109,-1.5]],
 [0,[119.5,-5.5,120.5,-2,121,1.2,125,1.5,123,-1,122,-4.5,120.4,-5.6]],
 [0,[131,-1,135,-3.3,138,-1.8,145,-4,147.5,-6,150.5,-10.5,147.5,-10,143,-9,141,-9.1,138,-8.3,137.5,-5,134,-4]],
 [0,[114,-22,113.5,-26,115,-34,118,-35,123,-34,129,-31.7,134,-32.5,137.7,-35,140,-38,144,-38.5,147,-38.5,150,-37,153,-31,153.5,-27,149,-21,146,-18.7,145.5,-15,143.5,-14,142.5,-10.7,141,-13,140,-17.5,136.5,-15.5,135.5,-12.3,131,-11.3,129,-14.8,125,-14.5,122,-17.5,118,-20.3]],
 [0,[172.7,-34.5,178.5,-37.7,177,-39.5,175,-41.5,173.5,-39.5,174.5,-37]],
 [0,[172.7,-40.5,174,-41.7,171,-44.5,169,-46.6,166.5,-46,168,-44]],
 [0,[49.3,-12,50.5,-15.5,47.5,-24.5,45,-25.5,43.3,-22,44.3,-16.5]],
 [0,[-85,22,-82,23.2,-77,21.8,-74.2,20.2,-77.7,19.9,-80,21.8]],
 [0,[-74.4,18.4,-72,19.9,-69,19.2,-68.4,18.5,-71,17.7]],
 [0,[-180,-90,180,-90,180,-78,160,-78,135,-66,90,-66,45,-68,0,-70,-45,-77,-62,-72,-65,-66,-75,-72,-110,-74,-150,-78,-180,-78]],
 [1,[-124.7,48.4,-123,49,-95,49,-94.7,48.7,-89.5,48,-84.5,46.5,-82.5,45.3,-82.5,42.5,-79,43.3,-76.5,44,-75,45,-71.5,45,-70.8,45.4,-69,47.4,-67.8,47,-67,45,-70,43.7,-70,41.8,-74,40.5,-76,38,-75.5,35.3,-78,33.8,-81,31.5,-80,27,-80.3,25.3,-81.8,26,-82.7,28.5,-84,30,-86,30.3,-89,30.2,-90,29.1,-94,29.5,-97.3,27.5,-97.2,25.9,-99,26.4,-101,29.8,-104.5,29.6,-106.5,31.8,-111,31.3,-114.8,32.5,-117.1,32.5,-120.5,34.5,-122.5,37.5,-124.2,40.5,-124,46]]
];
const LANDPOLY=LAND_RAW.map(r=>({us:!!r[0],p:P(r[1])}));

/* ---- imagery loaded as plain pictures (no fetch), drawn by deck.gl ---- */
function tileUrl(kind,x,y,z){
  const c=CARTO[(x+y)%3];
  switch(kind){
    case 'dark':return c+'dark_all/'+z+'/'+x+'/'+y+'.png';
    case 'darknl':return c+'dark_nolabels/'+z+'/'+x+'/'+y+'.png';
    case 'voy':return c+'rastertiles/voyager/'+z+'/'+x+'/'+y+'.png';
    case 'voynl':return c+'rastertiles/voyager_nolabels/'+z+'/'+x+'/'+y+'.png';
    case 'sat':return ESRI+'World_Imagery/MapServer/tile/'+z+'/'+y+'/'+x;
    default:return ESRI+'Reference/World_Boundaries_and_Places/MapServer/tile/'+z+'/'+y+'/'+x;
  }
}
function baseKinds(){const st=S.basemapStyle,l=S.placeNames;return st==='dark'?[l?'dark':'darknl']:st==='streets'?[l?'voy':'voynl']:(l?['sat','satlab']:['sat'])}
function loadTile(url,signal){
  return new Promise((res,rej)=>{
    const im=new Image();im.crossOrigin='anonymous';
    im.onload=()=>res(im);im.onerror=()=>rej(new Error('tile'));
    if(signal)signal.addEventListener('abort',()=>{im.onload=im.onerror=null;im.src='';const e=new Error('abort');e.name='AbortError';rej(e)});
    im.src=url;
  });
}
function baseTileLayers(){
  return baseKinds().map(kind=>new deck.TileLayer({id:'tiles-'+kind,data:'tiles://'+kind+'/{z}/{x}/{y}',minZoom:0,maxZoom:19,tileSize:256,
    getTileData:t=>loadTile(tileUrl(kind,t.index.x,t.index.y,t.index.z),t.signal),
    onTileLoad:()=>{tilesOk++},onTileError:err=>{if(err&&err.message==='tile')tilesBad++},
    renderSubLayers:p=>{const b=p.tile.bbox;return new deck.BitmapLayer({id:p.id,image:p.data,bounds:[b.west,b.south,b.east,b.north]})}}));
}
function switchEngine(mode,msg){S.baseEngine=mode;apply('baseEngine');if(openCat)renderPop();toast(msg);tilesOk=0;tilesBad=0;tileErrors=0}
setInterval(()=>{
  if(!S.basemap||S.baseEngine==='off')return;
  if(S.baseEngine==='img'&&tilesOk===0&&tilesBad>=6)switchEngine('gl','Direct tiles were blocked. Trying the map engine loader.');
  else if(S.baseEngine==='gl'&&glOk===0&&tileErrors>=6)switchEngine('off','Map imagery is blocked on this screen. Showing the built-in outline map.');
},3000);

/* ================= layers ================= */
const GRAT=[];
for(let lon=-180;lon<=180;lon+=30){const p=[];for(let lat=-80;lat<=80;lat+=10)p.push([lon,lat]);GRAT.push(p)}
for(let lat=-60;lat<=60;lat+=30){const p=[];for(let lon=-180;lon<=180;lon+=10)p.push([lon,lat]);GRAT.push(p)}
const LINK_PAIRS=[[0,1],[1,2],[2,3],[3,4],[4,5],[5,14],[0,9],[9,5],[1,7],[2,8],[3,10],[10,4],[4,6],[13,3],[0,11],[11,1]];
const LINKS=LINK_PAIRS.map((p,i)=>({a:[HUBS[p[0]][1],HUBS[p[0]][2]],b:[HUBS[p[1]][1],HUBS[p[1]][2]],an:HUBS[p[0]][0],bn:HUBS[p[1]][0],ok:i%5!==3,
  deps:['Replica sync','Identity gateway','Log forwarder','Threat feed relay'].slice(0,2+i%3)}));
const HUBDATA=HUBS.map(h=>({name:h[0],pos:[h[1],h[2]]}));
const TYPE_COL={vehicle:[82,199,232],personnel:[243,247,249],sensor:[183,166,255]};
const SEV_COL={1:[242,184,75],2:[255,140,80],3:[255,107,87]};
const FONT='Bahnschrift,Segoe UI,system-ui,sans-serif';
function assetColor(a){
  if(a.iso)return [242,184,75,255];
  const base=TYPE_COL[a.type], age=(simNow-a.upd)/60000;
  if(S.staleDim){if(age>a.every*3)return [base[0],base[1],base[2],70];if(age>a.every*2)return [base[0],base[1],base[2],140]}
  return [base[0],base[1],base[2],255];
}
function visibleAssets(){
  const b=map.getBounds();if(!b)return [];
  const w=b.getWest(),e=b.getEast(),s=b.getSouth(),n=b.getNorth();
  return singles.filter(a=>a.pos[0]>=w&&a.pos[0]<=e&&a.pos[1]>=s&&a.pos[1]<=n);
}
function buildLayers(){
  const L=[], D=deck, z=map.getZoom(), clustered=wasClustered, light=S.basemap&&S.basemapStyle==='streets';
  if(S.outline)L.push(new D.PolygonLayer({id:'land-'+(light?'l':'d'),data:LANDPOLY,getPolygon:d=>d.p,filled:true,stroked:true,
    getFillColor:d=>light?(d.us?[226,232,236,255]:[214,222,228,255]):(d.us?[30,64,82,255]:[24,52,68,255]),getLineColor:light?[140,160,172,255]:[60,100,125,255],lineWidthMinPixels:1}));
  if(S.basemap&&S.baseEngine==='img'&&D.TileLayer&&D.BitmapLayer)baseTileLayers().forEach(l=>L.push(l));
  if(S.graticule)L.push(new D.PathLayer({id:'grat',data:GRAT,getPath:d=>d,getColor:light?[40,70,90,40]:[120,160,180,30],widthMinPixels:1}));
  if(S.alerts)L.push(new D.ScatterplotLayer({id:'alerts',data:alertsArr,pickable:z<6,radiusUnits:'meters',getPosition:d=>[d.lon,d.lat],getRadius:d=>d.rKm*1000,
    filled:z<7,getFillColor:d=>[...SEV_COL[d.sev],38],getLineColor:d=>[...SEV_COL[d.sev],210],stroked:true,lineWidthMinPixels:2,updateTriggers:{filled:[z<7]}}));
  if(S.alerts&&S.pulse){
    const ph=(performance.now()%2400)/2400;
    L.push(new D.ScatterplotLayer({id:'pulse',data:alertsArr,radiusUnits:'meters',getPosition:d=>[d.lon,d.lat],getRadius:d=>d.rKm*1000*(.5+ph*.9),
      filled:false,getLineColor:d=>[...SEV_COL[d.sev],Math.round(180*(1-ph))],stroked:true,lineWidthMinPixels:2,updateTriggers:{getRadius:[ph],getLineColor:[ph]}}));
  }
  if(S.links)L.push(new D.ArcLayer({id:'links',data:LINKS,pickable:true,greatCircle:true,getHeight:.12,getWidth:2,
    getSourcePosition:d=>d.a,getTargetPosition:d=>d.b,
    getSourceColor:d=>d.ok?[127,167,189,120]:[242,184,75,200],getTargetColor:d=>d.ok?[127,167,189,120]:[242,184,75,200]}));
  if(S.trails&&!clustered)L.push(new D.PathLayer({id:'trails',data:trailArr,getPath:d=>d.trail,getColor:d=>[...TYPE_COL[d.type],light?170:90],widthMinPixels:1.5}));
  if(S.hubs){
    L.push(new D.ScatterplotLayer({id:'hubs',data:HUBDATA,getPosition:d=>d.pos,radiusUnits:'pixels',getRadius:5,filled:false,stroked:true,getLineColor:light?[30,60,80,230]:[143,169,184,200],lineWidthMinPixels:1.5}));
    L.push(new D.TextLayer({id:'hublabels',data:HUBDATA,getPosition:d=>d.pos,getText:d=>d.name,getSize:13,getColor:light?[20,40,55,255]:[200,216,225,235],getPixelOffset:[0,-16],
      fontFamily:FONT,fontSettings:{sdf:true},outlineWidth:2,outlineColor:light?[240,245,248,235]:[11,22,32,235]}));
  }
  const glide=S.interp&&!clustered?{getPosition:{duration:S.transMs,easing:t=>t*(2-t)}}:{getPosition:0};
  L.push(new D.ScatterplotLayer({id:'assets-'+epoch,data:singles,pickable:true,radiusUnits:'pixels',getPosition:d=>d.pos,getRadius:d=>d.id===selId?10:6.5,
    getFillColor:assetColor,stroked:true,getLineColor:[11,22,32,230],lineWidthMinPixels:1.5,transitions:glide,
    updateTriggers:{getFillColor:[staleTick,selId,assetsArr],getRadius:[selId]}}));
  if(S.assetLabels&&!clustered&&z>=9){
    L.push(new D.TextLayer({id:'assetlabels',data:visibleAssets(),getPosition:d=>d.pos,getText:d=>d.id,getSize:12,getColor:light?[20,40,55,255]:[220,232,238,255],getPixelOffset:[0,18],
      fontFamily:FONT,fontSettings:{sdf:true},outlineWidth:2,outlineColor:light?[240,245,248,235]:[11,22,32,235]}));
  }
  if(clusters.length){
    L.push(new D.ScatterplotLayer({id:'clusters',data:clusters,pickable:true,radiusUnits:'pixels',getPosition:d=>d.pos,getRadius:d=>14+Math.min(18,Math.sqrt(d.count)*3.2),
      getFillColor:[22,64,84,235],stroked:true,getLineColor:[82,199,232,255],lineWidthMinPixels:2}));
    L.push(new D.TextLayer({id:'clustercount',data:clusters,getPosition:d=>d.pos,getText:d=>String(d.count),getSize:14,getColor:[235,245,250,255],
      fontFamily:FONT,fontWeight:600,getTextAnchor:'middle',getAlignmentBaseline:'center'}));
  }
  if(S.pins){
    L.push(new D.ScatterplotLayer({id:'pins',data:alertsArr,pickable:true,radiusUnits:'pixels',getPosition:d=>[d.lon,d.lat],getRadius:9,
      getFillColor:d=>[...SEV_COL[d.sev],255],stroked:true,getLineColor:[255,255,255,255],lineWidthMinPixels:2.5}));
    if(z>=7)L.push(new D.TextLayer({id:'pinlabels',data:alertsArr,getPosition:d=>[d.lon,d.lat],getText:d=>d.title,getSize:13,getColor:light?[20,40,55,255]:[255,235,225,255],getPixelOffset:[0,22],
      fontFamily:FONT,fontSettings:{sdf:true},outlineWidth:2,outlineColor:light?[240,245,248,235]:[11,22,32,235]}));
  }
  const sel=selId&&assets.get(selId);
  if(sel&&!clustered)L.push(new D.ScatterplotLayer({id:'selring',data:[sel],radiusUnits:'pixels',getPosition:d=>d.pos,getRadius:18,filled:false,stroked:true,
    getLineColor:selId===trackId?[242,184,75,255]:[255,255,255,230],lineWidthMinPixels:2,transitions:{getPosition:S.interp?S.transMs:0},updateTriggers:{getLineColor:[trackId]}}));
  if(pinned)L.push(new D.ScatterplotLayer({id:'pinring',data:[pinned],radiusUnits:'pixels',getPosition:d=>[d.lon,d.lat],getRadius:26,filled:false,stroked:true,
    getLineColor:[255,255,255,255],lineWidthMinPixels:3}));
  return L;
}
let rq=false;
function scheduleRender(){if(rq)return;rq=true;requestAnimationFrame(()=>{rq=false;overlay.setProps({layers:buildLayers()})})}

/* ================= picking, radial menu, pinpoint ================= */
function onPick(info){
  if(!info||!info.object||!info.layer){closeRadial();return}
  const id=info.layer.id;
  if(id.startsWith('assets-'))pickAsset(info.object,info.x,info.y);
  else if(id==='clusters')pickCluster(info.object);
  else if(id==='pins'||id==='alerts')selectAlert(info.object);
  else if(id==='links'){closeRadial();selLink=info.object;selAlert=null;selId=null;selCluster=null;openDrawer()}
}
function pickAsset(a,x,y){
  selId=a.id;selAlert=null;selLink=null;selCluster=null;scheduleRender();
  if(S.radial)openRadial(x,y,a);else openDrawer();
}
function pickCluster(c){
  closeRadial();
  if(S.clusterZoom)map.easeTo({center:c.pos,zoom:Math.min(map.getZoom()+2.2,9),duration:700});
  else{selCluster=c;selId=null;selAlert=null;selLink=null;openDrawer()}
}
function selectAlert(a){closeRadial();selAlert=a;selId=null;selLink=null;selCluster=null;openDrawer();scheduleRender()}
function pinpoint(a){
  selectAlert(a);pinned=a;
  map.flyTo({center:[a.lon,a.lat],zoom:17,duration:2600,essential:true});
  scheduleRender();
}
let ringTimer=null;
function closeRadial(){$('#ringHost').innerHTML='';clearTimeout(ringTimer)}
function openRadial(x,y,a){
  closeRadial();
  const t=S.target,R=t*1.1+30,W=innerWidth,H=innerHeight,m=R+t/2+16;
  const cx=clamp(x,m,W-m),cy=clamp(y,m,H-m);
  const el=document.createElement('div');el.className='ring';el.style.left=cx+'px';el.style.top=cy+'px';
  const items=[
    ['◎',trackId===a.id?'Untrack':'Track',()=>{trackId=trackId===a.id?null:a.id;toast(trackId?'Tracking '+a.id:'Stopped tracking');scheduleRender()}],
    ['⊘',a.iso?'Restore':'Isolate',()=>toggleIso(a.id)],
    ['≋','Ping',()=>{toast('Ping sent to '+a.id);setTimeout(()=>toast('Reply from '+a.id+' in '+(0.6+Math.random()*1.4).toFixed(1)+' s'),900+Math.random()*600)}],
    ['i','Details',()=>openDrawer()],
    ['×','Close',()=>{selId=null;scheduleRender()}]
  ];
  items.forEach((it,i)=>{
    const ang=(-90+i*72)*Math.PI/180,b=document.createElement('button');
    b.style.setProperty('--dx',(Math.cos(ang)*R).toFixed(1)+'px');b.style.setProperty('--dy',(Math.sin(ang)*R).toFixed(1)+'px');
    b.innerHTML='<span>'+it[0]+'</span><span class="l">'+it[1]+'</span>';
    b.addEventListener('click',e=>{e.stopPropagation();it[2]();closeRadial()});
    el.appendChild(b);
  });
  $('#ringHost').appendChild(el);
  requestAnimationFrame(()=>el.classList.add('on'));
  ringTimer=setTimeout(closeRadial,9000);
}
function toggleIso(id){
  const s=srv.byId.get(id),a=assets.get(id);if(!s||!a)return;
  s.iso=!s.iso;s.rev=++srv.rev;a.iso=s.iso;
  toast(a.iso?id+' isolated from network':id+' restored');scheduleRender();setTimeout(pull,0);
}
map.on('movestart',e=>{if(e.originalEvent)closeRadial()});

/* ================= details drawer ================= */
function openDrawer(){$('#drawer').classList.add('open');renderSel()}
function renderSel(){
  const b=$('#dbody');
  if(selId&&assets.get(selId)){
    const a=assets.get(selId),m=Math.round((simNow-a.upd)/60000);
    b.innerHTML='<div class="sel"><h2>'+esc(a.id)+'</h2><p>'+esc(a.type)+' near '+esc(a.hub)+'</p><div class="kv"><span>Last report</span><span>'+fmt(a.upd)+' ('+m+' min ago)</span><span>Reports every</span><span>'+a.every+' min</span><span>Position</span><span>'+a.pos[1].toFixed(4)+', '+a.pos[0].toFixed(4)+'</span><span>Status</span><span>'+(a.iso?'Isolated':'Connected')+'</span><span>Trail points</span><span>'+a.trail.length+'</span></div></div><div class="tools"><button id="selZoomA" class="pri">Zoom to asset</button><button id="selTrack">'+(trackId===a.id?'Stop tracking':'Track')+'</button><button id="selIso">'+(a.iso?'Restore':'Isolate')+'</button></div>';
  }else if(selAlert){
    const a=selAlert,nh=nearHub(a.lon,a.lat);
    b.innerHTML='<div class="sel"><h2>'+esc(a.title)+'</h2><p>'+esc(a.id)+' from '+esc(a.src)+'</p><div class="kv"><span>Severity</span><span>'+['','Low','Elevated','High'][a.sev]+'</span><span>Opened</span><span>'+fmt(a.created)+'</span><span>Epicenter</span><span>'+a.lat.toFixed(5)+', '+a.lon.toFixed(5)+'</span><span>Degrees</span><span>'+dms(a.lat,'N','S')+'<br>'+dms(a.lon,'E','W')+'</span><span>Nearest hub</span><span>'+esc(nh[0])+', '+nh[1]+' km</span><span>Zone radius</span><span>'+a.rKm+' km</span></div></div><div class="tools"><button id="selPin" class="pri">Pinpoint location</button><button id="selZoom">Show zone</button><button id="selCopy">Copy coordinates</button></div>';
  }else if(selLink){
    const l=selLink;
    b.innerHTML='<div class="sel"><h2>'+esc(l.an)+' to '+esc(l.bn)+'</h2><p>'+(l.ok?'Healthy link':'Degraded link')+'</p><h3>Depends on this link</h3>'+l.deps.map(d=>'<div class="li"><span>'+esc(d)+'</span><small>'+esc(l.an)+' to '+esc(l.bn)+'</small></div>').join('')+'</div>';
  }else if(selCluster){
    const c=selCluster;
    b.innerHTML='<div class="sel"><h2>'+c.count+' assets grouped</h2><p>Tap one to select it.</p></div>'+c.items.slice(0,80).map(a=>'<button class="li" data-id="'+esc(a.id)+'"><span>'+esc(a.id)+'</span><small>'+esc(a.type)+', '+esc(a.hub)+'</small></button>').join('');
  }else b.innerHTML='<div class="empty">Tap an asset, group, alert pin or network line on the map to see it here.</div>';
}
$('#dbody').addEventListener('click',e=>{
  const t=e.target.closest('button');if(!t)return;
  if(t.id==='selTrack'){trackId=trackId===selId?null:selId;scheduleRender();renderSel();return}
  if(t.id==='selIso'){toggleIso(selId);renderSel();return}
  if(t.id==='selZoomA'){const a=assets.get(selId);if(a)map.flyTo({center:a.pos,zoom:16,duration:2200});return}
  if(t.id==='selPin'){pinpoint(selAlert);return}
  if(t.id==='selZoom'){map.fitBounds(zoneBounds(selAlert),{padding:60,duration:1200});return}
  if(t.id==='selCopy'){const txt=selAlert.lat.toFixed(5)+', '+selAlert.lon.toFixed(5);try{navigator.clipboard.writeText(txt).then(()=>toast('Copied '+txt),()=>toast(txt))}catch(_){toast(txt)}return}
  if(t.dataset.id){const a=assets.get(t.dataset.id);selId=a.id;selCluster=null;map.easeTo({center:a.pos,zoom:Math.max(map.getZoom(),6),duration:800});scheduleRender();renderSel()}
});
function zoneBounds(a){const dLat=a.rKm/111,dLon=a.rKm/(111*Math.max(.2,Math.cos(a.lat*Math.PI/180)));return[[a.lon-dLon,clamp(a.lat-dLat,-85,85)],[a.lon+dLon,clamp(a.lat+dLat,-85,85)]]}
$('#dclose').addEventListener('click',()=>{$('#drawer').classList.remove('open');pinned=null;scheduleRender()});

/* ================= bottom dock and option popovers ================= */
let openCat=null;
$('#dock').innerHTML=CATS.map(c=>'<button class="dk" data-cat="'+c.id+'" aria-label="'+c.name+'"><svg viewBox="0 0 24 24"><path d="'+c.icon+'"/></svg><span>'+c.name+'</span>'+(c.id==='incidents'?'<b class="bd" id="badge"></b>':'')+'</button>').join('');
$('#dock').addEventListener('click',e=>{
  const b=e.target.closest('.dk');if(!b)return;
  if(openCat===b.dataset.cat){closePop();return}
  openPop(b.dataset.cat);
});
function updateBadge(){const b=$('#badge');if(b)b.textContent=alertsArr.length?String(alertsArr.length):''}
function openPop(id){openCat=id;renderPop();placePop()}
function closePop(){openCat=null;const p=$('#pop');p.classList.remove('on');p.innerHTML='';hideTip();document.querySelectorAll('.dk').forEach(b=>b.classList.remove('on'))}
function placePop(){
  const p=$('#pop'),btn=document.querySelector('.dk[data-cat="'+openCat+'"]');
  document.querySelectorAll('.dk').forEach(b=>b.classList.toggle('on',b.dataset.cat===openCat));
  if(!btn)return;
  const r=btn.getBoundingClientRect(),w=Math.min(370,innerWidth-16);
  p.style.left=clamp(r.left+r.width/2-w/2,8,innerWidth-w-8)+'px';
}
function renderPop(){
  const c=CATS.find(x=>x.id===openCat),p=$('#pop');if(!c)return;
  const keep=p.scrollTop;
  let h='<div class="ph"><span>'+c.name+'</span><button id="popx" aria-label="Close">×</button></div>';
  if(c.list){
    h+=alertsArr.length?alertsArr.map(a=>{const nh=nearHub(a.lon,a.lat);return '<button class="inc" data-inc="'+esc(a.id)+'"><i style="background:rgb('+SEV_COL[a.sev]+')"></i><span><b>'+esc(a.title)+'</b><small>Near '+esc(nh[0])+', opened '+fmt(a.created)+'</small></span></button>'}).join(''):'<div class="note">No open incidents right now.</div>';
    h+='<div class="note">Tap an incident to fly to its exact epicenter. You can zoom in to street and rooftop level from there.</div>';
  }else if(c.tools){
    h+='<div class="btns"><button data-preset="all">Everything on</button><button data-preset="balanced">Balanced</button><button data-preset="stable">Max stability</button></div>';
    h+='<div class="btns"><button id="selfcheck">Run self-check</button><button id="outage">Drop link 30 s</button><button id="exportCfg">Show config</button></div><div id="toolOut"></div>';
  }else{
    for(const o of c.opts){
      const [k,type,label,hint,choices]=o;
      if(type==='toggle')h+='<label class="opt" data-tip="'+esc(hint)+'"><input type="checkbox" data-k="'+k+'"'+(S[k]?' checked':'')+'><span class="t"><b>'+label+'</b><small>'+esc(hint)+'</small></span></label>';
      else h+='<div class="opt choice" data-tip="'+esc(hint)+'"><span class="t"><b>'+label+'</b><small>'+esc(hint)+'</small></span><span class="seg" data-k="'+k+'">'+choices.map(x=>'<button data-v="'+x[0]+'" class="'+(String(S[k])===String(x[0])?'on':'')+'">'+x[1]+'</button>').join('')+'</span></div>';
    }
  }
  p.innerHTML=h;p.classList.add('on');p.scrollTop=keep;
}
const pop=$('#pop');
pop.addEventListener('change',e=>{const k=e.target.dataset&&e.target.dataset.k;if(!k)return;S[k]=e.target.checked;apply(k)});
pop.addEventListener('click',e=>{
  const t=e.target.closest('button');if(!t)return;
  if(t.id==='popx'){closePop();return}
  if(t.dataset.inc){const a=alertsOpen.get(t.dataset.inc);if(a)pinpoint(a);return}
  if(t.dataset.preset){Object.assign(S,PRESETS[t.dataset.preset]);applyAll();toast('Preset applied');return}
  if(t.id==='selfcheck'){showSelfCheck();return}
  if(t.id==='outage'){dropLink();return}
  if(t.id==='exportCfg'){
    const txt=JSON.stringify(S,null,1);$('#toolOut').innerHTML='<textarea readonly></textarea>';const ta=$('#toolOut textarea');ta.value=txt;ta.select();
    try{navigator.clipboard.writeText(txt).then(()=>toast('Config copied'),()=>{})}catch(_){}return}
  const seg=t.parentElement;
  if(seg&&seg.classList.contains('seg')){
    const k=seg.dataset.k,raw=t.dataset.v;S[k]=isNaN(Number(raw))?raw:Number(raw);
    seg.querySelectorAll('button').forEach(b=>b.classList.toggle('on',b===t));apply(k);
  }
});
/* hover tip: one plain sentence per option */
const tipEl=$('#tip');
function hideTip(){tipEl.style.display='none'}
pop.addEventListener('mouseover',e=>{
  const o=e.target.closest('.opt');if(!o||!o.dataset.tip){return hideTip()}
  tipEl.textContent=o.dataset.tip;tipEl.style.display='block';
  const r=o.getBoundingClientRect(),pr=pop.getBoundingClientRect(),tw=tipEl.offsetWidth||250;
  let left=pr.right+10;if(left+tw>innerWidth-8)left=Math.max(8,pr.left-tw-10);
  tipEl.style.left=left+'px';tipEl.style.top=clamp(r.top+r.height/2-20,8,innerHeight-80)+'px';
});
pop.addEventListener('mouseleave',hideTip);
addEventListener('keydown',e=>{if(e.key==='Escape'){closePop();$('#drawer').classList.remove('open')}});

/* ================= tools: outage and self-check ================= */
function dropLink(){
  linkUp=false;setChip();toast('Link dropped for 30 s');
  setTimeout(()=>{linkUp=true;pull();toast('Link restored. '+lastPayload)},30000);
}
function selfCheck(){
  const r=[],z0=map.getZoom();
  r.push([!!map&&typeof map.loaded==='function','Map engine started','']);
  r.push([assets.size===360,'Data loaded',assets.size+' assets, '+alertsOpen.size+' open alerts']);
  r.push([lastFullBytes>0&&(lastDeltaBytes===0||lastDeltaBytes<lastFullBytes),'Delta smaller than full set',lastDeltaBytes?(lastDeltaBytes+' B vs '+lastFullBytes+' B'):'no delta received yet']);
  const maxTrail=assetsArr.reduce((m,a)=>Math.max(m,a.trail.length),0);
  r.push([maxTrail<=S.trailLen,'Trail memory capped','longest '+maxTrail+', limit '+S.trailLen]);
  r.push([map.getMinZoom()<=1&&map.getMaxZoom()>=19,'Zoom range world to street','zoom '+map.getMinZoom()+' to '+map.getMaxZoom()]);
  const sv=wasClustered,sc=singles.length+clusters.length;
  r.push([!S.cluster||z0>=CLUSTER_MAX_ZOOM||sc<assetsArr.length,'Grouping reduces icons at this zoom',sc+' items for '+assetsArr.length+' assets']);
  r.push([S.baseEngine==='off'||!S.basemap||tilesOk+glOk>0,'Map imagery loading','loader '+S.baseEngine+', loaded '+(tilesOk+glOk)+', failed '+(tilesBad+tileErrors)+(tilesOk+glOk===0?'. Try the other loader in Map.':'')]);
  r.push([fps===0||fps>=20,'Frame rate',fps+' fps, worst frame '+worst+' ms']);
  r.push([alertsArr.length>0,'Alert pins available',alertsArr.length+' open']);
  return r;
}
function showSelfCheck(){
  const rows=selfCheck();
  $('#toolOut').innerHTML=rows.map(x=>'<div class="chk"><span class="'+(x[0]?'ok':'bad')+'">'+(x[0]?'Pass':'Check')+'</span>  '+esc(x[1])+(x[2]?'<br><small style="color:var(--dim)">'+esc(x[2])+'</small>':'')+'</div>').join('');
}

/* ================= applying settings ================= */
let pulseTimer=null;
function apply(k){
  if(k==='basemap'||k==='basemapStyle'||k==='placeNames'||k==='baseEngine'||k==='outline'){applyBase();if(k==='basemapStyle')scheduleRender()}
  if(k==='lockRot'){
    if(S.lockRot){map.dragRotate.disable();map.touchZoomRotate.disableRotation();map.touchPitch.disable();map.setPitch(0);map.setBearing(0)}
    else{map.dragRotate.enable();map.touchZoomRotate.enableRotation();map.touchPitch.enable()}
  }
  if(k==='target'){document.documentElement.style.setProperty('--t',S.target+'px');overlay.setProps({pickingRadius:S.target/2})}
  if(k==='dprCap'){const pr=S.dprCap?Math.min(devicePixelRatio||1,1.5):(devicePixelRatio||1);try{map.setPixelRatio(pr)}catch(_){}try{overlay.setProps({_pixelRatio:pr})}catch(_){}}
  if(k==='pulse'||k==='pulseFps'){clearInterval(pulseTimer);if(S.pulse)pulseTimer=setInterval(scheduleRender,1000/S.pulseFps)}
  if(k==='cluster'){computeClusters()}
  if(k==='trailLen'||k==='retention')gc();
  if(k==='hud')$('#hud').classList.toggle('off',!S.hud);
  if(k==='readout')document.body.classList.toggle('noreadout',!S.readout);
  if(k==='delta')synced=false;
  scheduleRender();
}
function applyAll(){KEYS.forEach(apply);if(openCat)renderPop()}

/* ================= housekeeping ================= */
let purged=0;
function gc(){
  const cutoff=simNow-S.retention*3600e3;
  for(const a of assets.values()){
    while(a.trail.length>S.trailLen||(a.tt.length>1&&a.tt[0]<cutoff)){a.trail.shift();a.tt.shift();purged++}
  }
  while(closedLog.length&&closedLog[0].closedAt<cutoff){closedLog.shift();purged++}
}
setInterval(gc,30000);

/* ================= chip, hud, readout, toast ================= */
function setChip(){
  const c=$('#chip');
  if(linkUp){c.classList.remove('lost');$('#chipA').textContent='Live';$('#chipB').textContent='Synced '+fmt(lastSyncT)+', clock '+fmt(simNow)}
  else{c.classList.add('lost');$('#chipA').textContent='Link lost';$('#chipB').textContent='Showing data from '+fmt(lastSyncT)}
}
let fps=0,worst=0;
function updateHud(){
  if(!S.hud)return;
  const mem=performance.memory?Math.round(performance.memory.usedJSHeapSize/1048576)+' MB':'n/a';
  $('#hud').innerHTML='<b>'+fps+' fps</b>, worst frame '+worst+' ms<br>'+assetsArr.length+' assets, '+alertsArr.length+' open alerts, '+clusters.length+' groups<br>Last sync: '+(lastPayload||'none')+'<br>Memory '+mem+', history '+closedLog.length+', purged '+purged+
   '<div class="k"><span><i style="background:#52C7E8"></i>Vehicle</span><span><i style="background:#F3F7F9"></i>Personnel</span><span><i style="background:#B7A6FF"></i>Sensor</span><span><i style="background:#FF6B57"></i>Alert</span></div>';
}
let last=performance.now(),frames=0,w=0,acc=0;
function mon(t){const d=t-last;last=t;frames++;if(d>w)w=d;acc+=d;if(acc>=1000){fps=Math.round(frames*1000/acc);worst=Math.round(w);frames=0;w=0;acc=0;updateHud()}requestAnimationFrame(mon)}
requestAnimationFrame(mon);
let tt=null;
function toast(m){const t=$('#toast');t.textContent=m;t.classList.add('on');clearTimeout(tt);tt=setTimeout(()=>t.classList.remove('on'),2800)}
let cq=false;
function updateCoord(){
  if(cq)return;cq=true;requestAnimationFrame(()=>{cq=false;const c=map.getCenter();$('#coordtxt').textContent=c.lat.toFixed(4)+', '+c.lng.toFixed(4)+'   zoom '+map.getZoom().toFixed(1)})
}
map.on('move',updateCoord);
$('#goto').addEventListener('keydown',e=>{
  if(e.key!=='Enter')return;
  const m=String($('#goto').value).match(/(-?\d+(?:\.\d+)?)\s*[, ]\s*(-?\d+(?:\.\d+)?)/);
  if(!m||Math.abs(+m[1])>85||Math.abs(+m[2])>180){toast('Enter coordinates as latitude, longitude');return}
  map.flyTo({center:[+m[2],+m[1]],zoom:16,duration:2200,essential:true});$('#goto').blur();
});
let tileToast=0;
map.on('error',()=>{tileErrors++;if(tileErrors===1||Date.now()-tileToast>60000){tileToast=Date.now();toast('Some map tiles did not load. Try another base map in Map.')}});

/* ================= loops ================= */
let lastReal=Date.now();
setInterval(()=>{
  const now=Date.now(),dt=Math.min(now-lastReal,60000);lastReal=now;
  simNow+=dt*S.speed;serverAdvance(simNow);
},1000);
setInterval(pull,4000);
setInterval(()=>{staleTick++;if(S.staleDim)scheduleRender();setChip()},15000);
let lastTouch=Date.now();
['pointerdown','wheel','keydown'].forEach(ev=>addEventListener(ev,()=>{lastTouch=Date.now()},{passive:true,capture:true}));
setInterval(()=>{
  if(S.idleReset&&Date.now()-lastTouch>S.idleSec*1000){
    lastTouch=Date.now();
    if(!trackId&&!pinned&&!$('#drawer').classList.contains('open')){goHome();selId=null;scheduleRender()}
  }
},5000);
function goHome(){const h=homeView();map.easeTo({center:h.center,zoom:h.zoom,duration:900});closeRadial()}
$('#zoomIn').addEventListener('click',()=>map.zoomIn({duration:300}));
$('#zoomOut').addEventListener('click',()=>map.zoomOut({duration:300}));
$('#home').addEventListener('click',goHome);
map.on('zoomend',()=>{computeClusters();scheduleRender()});
map.on('moveend',()=>{if(map.getZoom()>=9)scheduleRender()});
map.on('style.load',applyBase);
map.on('data',e=>{if(e.dataType==='source'&&e.tile)glOk++});

/* ================= start ================= */
serverAdvance(simNow);
document.documentElement.style.setProperty('--t',S.target+'px');
applyAll();
pull();
updateCoord();
window.__ops={S,PRESETS,CATS,pull,buildLayers,computeClusters,openPop,closePop,renderPop,onPick,pinpoint,selfCheck,apply,applyAll,serverAdvance,
  get state(){return{assets:assets.size,alerts:alertsArr.length,singles:singles.length,clusters:clusters.length,epoch,lastPayload,simNow}},
  setSim(t){simNow=t}};
};
</script>
<script>
(function(){
var log=[],fail=document.getElementById('fail'),err=document.getElementById('err');
function showErr(m){err.style.display='block';err.textContent=m+'\n'+err.textContent.slice(0,600)}
window.addEventListener('error',function(e){showErr('Error: '+(e.message||'unknown')+(e.lineno?' (line '+e.lineno+')':''))});
window.addEventListener('unhandledrejection',function(e){showErr('Error: '+(e.reason&&e.reason.message||e.reason))});
function load(u,ok,bad){var s=document.createElement('script');s.src=u;s.onload=function(){log.push('loaded '+u);ok()};s.onerror=function(){log.push('blocked or missing '+u);bad()};document.head.appendChild(s)}
function css(u){var l=document.createElement('link');l.rel='stylesheet';l.href=u;document.head.appendChild(l)}
function first(list,check,done){var i=0;(function next(){if(check())return done(true);if(i>=list.length)return done(false);var c=list[i++];load(c.js,function(){if(check()){if(c.css)css(c.css);done(true)}else next()},next)})()}
var ML=[],DK=[],cd='https://cdnjs.cloudflare.com/ajax/libs/',jd='https://cdn.jsdelivr.net/npm/',up='https://unpkg.com/';
['4.7.1','4.5.0','3.6.2','2.4.0'].forEach(function(v){ML.push({js:cd+'maplibre-gl/'+v+'/maplibre-gl.min.js',css:cd+'maplibre-gl/'+v+'/maplibre-gl.min.css'})});
ML.push({js:jd+'maplibre-gl@4.7.1/dist/maplibre-gl.js',css:jd+'maplibre-gl@4.7.1/dist/maplibre-gl.css'});
ML.push({js:up+'maplibre-gl@4.7.1/dist/maplibre-gl.js',css:up+'maplibre-gl@4.7.1/dist/maplibre-gl.css'});
['8.9.35','8.9.34','8.9.33','8.8.27'].forEach(function(v){DK.push({js:cd+'deck.gl/'+v+'/dist.min.js'})});
DK.push({js:jd+'deck.gl@8.9.35/dist.min.js'});DK.push({js:up+'deck.gl@8.9.35/dist.min.js'});
first(ML,function(){return !!window.maplibregl},function(a){
  first(DK,function(){return !!(window.deck&&window.deck.MapboxOverlay)},function(b){
    if(!a||!b){
      fail.style.display='grid';
      fail.innerHTML='<div><b>The map libraries could not load.</b><br><br>Map engine: '+(a?'ok':'missing')+'<br>Overlay engine: '+(b?'ok':'missing')+'<br><br><small style="color:#8FA9B8;white-space:pre-line">'+log.join('\n')+'</small></div>';
      return;
    }
    fail.style.display='none';
    try{window.__start()}catch(e){showErr('Start failed: '+e.message)}
  });
});
})();
</script>
</body>
</html>



- [Get started](/docs/get-started/using-standalone.md#using-the-scripting-api)
- [Full examples](https://github.com/visgl/deck.gl/tree/master/examples/get-started/scripting)

### NPM Module

```bash
npm install deck.gl
```

#### Pure JS

- [Get started](/docs/get-started/using-standalone.md)
- [Full examples](/examples/get-started/pure-js)

#### React

- [Get started](/docs/get-started/using-with-react.md)
- [Full examples](/examples/get-started/react)

### Python

```bash
pip install pydeck
```

- [Get started](https://deckgl.readthedocs.io/en/latest/installation.html)
- [Examples](https://deckgl.readthedocs.io/en/latest/layer.html)

### Third-Party Goodies

- [deckgl-typings](https://github.com/danmarshall/deckgl-typings) (Typescript)
- [mapdeck](https://symbolixau.github.io/mapdeck/articles/mapdeck.html) (R)
- [vega-deck.gl](https://github.com/microsoft/SandDance/tree/master/packages/vega-deck.gl) ([Vega](https://vega.github.io/))
- [earthengine-layers](https://earthengine-layers.com/) ([Google Earth Engine](https://earthengine.google.com/))
- [deck.gl-native](https://github.com/UnfoldedInc/deck.gl-native) (C++)
- [deck.gl-raster](https://github.com/kylebarron/deck.gl-raster/) (Computation on rasters)

## Learning Resources

* [API documentation](https://deck.gl/docs) for the latest release
* [Website demos](https://deck.gl/examples) with links to source
* [Interactive playground](https://deck.gl/playground)
* [deck.gl Codepen demos](https://codepen.io/vis-gl/)
* [deck.gl Observable demos](https://beta.observablehq.com/@pessimistress)
* [vis.gl Medium blog](https://medium.com/vis-gl)
* [deck.gl Slack workspace](https://slack-invite.openjsf.org/)

## Contributing

deck.gl is part of vis.gl, an [OpenJS Foundation](https://openjsf.org/) project. Read the [contribution guidelines](/CONTRIBUTING.md) if you are interested in contributing.


## Attributions

#### Data sources

Data sources are listed in each example.


#### The deck.gl project is supported by

<a href="https://www.unfolded.ai"><img src="https://raw.githubusercontent.com/visgl/deck.gl-data/master/images/branding/unfolded.png" height="32" /></a>
<a href="https://www.foursquare.com"><img src="https://raw.githubusercontent.com/visgl/deck.gl-data/master/images/branding/fsq.svg" height="40" /></a>

<a href="https://www.carto.com"><img src="https://raw.githubusercontent.com/visgl/deck.gl-data/master/images/branding/carto.svg" height="48" /></a>

<a href="https://www.mapbox.com"><img src="https://raw.githubusercontent.com/visgl/deck.gl-data/master/images/branding/mapbox.svg" height="44" /></a>
<a href="https://www.uber.com"><img src="https://raw.githubusercontent.com/visgl/deck.gl-data/master/images/branding/uber.png" height="40" /></a>

<a href="https://www.browserstack.com/"><img src="https://d98b8t1nnulk5.cloudfront.net/production/images/static/logo.svg" alt="BrowserStack" width="200" /></a>
