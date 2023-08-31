# multi-pmtiles-in-single-source
複数のPMTilesを１つのSourceとしてWeb地図で扱う

blog(Qiita): https://qiita.com/mg_kudo/items/6fa899f4532ba2fe879b

## デモサイト
https://mghs15.github.io/multi-pmtiles-in-single-source/

タイル座標のうち X 座標と Y 座標の合計値が2で割り切れるかどうかで、2種類の PMTiles のどちらを読み込むか変更している。

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
  * 加工方法は [こちら](https://github.com/mghs15/flagment-inter-station) を参照
