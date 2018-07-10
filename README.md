WalkMyAndroidPlaces - Starter Code
=============================

Starter code for the WalkMyAndroidPlaces app, which gets information about
places using the Google Places API.

Introduction
------------
The final version of this app has a button that starts tracking the device
location. It then gets updates about the location using the Places API. It also
includes a button that launches the Place Picker, which allows the user to
search for a place instead of using the device location.

Pre-requisites
--------------

You should be familiar with:
- Creating, building, and running apps in Android Studio.
- The Activity lifecycle.
- Persisting data across configuration changes.
- Using an AsyncTask to do background work.
- Requesting permissions at runtime.


Getting Started
---------------

1. Download the code.
2. Open the code in Android Studio.
3. Run the app.

License
-------

Copyright 2017 Google, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.

Additional notes
-------

This is a personal note.

##### 1. APIキーの準備
1. プロジェクト作成
2. APIキー作成
    1. APIごとではなく、プロジェクト毎
    2. Android用に制限
        1. パッケージ名
        1. APIキーを取得
        2. 電子署名(SHA-1)
2. APIキーをManifestファイルにmeta-dataタグで追加
3. MainActivityのonCreateの中でPlaceDetectionClientをmenber変数にセットしておく

##### 2. locationからplace nameを取得する
0. 前提
    1. PlaceDetectionClient.getCurrentPlace()を使う
        1. 戻り値のTaskは、非同期処理を表し、パラメーターの型を持つ。
        1. PlaceLikelihoodBufferResponseは"likelihood"を持ったPlaceオブジェクト
            1. likelihoodは確率（0-1の値）
            1. 現在の地点である確率が高い場所を選ぶ設計
    2. getCurrentPlace()はユーザーが権限を与えなかった時に、チェック例外のSecurityExceptionを投げる
    3.  FetchAddressTask(AsyncTask)のイベントハンドラonTaskCompleted()内でさらに、Taskのイベントリスナー(OnCompleteListener)をセットする、その中で確立の最も高い場所をループで選定し、その場所を表示する
1. 手順
    1. 一個目の非同期処理のイベントハンドラであるonTaskCompletedの中でPlaceDetectionClient.getCurrentPlace()を呼び、Taskを得る
        1. チェック例外の処理が必要
    2. TaskであるplaceResultにリスナーを追加
        1. **FetchAddressTaskの実装クラスであるMainActivityをFetchAddressTaskに渡してnewしたのってこれとほぼ同じことしてんだなと気づいた**
    3. 非同期が成功した場合に、最も確率が高いものをループで選定し、表示する。
    4. placeの情報を使ったら、bufferを解放する

##### 3. locationからplace typeを取得する
1. place nameとほぼ同じ

#### 4. place-picker UIを追加する
0. 前提
    1. ユーザーが場所を選択できるUI
    2. このUIはdialog
    3. PlacePicker.IntentBuilderオブジェクトで専用のintentを作成
1. 手順
    1. PlacePicker.IntentBuilderでintentを作成し、startActivityForResultで起動する
        1. ほぼこれだけ
    2. onActivityResult()で選択されたplaceを取得し処理