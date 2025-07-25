### フォトレイアウトツール 開発ガイド

#### 1. 概要

このツールは、複数の写真画像を一枚のキャンバスに読み込み、まるで机の上やコルクボードに写真を飾ったかのようなレイアウトを作成するためのWebアプリケーションです。ユーザーは写真の位置、角度、サイズを直感的に調整し、完成した画像をPNGファイルとしてダウンロードできます。

すべての機能は単一のHTMLファイル (`photo-layout-tool.html`) に実装されており、サーバーは不要です。

#### 2. ファイル構成

- `photo-layout-tool.html`: アプリケーションの本体。HTML、CSS、JavaScriptのすべてを含みます。
- `README.md`: このファイル。

#### 3. 主な機能

コントロールパネルは、機能ごとにグループ化されています。

- **画像読み込み:**
    - **ファイル選択:** 複数の画像ファイル（JPG, PNG, GIFなど）を一度に選択して読み込めます。
    - **ドラッグ＆ドロップ:** キャンバスエリアに画像ファイルを直接ドラッグ＆ドロップして追加することも可能です。
    - **初期サイズ:** 読み込み時に、指定された「初期サイズ」（小/中/大）に合わせて画像がリサイズされます。
    - **自動配置:** 新しく追加された画像は、既存の画像との重なりが最小になるよう、自動的に最適な位置に配置されます。

- **画像操作:**
    - **選択:** クリックした画像が選択され、操作対象になります。選択された画像は最前面に表示されます。
    - **移動:** 画像をドラッグ＆ドロップして自由に移動できます。
    - **回転:** 選択した画像の右上にある**赤い円形ハンドル**をドラッグして回転させます。
    - **リサイズ:** 選択した画像の右下にある**青い四角ハンドル**をドラッグして拡大・縮小します。

- **カスタマイズ＆エフェクト:**
    - **白い枠:** 各画像に白い枠と影を付けるかどうかのON/OFFを切り替えられます。
    - **画鋲:** 画像の上部中央に、画鋲で留めているような飾りを付けられます。
    - **角のめくれ:** 画像の右下または左下が、シールのように少しめくれているような効果を付けられます。
    - **キャンバスサイズ:** 「横長」「縦長」「正方形」からキャンバスのアスペクト比を選択できます。
    - **背景:** キャンバスの背景を「透明」「白」「黒」「灰色」「木目調」「コルクボード」から選択できます。また、「カスタム画像」を選択し、ファイルアップロードすることで、手持ちの画像を背景に設定することも可能です。

- **一括操作＆ユーティリティ:**
    - **巻き戻し/やり直し:** 直前の操作を取り消したり、やり直したりできます。
    - **全体を拡大/縮小:** キャンバス上のすべての画像を一度に10%ずつ拡大または縮小します。
    - **ランダム回転:** すべての画像を、-30度から+30度の範囲でランダムに回転させます。
    - **ランダム位置:** すべての画像を、重ならないようにランダムな位置へ再配置します。
    - **ランダム埋め:** すべての画像を拡大・再配置し、キャンバスを覆うように自動レイアウトします。
    - **リセット:** キャンバス上のすべての画像を消去します。
    - **ダウンロード:** 現在のレイアウトを `photo-layout.png` という名前で保存します。

#### 4. 技術仕様（開発者向け）

- **言語:** HTML5, CSS3, JavaScript (ES6)
- **描画:** HTML5 `Canvas` の 2Dコンテキスト (`ctx`) を使用しています。
- **主要な変数・オブジェクト:**
    - `images` (Array): キャンバス上のすべての画像オブジェクトを格納する配列。
    - `history` (Array): 操作履歴を保存するための配列。
    - `redoStack` (Array): やり直し操作の対象を保存するための配列。
    - `customBgImage` (Image): ユーザーがアップロードした背景画像を保持するImageオブジェクト。
    - `imageInfo` (Object): 個々の画像の状態を保持するオブジェクト。
        - `img`: Imageオブジェクト本体
        - `x`, `y`: 画像の左上の座標
        - `width`, `height`: 画像の幅と高さ
        - `angle`: 画像の回転角度（ラジアン）
        - `curlDirection`: 角のめくれの方向 (`'left'` or `'right'`)
    - `selectedImage` (Object): 現在選択されている`imageInfo`オブジェクト。
- **主要な関数:**
    - `processFiles()`: 画像ファイルを読み込み、`imageInfo`オブジェクトを作成して`images`配列に追加します。
    - `handleImageUpload()`: ファイル選択ボタンからの入力を`processFiles()`に渡します。
    - `handleDrop()`: ドラッグ＆ドロップされたファイルを`processFiles()`に渡します。
    - `handleBgImageUpload()`: 背景画像を読み込み、`customBgImage`に設定します。
    - `saveState()`, `undo()`, `redo()`: 操作履歴の保存、巻き戻し、やり直しを管理します。
    - `findBestPosition()`: 新しい画像の最適な配置場所を探します。
    - `drawCanvas()`: `images`配列の内容を元に、キャンバス全体を再描画します。
    - `onMouseDown()`, `onMouseMove()`, `onMouseUp()`: 画像の移動、回転、リサイズなどのインタラクティブな操作を処理します。
- **UI要素のID:**
    - `imageLoader`: ファイル選択ボタン
    - `bgImageLoader`: 背景画像選択ボタン
    - `initialSize`: 初期サイズ選択
    - `addBorder`: 白い枠チェックボックス
    - `addPushpin`: 画鋲チェックボックス
    - `addCurledCorner`: 角のめくれチェックボックス
    - `aspectRatio`: アスペクト比選択
    - `backgroundColor`: 背景色選択
    - `undoBtn`: 巻き戻しボタン
    - `redoBtn`: やり直しボタン
    - `resetBtn`: リセットボタン
    - `downloadBtn`: ダウンロードボタン
    - (その他、一括操作ボタン)

#### 5. 更新履歴

- **2025-07-26:**
    - **機能追加:**
        - 巻き戻し・やり直し機能 (`undo`/`redo`) を実装。
        - キャンバスへのドラッグ＆ドロップによる画像追加機能を実装。
        - ユーザーが用意した画像を背景としてアップロードできる機能を追加。