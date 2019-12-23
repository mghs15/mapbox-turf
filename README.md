# Mapbox GL JS上でTurf.jsを利用する
* Mapboxのチュートリアル（図書館に最も近い病院を強調表示）を改造して、クリック地点から最も近い郵便局を、地理院地図Vectorで提供実験されているベクトルタイルから抽出して強調表示するサイトを作成した。
* 抽出対象は、タイルに存在するものではなく、画面内でスタイルでレンダリングされたものとなる。
* Mapbox GL JSのqueryRenderedFeaturesをそのまま利用すると、Turf.jsの処理（ここでは、nearestPoint）でエラーが生じたので、Turf.jsのturf.point、turf.featureCollectionで、データの形を整えてあげる必要があるようだ。

The example use case of turf js on mapbox gl js using gsimaps vector according to a [tutorial](https://docs.mapbox.com/help/tutorials/analysis-with-turf/).

## 参考文献
* チュートリアル（Analyze data with Turf.js and Mapbox GL JS） <br> https://docs.mapbox.com/help/tutorials/analysis-with-turf/
* Turf#nearestPoint <br> http://turfjs.org/docs/#nearestPoint
* Mapbox GL JS API Referece (queryRenderedFeatures) <br> https://docs.mapbox.com/mapbox-gl-js/api/#map#queryrenderedfeatures
* 地理院地図Vector <br> https://github.com/gsi-cyberjapan/gsimaps-vector-experiment
