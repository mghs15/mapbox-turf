# Mapbox GL JS上でTurf.jsを利用する
地理院地図Vectorで提供実験されているベクトルタイルを用いて、Turf.jsで空間解析を行うサンプルサイトを作成した。
The example use case of turf js on mapbox gl js using gsimaps vector according to a [tutorial](https://docs.mapbox.com/help/tutorials/analysis-with-turf/).

## サンプル
* [index.html](https://mghs15.github.io/mapbox-turf/)
  - Mapboxのチュートリアルを地理院地図Vectorのベクトルタイルに適用。クリック地点に最も近い、郵便局を強調表示する。
  - `turf.nearestPoint`
* [voronoi.html](https://mghs15.github.io/mapbox-turf/voronoi.html)
  - 郵便局の位置をもとにボロノイ図を作成。
  - `turf.voronoi`
* [school.html](https://mghs15.github.io/mapbox-turf/school.html)
  - 学校（高等学校・中学校・小学校）の位置をもとにボロノイ図を作成。また、地図上をクリックすると、最も近い学校を強調表示する。
  - `turf.nearestPoint` `turf.voronoi`
* [isolines.html](https://mghs15.github.io/mapbox-turf/isolines.html)
  - 標高値を持つポイントから、IDWを用いた内挿により、ポイントグリッドを作成。ポイントグリッドからisolines（等値線）を表示する。
  - `turf.interpolate` `turf.isolines`
* [isobands.html](https://mghs15.github.io/mapbox-turf/isobands.html)
  - 標高値を持つポイントから、IDWを用いた内挿により、ポイントグリッドを作成。ポイントグリッドからisobands（等値線の間の部分をポリゴンとしたもの）を表示する。表示の際は、fill-extrusionを用いて、標高値を視覚的に表現。
  - `turf.interpolate` `turf.isobands` 
* [tin.html](https://mghs15.github.io/mapbox-turf/tin.html)
  - 標高値を持つポイントから、TINを発生させて表示する。表示の際は、fill-extrusionを用いて、標高値を視覚的に表現。
  - `turf.tin`
* [centroid.html](https://mghs15.github.io/mapbox-turf/centroid.html)
  - 建物の各ポリゴンデータから代表点を取得。Turf.jsのクリッピング機能（pointsWithinPolygon）を利用し、代表点が表示画面内に入っているか判定し、入っているものとはみ出しているものを分別して表示。
  - なお、`turf.centroid`は頂点で重みづけされているようである。ポリゴンの重心を出すには、`turf.centerOfMass`の方が良いかもしれない。
  - `turf.centroid` `turf.bboxPolygon` `turf.pointsWithinPolygon`
* [nearestPointOnLine.html](https://mghs15.github.io/mapbox-turf/nearestPointOnLine.html)
  - クリック地点に一番近い鉄道上のポイントを強調表示する。
  - turf.nearestPointは、対象にLineStringかMultiLineStringしかとれない。（featureCollectionを対象にできない。）
  - そのため、queryRenderedFeaturesで得られたデータの成形時に、ターゲットポイント（ここではクリック地点）から最短となる各(Multi)LineStringのポイントを計算する。最終的に、そのポイントのリストをfeatureCollectionにまとめて、nearestPointに渡し、その中からクリック地点に最も近い点を決定する。
  - `turf.nearestPoint` `turf.nearestPointOnLine`
* [buffer.html](https://mghs15.github.io/mapbox-turf/buffer.html])
  - 鉄道駅部分のラインデータにバッファを発生させる。
  - `turf.buffer`
* [clustersKmeans.html](https://mghs15.github.io/mapbox-turf/clustersKmeans.html)
  - kmeans法により、普通建物のクラスタリングを行う。クラスター数はクエリに`?kmeans=クラスター数`を指定できるようにしている。
  - また、クエリに`?maxd=距離（km）`を指定することで、指定距離をクラスター内の点間の最大距離とするDbscanのクラスタリングを行うようにしている。
  - 色分けは"case"で20色を指定しているので、クラスター数がそれを超えると、超えた分のクラスターは同じ色（薄い赤色）で表示される。なお、Dbscanのクラスタリングの場合、クラスターに入らなかった点は灰色で表示される。
  - `turf.clustersKmeans` `turf.clustersDbscan`
* [bboxClip.html](https://mghs15.github.io/mapbox-turf/bboxClip.html)
  - 地図上の2地点を指定し、bboxを生成し、そのbbox内に含まれるポリゴン（建物）をクリップ（bboxClip）する。
  - クリップしたポリゴンデータから、ポリゴンの面積を集計。
  - また、ポリゴンの重心（centerOfMass）を計算し、指定領域（bbox）内に重心が入る（pointsWithinPolygon）ポリゴン数を集計。
  - `turf.bboxClip` `turf.centerOfMass` `turf.pointsWithinPolygon`


## 注意すべき点など
* ベクトルタイルの地物はMapbox GL JSのqueryRenderedFeaturesで行う。解析対象は、画面内でレンダリングされたものとなる（タイルに入っているものがすべて解析対象となるわけではない）。
* Mapbox GL JSのqueryRenderedFeaturesをそのまま利用すると、Turf.jsの処理でエラーが生じたので、Turf.jsのturf.point、turf.featureCollection等で、データの形をfeatureやfertureCollection等に整えてあげる必要があるようだ。
* データを整えたり、属性値の加工を行うため、頻繁にFor文を回している。

## 参考文献
* Mapboxによるチュートリアル（Analyze data with Turf.js and Mapbox GL JS） <br> https://docs.mapbox.com/help/tutorials/analysis-with-turf/
* Turf <br> https://turfjs.org/docs/
* Mapbox GL JS API Referece (queryRenderedFeatures) <br> https://docs.mapbox.com/mapbox-gl-js/api/#map#queryrenderedfeatures
* 地理院地図Vector（仮称）提供実験 <br> https://github.com/gsi-cyberjapan/gsimaps-vector-experiment
