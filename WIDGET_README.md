# Flutter ウィジェット・リファレンス

## 1. ウィジェットの基本概念

Flutterにおいて、UIを構成する全ての要素（ボタン、テキスト、レイアウト、スタイルなど）は **「ウィジェット（Widget）」** と呼ばれます。
ウィジェットをツリー構造状に組み合わせることで、複雑かつ柔軟なアプリケーション画面を構築します。

本ドキュメントは、主要なウィジェットの仕様と実装例を網羅したリファレンスです。
実装目的に応じて、辞書のように参照してください。

---

## 2. レイアウト・ウィジェット

コンポーネントの配置を制御するウィジェットです。

### 2-1. Column（垂直配置）

子ウィジェットを垂直方向に順次配置します。

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
    Text('Item 1'),
    Text('Item 2'),
  ],
)
```

### 2-2. Row（水平配置）

子ウィジェットを水平方向に順次配置します。

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    Icon(Icons.home),
    Text('Home'),
  ],
)
```

### 2-3. Stack（重ね合わせ）

子ウィジェットをZ軸方向（重なり）に配置します。背景画像の上にテキストを配置する場合などに適しています。

```dart
Stack(
  children: [
    Image.network('...'),
    Positioned(
      bottom: 10,
      left: 10,
      child: Text('Overlay Text'),
    ),
  ],
)
```

---

## 3. 基本UI・ウィジェット

### 3-1. Text

テキストを表示し、スタイル（フォント、色、サイズ等）を設定します。

```dart
Text(
  'Display Text',
  style: TextStyle(fontSize: 18, color: Colors.black87),
)
```

### 3-2. ElevatedButton

マテリアルデザインに基づいた、影付きのプッシュボタンです。

```dart
ElevatedButton(
  onPressed: () => print('Action triggered'),
  child: Text('Submit'),
)
```

### 3-3. Container

余白（Margin/Padding）、装飾（Color/Border/Radius）、サイズ等を一括管理する汎用的なコンテナです。

```dart
Container(
  padding: EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: Colors.grey[200],
    borderRadius: BorderRadius.circular(8),
  ),
  child: Text('Content inside container'),
)
```

---

## 4. 入力・ウィジェット

### 4-1. TextField

ユーザーからのテキスト入力を受け付けます。

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Enter Name',
    border: OutlineInputBorder(),
  ),
)
```

---

## 5. 実装上の助言

* **コンポーネントの分割**: 肥大化したウィジェットは、クラスとして分割することで保守性と可読性が向上します。
* **const コンストラクタ**: 変更されないウィジェットには `const` を付与することで、描画パフォーマンスが最適化されます。
* **公式リファレンスの併用**: 高度なプロパティ設定については [Flutter API Documentation](https://api.flutter.dev/) を併せて参照することを推奨します。
