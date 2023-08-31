# multi-pmtiles-in-single-source
複数のPMTilesを１つのSourceとしてWeb地図で扱う

## やり方
```
let protocol = new pmtiles.Protocol();
const urlConvert = (url) => {
  // convert url
  return url;
}
const myProtocol = (param, callback) => {
  console.log(param);
  // URL を変換
  if(param.type != "json"){
    param.url = urlConvert(param.url);
    console.log("converted:" + param.url);
  }
  return protocol.tile(param, callback);
}
maplibregl.addProtocol("multi-pmtiles", myProtocol);
```
```
map.addSource('my-pmtiles', {
  type: 'vector',
  tiles: ["multi-pmtiles://https://example.com/path/to/your.pmtiles/{z}/{x}/{y}/"],
  minzoom: 4,
  maxzoom: 10
});
map.addLayer({
  'source': 'my-pmtiles',
  ...
})
```
## 出典
* PMTiles https://github.com/protomaps/PMTiles
* MapLibre GL JS https://github.com/maplibre/maplibre-gl-js
* 国土地理院最適化ベクトルタイル https://github.com/gsi-cyberjapan/optimal_bvmap
* 国土数値情報（鉄道データ） https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-N02-v2_3.html
