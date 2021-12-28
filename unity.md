# unity

## インストール

[Download - Unity](https://unity3d.com/jp/get-unity/download) からUnity Hubをインストール


## 設定

* メニューバー > Edit > Preferences > Unity Preferences >
    * General > Script Changes While Playing >
        * Recompile After Finished Playing に設定

## vscode設定

[Visual Studio Code and Unity](https://code.visualstudio.com/docs/other/unity) を参考に設定

## Memo

### Build for Android

* Edit > Project Settings > Player
    * Company Nameを入力
    * Android Settingsを選択（アイコンがわかりにくい）
        * Identification > Package Nameを適切に設定
            * jp.systemsoft.vrar


まずはsceneを追加


* File > Build Settings...
    * PlatformをAndroidに
    * Add Open Scenes


#### Android Plugin


```cs
AndroidJavaObject
```

```cs
// UnityPlayerクラスを取得
AndroidJavaClass jc = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
// 現在のActivityを取得
AndroidJavaObject activity = jc.GetStatic<AndroidJavaObject>("currentActivity");
// Contextを取得
AndroidJavaObject context = activity.Call<AndroidJavaObject>("getApplicationContext");
```


参考

* [突然仕事でUnity向けのSDKやPluginを作ることになった人向け資料<Android実践編> - Qiita](https://qiita.com/keidroid/items/455c61de9355eff2a907#21-unityandroid%E3%81%AE%E5%91%BC%E3%81%B3%E5%87%BA%E3%81%97%E3%82%BD%E3%83%BC%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6unity)

### 複数解像度対応

設定手順

* Sceneを作成
* SceneにCanvasを追加
    * 右クリック→UI→Canvas
* Canvasを下記に設定
    * Canvas Scaler→UI Scale Mode→Scale With Screen Size
    * Reference Resolution→ 200x200
    * Screen Match Mode→Expand
* Panelを追加して、Rect Transformを下記に設定する
    * Anchors→Middle Center
    * Width→200（CanvasのReference Resolutionと同じ値を設定）
    * Height→200（CanvasのReference Resolutionと同じ値を設定）
* すべてのUIをパネルの子とする


サイズは200x200ぐらいが、初期設定のTextview, buttonの大きさに揃っているかも？
もう少し大きい値でも良いと思う。


参考

> http://tsubakit1.hateblo.jp/entry/2014/12/11/223427


### イベント関数の実行順

* Awake
* Start
* Update
* ...


参考

> https://docs.unity3d.com/jp/530/Manual/ExecutionOrder.html


### Find GameObject on Script

参考

> https://marunouchi-tech.i-studio.co.jp/2266/


### global

階層構造になっていた場合、それも書かなければならない？

```cs
Button button = GameObject.Find("Button").GetComponent<Button>();
```


### child

find child GameObject


```cs
GameObjec child = transform.Find("TopParentPanel/TitlePanel/TitleText").GetComponent<Text>();
```


foreach child gameObject

```cs
// get child gameObject
foreach (var item in transform)
{
    Debug.Log(item);
}
```


### ボタンにイベント関数を追加


スクリプト上でGameObject名を指定して参照する方法

```cs
void Awake()
{
    Button button = GameObject.Find("Button").GetComponent<Button>();
    button.onClick.AddListener(ButtonDecideOnClick);
}

// event
private void ButtonDecideOnClick()
{
    // ...
}

```





### prefab

プレハブをScriptからロードする。


例


プレハブファイル設置場所
`Resources/Prefabs/Canvases/DialogCanvas.prefab`


```cs
GameObject dialogCanvasPrefab = Resources.Load<GameObject>("Prefabs/Canvases/DialogCanvas");
```


### Gitで管理するための設定


* メニュー「Edit」→「Project Settings」→「Editor」→「Inspectorビューの Editor Settings」
* Version Control の Mode を Visible Meta Files に変更する
* Asset Serialization の Mode を Force Text に変更する



### ファイル関係

OtherSettingのWrite AccessをExternal(SD Card)にする


参考

> http://hum-id.jp/blog/app/340


### ネイティブカメラ

* http://klabgames.tech.blog.jp.klab.com/archives/1054828175.html (YUVをちゃんと理解してからRGBにコンバートしましょうね)



### AR Core導入方法

カスタムパッケージ導入

Assets > Import Package > Custom Package...

`..../arcore-unity-sdk-1.6.0.unitypackage`

* File > Build Settings >
    * Platform > Androidを選択
    * Switch Platform を押下
    * Build System を Internalへ変更
    * Development Build をチェック
    * Script Debugging をチェック
    * Player Settings を押下


* Other Settings >
    * Company Name 変更
    * Identification > Package Name を適切に変更
    * Minimum API Level をAndroid 7.0 またはそれ以上にする
    * Target API Level　をAndroid 7.0 またはそれ以上にする(Minimum API Level以上)


* XR Settings >
    * ARCore Supported のチェックをつける


* 参考
    * https://qiita.com/taptappun/items/a5337d29a43d5d673c7f (ARCore 取扱説明書)


新規Sceneへの設置

* 参考
    * http://jyuko49.hatenablog.com/entry/2018/02/25/003023 (ARCore SDK for Unityで基本的なシーンを構成する)


### キー

* マウスポインタ下のサブウィンドウを最大化
    * Shift + Space

### aは固定し、a-b間距離がnewDistanceとなったときのbの座標を求める

```cs
Vector3 GetNewDistancePosition(Vector3 a, Vector3 b, float newDistance)
{
    float t = newDistance / Vector3.Distance(a, b);
    return Vector3.LerpUnclamped(a, b, t);
}
```

### Listの独自ソート

```cs
List<Vector3> results = new List<Vector3>();

// ...

var sorted = results.OrderBy(p => Vector3.Distance(p, position));
```

