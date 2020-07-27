# PlayCanvas Dev Console

This is a little script I use to look at what assets are currently loaded in the scene and what the file size + VRAM is.

**Functions:**

`loadedAssets([ filter() ])` Show all assets currently loaded. Filter is optional.

`loadedAssetsOnlyVRAM([ minSize ])` Show only assets with VRAM (no materials, shaders, etc). minSize is for minimum VRAM bytes and is optional.

`loadedAssetsOnlyBigVRAM()` Basically `loadedAssetsOnlyVRAM()` with a minSize of `3MB`

**Examples:**

`loadedAssets()` (All loaded assets)

`loadedAssets(asset => asset.type === 'texture')` (Assets of type texture)

`loadedAssetsOnlyVRAM()` (Assets with VRAM)

`loadedAssetsOnlyVRAM(10000000)` (Assets with VRAM 10 MB and greater)

`loadedAssetsOnlyBigVRAM()` (Assets with VRAM 3 MB and greater)

**Output**

Output shows `[reference number] [filename] [asset type] [asset size] [asset VRAM]`

The asset object can be quickly viewed using the `asset` object via either the "reference number" or the asset name, ie `asset[38]` or `asset['Asset Name.png']`


**Paste this into a script or directly into the dev console on any PlayCanvas project**

```
function formatBytes(a,b=2){if(0===a)return"0 Bytes";const c=0>b?0:b,d=Math.floor(Math.log(a)/Math.log(1024));return parseFloat((a/Math.pow(1024,d)).toFixed(c))+" "+["Bytes","KB","MB","GB","TB","PB","EB","ZB","YB"][d];}
var asset={};var cs = { stat: 'background:#555;color:white;padding:.2em;border-radius:3px;', stat2: 'background:#333;color:white;padding:.2em;border-radius:3px;', stat3: 'background:#888;color:#222;padding:.2em;border-radius:3px;', };
function loadedAssets(e){asset={};for(let [k,s] of Object.entries(pc.Application.getApplication().assets.filter(function(s){return s.loaded&&(!e||e(s));}))){console.log("%c"+k+'\t'+s.name+" %c"+s.type+"%c %c"+(s.file&&s.file.size?formatBytes(s.file.size):"")+"%c %c"+(s.resource&&s.resource._gpuSize?formatBytes(s.resource._gpuSize):""),"",cs.stat2,"",cs.stat,"",cs.stat3);asset[k]=asset[s.name]=s;}}
function loadedAssetsOnlyVRAM(e){loadedAssets(function(s){return s.resource&&s.resource._gpuSize&&s.resource._gpuSize>=(e||0);});}
function loadedAssetsOnlyBigVRAM(){loadedAssetsOnlyVRAM(3e6);}
```

**Bookmarklet**

This script can be easily invoked by using it as a bookmark. This is done by creating a new bookmark and pasting the code below into the "URL". "Open" the bookmark in any tab that is running a PlayCanvas project and it sets up all the functions mentioned above, automatically runs `loadedAssetsOnlyVRAM()` as well.

```
javascript:function formatBytes(a,b=2){if(0===a)return"0 Bytes";const c=0>b?0:b,d=Math.floor(Math.log(a)/Math.log(1024));return parseFloat((a/Math.pow(1024,d)).toFixed(c))+" "+["Bytes","KB","MB","GB","TB","PB","EB","ZB","YB"][d];}var asset={};var cs = { stat: 'background:#555;color:white;padding:.2em;border-radius:3px;', stat2: 'background:#333;color:white;padding:.2em;border-radius:3px;', stat3: 'background:#888;color:#222;padding:.2em;border-radius:3px;', };function loadedAssets(e){asset={};for(let [k,s] of Object.entries(pc.Application.getApplication().assets.filter(function(s){return s.loaded&&(!e||e(s));}))){console.log("%c"+k+'\t'+s.name+" %c"+s.type+"%c %c"+(s.file&&s.file.size?formatBytes(s.file.size):"")+"%c %c"+(s.resource&&s.resource._gpuSize?formatBytes(s.resource._gpuSize):""),"",cs.stat2,"",cs.stat,"",cs.stat3);asset[k]=asset[s.name]=s;}}function loadedAssetsOnlyVRAM(e){loadedAssets(function(s){return s.resource&&s.resource._gpuSize&&s.resource._gpuSize>=(e||0);});}function loadedAssetsOnlyBigVRAM(){loadedAssetsOnlyVRAM(3e6);};loadedAssetsOnlyVRAM();
```

