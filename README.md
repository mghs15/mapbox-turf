# Mapbox GL JS上でTurf.jsを利用する
* 地理院地図Vectorで提供実験されているベクトルタイルを用いて、Turf.jsで空間解析を行うサンプルサイトを作成した。
* 抽出対象は、タイルに存在するものではなく、画面内でスタイルでレンダリングされたものとなる。
* Mapbox GL JSのqueryRenderedFeaturesをそのまま利用すると、Turf.jsの処理（ここでは、nearestPoint）でエラーが生じたので、Turf.jsのturf.point、turf.featureCollection等で、データの形を整えてあげる必要があるようだ。

The example use case of turf js on mapbox gl js using gsimaps vector according to a [tutorial](https://docs.mapbox.com/help/tutorials/analysis-with-turf/).

## サンプル
* index.html
  - Mapboxのチュートリアルを地理院地図Vectorのベクトルタイルに適用。クリック地点に最も近い、郵便局を強調表示する。
* school.html
  - 学校（高等学校・中学校・小学校）の位置をもとにボロノイ図を作成。また、地図上をクリックすると、最も近い学校を強調表示する。
* isolines.html
  - 標高値を持つポイントから、IDWを用いた内挿により、ポイントグリッドを作成。ポイントグリッドからisolines（等値線）を表示する。
* isobands.html
  - 標高値を持つポイントから、IDWを用いた内挿により、ポイントグリッドを作成。ポイントグリッドからisobands（等値線の間の部分をポリゴンとしたもの）を表示する。表示の際は、fill-extrusionを用いて、標高値を視覚的に表現。
* tin.html
  - 標高値を持つポイントから、TINを発生させて表示する。表示の際は、fill-extrusionを用いて、標高値を視覚的に表現。
* centroid.html
  - 建物の各ポリゴンデータから代表点を取得。Turf.jsのクリッピング機能（）を利用し、代表点が表示画面内に入っているか判定し、入っているものとはみ出しているものを分別して表示。

## 参考文献
* チュートリアル（Analyze data with Turf.js and Mapbox GL JS） <br> https://docs.mapbox.com/help/tutorials/analysis-with-turf/
* Turf#nearestPoint <br> http://turfjs.org/docs/#nearestPoint
* Mapbox GL JS API Referece (queryRenderedFeatures) <br> https://docs.mapbox.com/mapbox-gl-js/api/#map#queryrenderedfeatures
* 地理院地図Vector <br> https://github.com/gsi-cyberjapan/gsimaps-vector-experiment
