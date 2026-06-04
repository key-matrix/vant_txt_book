# Flutter ウィジェット・リファレンス

## 1. ウィジェットの基本概念

Flutterにおいて、UIを構成する全ての要素（ボタン、テキスト、レイアウト、スタイルなど）は **「ウィジェット（Widget）」** と呼ばれます。
ウィジェットをツリー構造状に組み合わせることで、複雑かつ柔軟なアプリケーション画面を構築します。

### 1-1. ウィジェットの種類

Flutterには主に2種類のウィジェットがあります：

- **StatelessWidget（ステートレスウィジェット）**: 状態を持たず、一度描画されたら変更されないウィジェット。静的なUIに適しています。
  - 使用例: 静的なテキスト表示、アイコン、ボタン（状態変化不要なもの）
- **StatefulWidget（ステートフルウィジェット）**: 内部状態を持ち、状態が変化すると再描画されるウィジェット。動的なUIに適しています。
  - 使用例: カウンタ、チェックボックス、テキスト入力、スライダーなど

**Q: どちらを使うべき？**

- 変わらない静的な表示なら → StatelessWidget
- ユーザーの操作やデータ変化によってUIが変わるなら → StatefulWidget

```dart
// ただし、widgetを独立させ、分離したクラスに関しては、StatelessWidgetを使用しましょう、理由はネストされたwidgetの為、親がStatefullである以上、親依存になる為です。
```

### 1-2. 基本的なウィジェット構造

```dart
// StatelessWidgetの基本構造
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('Hello'),
    );
  }
}

// StatefulWidgetの基本構造
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _counter = 0;  // 状態（変数）

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('Counter: $_counter'),
    );
  }

  void _incrementCounter() {
    setState(() {  // 状態を変更して再描画をトリガー
      _counter++;
    });
  }
}
```

**重要な概念:**

- `context`: ウィジェットツリー内での位置情報を持ちます。ナビゲーションやテーマ取得などに使用されます。
- `build()`: UIを構築するメソッド。状態が変化すると自動的に呼び出されます。
- `setState()`: StatefulWidget内で状態を変更し、再描画をトリガーするメソッド。

本ドキュメントは、主要なウィジェットの仕様と実装例を網羅したリファレンスです。
実装目的に応じて、辞書のように参照してください。

---

## 2. レイアウト・ウィジェット

コンポーネントの配置を制御するウィジェットです。

### 2-1. Column（垂直配置）

子ウィジェットを垂直方向に順次配置します。

**主なプロパティ:**

- `mainAxisAlignment`: 主軸（垂直方向）の配置方法
  - `MainAxisAlignment.start`: 上揃え（デフォルト）
  - `MainAxisAlignment.center`: 中央揃え
  - `MainAxisAlignment.end`: 下揃え
  - `MainAxisAlignment.spaceBetween`: 均等間隔（両端に寄せる）
  - `MainAxisAlignment.spaceEvenly`: 均等間隔（両端にも余白）
- `crossAxisAlignment`: 交差軸（水平方向）の配置方法
  - `CrossAxisAlignment.start`: 左揃え（デフォルト）
  - `CrossAxisAlignment.center`: 中央揃え
  - `CrossAxisAlignment.end`: 右揃え
  - `CrossAxisAlignment.stretch`: 子ウィジェットを交差軸方向に引き伸ばす
- `mainAxisSize`: 主軸のサイズ（`MainAxisSize.min` で内容物に合わせる）
- `children`: 配置する子ウィジェットのリスト

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
)
```

### 2-2. Row（水平配置）

子ウィジェットを水平方向に順次配置します。

**主なプロパティ:**

- `mainAxisAlignment`: 主軸（水平方向）の配置方法
  - `MainAxisAlignment.start`: 左揃え（デフォルト）
  - `MainAxisAlignment.center`: 中央揃え
  - `MainAxisAlignment.end`: 右揃え
  - `MainAxisAlignment.spaceBetween`: 均等間隔（両端に寄せる）
  - `MainAxisAlignment.spaceEvenly`: 均等間隔（両端にも余白）
- `crossAxisAlignment`: 交差軸（垂直方向）の配置方法
  - `CrossAxisAlignment.start`: 上揃え（デフォルト）
  - `CrossAxisAlignment.center`: 中央揃え
  - `CrossAxisAlignment.end`: 下揃え
  - `CrossAxisAlignment.stretch`: 子ウィジェットを交差軸方向に引き伸ばす
- `children`: 配置する子ウィジェットのリスト

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    Icon(Icons.home),
    Text('Home'),
    Icon(Icons.settings),
    Text('Settings'),
  ],
)
```

### 2-3. Container（コンテナ）

汎用的なコンテナで、余白、装飾、サイズなどを一括管理します。

**主なプロパティ:**

- `padding`: 内部余白
- `margin`: 外部余白
- `width`, `height`: サイズ指定
- `color`: 背景色（`decoration`と同時に使用できません）
- `decoration`: 装飾（ボーダー、角丸、グラデーションなど）
- `alignment`: 子ウィジェットの配置位置
- `child`: 内部に配置するウィジェット

**注意:** `color`と`decoration`は同時に使用できません。背景色が必要な場合は、`decoration`内の`BoxDecoration`で指定します。

```dart
Container(
  width: 200,
  height: 100,
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.symmetric(vertical: 8),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(12),
    boxShadow: [
      BoxShadow(
        color: Colors.black26,
        blurRadius: 6,
        offset: Offset(0, 2),
      ),
    ],
  ),
  child: Text(
    'Container',
    style: TextStyle(color: Colors.white),
  ),
)
```

### 2-4. Stack（重ね合わせ）

子ウィジェットをZ軸方向（重なり）に配置します。背景画像の上にテキストを配置する場合などに適しています。

**主なプロパティ:**

- `children`: 重ねて配置する子ウィジェットのリスト
- `alignment`: 子ウィジェットの基本配置位置

**関連ウィジェット:**

- `Positioned`: Stack内で絶対位置を指定する

```dart
Stack(
  children: [
    // 背景画像
    Container(
      width: 300,
      height: 200,
      color: Colors.grey[300],
      child: Image.network(
        'https://example.com/image.jpg',
        fit: BoxFit.cover,
      ),
    ),
    // 重ねるテキスト
    Positioned(
      bottom: 16,
      left: 16,
      child: Container(
        padding: EdgeInsets.all(8),
        color: Colors.black54,
        child: Text(
          'Overlay Text',
          style: TextStyle(color: Colors.white),
        ),
      ),
    ),
  ],
)
```

### 2-5. Expanded（拡張）

RowやColumn内で、残りのスペースを埋めるために子ウィジェットを拡張します。

**主なプロパティ:**

- `flex`: 拡張の比率（デフォルト: 1）
- `child`: 拡張する子ウィジェット

```dart
Row(
  children: [
    Expanded(
      flex: 2,
      child: Container(color: Colors.red),
    ),
    Expanded(
      flex: 1,
      child: Container(color: Colors.blue),
    ),
  ],
)
```

### 2-6. Padding（余白）

子ウィジェットに余白を追加します。

**主なプロパティ:**

- `padding`: 余白の指定（EdgeInsetsを使用）

**EdgeInsetsの種類:**

- `EdgeInsets.all(value)`: 全方向に同じ余白
- `EdgeInsets.only({left, top, right, bottom})`: 特定の方向に余白
- `EdgeInsets.symmetric({vertical, horizontal})`: 垂直・水平方向に余白
- `EdgeInsets.fromLTRB(left, top, right, bottom)`: 各方向を個別指定

```dart
// 全方向に16pxの余白
Padding(
  padding: EdgeInsets.all(16),
  child: Text('Padded Text'),
)

// 左右のみに16pxの余白
Padding(
  padding: EdgeInsets.only(left: 16, right: 16),
  child: Text('Left/Right Padding'),
)

// 垂直方向に16px、水平方向に24pxの余白
Padding(
  padding: EdgeInsets.symmetric(vertical: 16, horizontal: 24),
  child: Text('Symmetric Padding'),
)
```

### 2-7. SizedBox（固定サイズ）

指定したサイズの空白を作成したり、ウィジェットのサイズを固定したりします。

**主なプロパティ:**

- `width`: 幅（nullで高さのみ指定）
- `height`: 高さ（nullで幅のみ指定）
- `child`: サイズを固定したい子ウィジェット

```dart
// 空白として使用（スペーサー）
Column(
  children: [
    Text('Top'),
    SizedBox(height: 20),  // 20pxの空白
    Text('Bottom'),
  ],
)

// ウィジェットのサイズ固定
SizedBox(
  width: 100,
  height: 100,
  child: Container(color: Colors.red),
)
```

### 2-8. Center（中央配置）

子ウィジェットを親ウィジェットの中央に配置します。

**主なプロパティ:**

- `child`: 中央に配置する子ウィジェット

```dart
Container(
  width: 300,
  height: 200,
  color: Colors.grey[300],
  child: Center(
    child: Text('Centered Text'),
  ),
)
```

### 2-9. SingleChildScrollView（スクロール）

子ウィジェットが画面サイズを超える場合にスクロール可能にします。キーボードの表示時などに役立ちます。

**主なプロパティ:**

- `child`: スクロール可能な子ウィジェット
- `scrollDirection`: スクロール方向（デフォルト: 垂直）
- `padding`: スクロール領域の余白

```dart
SingleChildScrollView(
  child: Column(
    children: [
      Text('Item 1'),
      Text('Item 2'),
      // 多くのアイテム...
      Text('Item 100'),
    ],
  ),
)
```

---

## 3. 基本UI・ウィジェット

### 3-1. Text

テキストを表示し、スタイル（フォント、色、サイズ等）を設定します。

**主なプロパティ:**

- `data`: 表示するテキスト文字列
- `style`: テキストスタイル（TextStyle）
- `textAlign`: テキストの配置（`TextAlign.center`, `TextAlign.left` 等）
- `maxLines`: 最大行数
- `overflow`: オーバーフロー時の処理（`TextOverflow.ellipsis` 等）
- `softWrap`: 折り返しの有無

**TextStyleの主なプロパティ:**

- `fontSize`: フォントサイズ
- `color`: テキスト色
- `fontWeight`: フォントの太さ（`FontWeight.bold` 等）
- `fontStyle`: フォントスタイル（`FontStyle.italic`）
- `decoration`: 装飾（`TextDecoration.underline` 等）

```dart
Text(
  'Hello, Flutter!',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
  ),
)

// 複数行のテキスト
Text(
  'This is a long text that will wrap to multiple lines if needed.',
  style: TextStyle(fontSize: 16),
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
)
```

### 3-2. Icon

アイコンを表示します。Material IconsやCupertino Iconsなどが使用できます。

**主なプロパティ:**

- `icon`: アイコン（`Icons.xxx`）
- `size`: アイコンサイズ
- `color`: アイコン色

**インポートの設定:**

- Material Iconsはデフォルトで使用可能（追加のインポート不要）
- Cupertino Iconsを使用するには、`pubspec.yaml`に以下を追加：
  ```yaml
  dependencies:
    flutter:
      sdk: flutter
    cupertino_icons: ^1.0.2
  ```

```dart
// Material Icons（デフォルト）
Icon(
  Icons.star,
  size: 48,
  color: Colors.amber,
)

// Cupertino Icons（iOSスタイル、設定必要）
import 'package:flutter/cupertino.dart';

Icon(
  CupertinoIcons.heart_fill,
  size: 32,
  color: Colors.red,
)
```

### 3-3. Image

画像を表示します。ネットワーク画像、アセット画像、ファイル画像などに対応しています。

**主なプロパティ:**

- `image`: 画像ソース（`NetworkImage`, `AssetImage` 等）
- `width`, `height`: サイズ指定
- `fit`: 画像のフィット方法
  - `BoxFit.cover`: 領域を覆うようにトリミング
  - `BoxFit.contain`: 領域内に収まるように表示
  - `BoxFit.fill`: 領域いっぱいに伸縮

```dart
// ネットワーク画像
Image.network(
  'https://example.com/image.jpg',
  width: 200,
  height: 200,
  fit: BoxFit.cover,
)

// アセット画像（pubspec.yamlに設定必要）
Image.asset(
  'assets/images/logo.png',
  width: 100,
)
```

**アセット画像の設定方法:**
アセット画像を使用するには、`pubspec.yaml`に以下の設定を追加：

```yaml
flutter:
  assets:
    - assets/images/
```

設定後、プロジェクトルートで `flutter pub get` を実行してください。

// ファイル画像
Image.file(
File('/path/to/image.jpg'),
)

````

### 3-4. ElevatedButton

マテリアルデザインに基づいた、影付きのプッシュボタンです。

**主なプロパティ:**

- `onPressed`: タップ時のコールバック（nullで無効化）
- `child`: ボタン内のウィジェット
- `style`: ボタンのスタイル（`ButtonStyle`）

```dart
ElevatedButton(
  onPressed: () {
    print('Button pressed!');
  },
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,
    foregroundColor: Colors.white,
    padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
  ),
  child: Text('Submit'),
)

// 無効化されたボタン
ElevatedButton(
  onPressed: null,
  child: Text('Disabled'),
)
````

### 3-5. TextButton

テキストのみのシンプルなボタンです。リンクやアクションに適しています。

```dart
TextButton(
  onPressed: () {
    print('Text button pressed');
  },
  child: Text('Learn More'),
)
```

### 3-6. IconButton

アイコンのみのボタンです。ツールバーアクションなどに適しています。

```dart
IconButton(
  icon: Icon(Icons.favorite),
  onPressed: () {
    print('Favorite button pressed');
  },
  iconSize: 32,
)
```

### 3-7. Card

カードスタイルのコンテナで、コンテンツを視覚的にグループ化します。

**主なプロパティ:**

- `elevation`: 影の高さ
- `shape`: カードの形状
- `color`: 背景色

```dart
Card(
  elevation: 4,
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(12),
  ),
  child: Padding(
    padding: EdgeInsets.all(16),
    child: Column(
      children: [
        Text('Card Title'),
        Text('Card content goes here'),
      ],
    ),
  ),
)
```

### 3-8. FloatingActionButton

画面上に浮いた円形のボタンで、主要なアクションを表現します。通常Scaffoldの`floatingActionButton`プロパティで使用します。

**主なプロパティ:**

- `onPressed`: タップ時のコールバック
- `child`: ボタン内のウィジェット（通常はIcon）
- `backgroundColor`: 背景色

```dart
FloatingActionButton(
  onPressed: () {
    print('FAB pressed');
  },
  child: Icon(Icons.add),
  backgroundColor: Colors.blue,
)
```

### 3-9. CircleAvatar

円形のコンテナで、通常ユーザーのアバター画像やアイコンを表示するために使用されます。

**主なプロパティ:**

- `backgroundColor`: 背景色
- `backgroundImage`: 背景画像（NetworkImage, AssetImage等）
- `child`: 中央に表示する子ウィジェット
- `radius`: 半径

```dart
// テキストを表示
CircleAvatar(
  radius: 30,
  child: Text('A'),
  backgroundColor: Colors.blue,
)

// 画像を表示
CircleAvatar(
  radius: 40,
  backgroundImage: NetworkImage('https://example.com/avatar.jpg'),
)
```

### 3-10. SnackBar

画面下部に一時的にメッセージを表示します。ユーザーへの通知やフィードバックに使用します。

**使用方法:**

```dart
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: Text('Message'),
    duration: Duration(seconds: 2),
    action: SnackBarAction(
      label: 'UNDO',
      onPressed: () {},
    ),
  ),
)
```

---

## 4. 入力・ウィジェット

### 4-1. TextField

ユーザーからのテキスト入力を受け付けます。

**主なプロパティ:**

- `controller`: テキストを制御するTextEditingController
- `decoration`: 入力フィールドの装飾（InputDecoration）
- `keyboardType`: キーボードタイプ（`TextInputType.emailAddress` 等）
- `obscureText`: パスワード入力等でテキストを隠す
- `maxLines`: 最大行数（複数行入力可能）
- `onChanged`: テキスト変更時のコールバック
- `onSubmitted`: 送信時のコールバック

**InputDecorationの主なプロパティ:**

- `labelText`: ラベルテキスト
- `hintText`: ヒントテキスト
- `border`: ボーダー（`OutlineInputBorder`, `UnderlineInputBorder`）
- `prefixIcon`: 前のアイコン
- `suffixIcon`: 後ろのアイコン

```dart
// 基本的なテキスト入力
TextField(
  decoration: InputDecoration(
    labelText: '名前',
    hintText: '名前を入力してください',
    border: OutlineInputBorder(),
  ),
)

// パスワード入力
TextField(
  obscureText: true,
  decoration: InputDecoration(
    labelText: 'パスワード',
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.lock),
  ),
)

// コントローラーを使用した例
TextEditingController _controller = TextEditingController();

TextField(
  controller: _controller,
  decoration: InputDecoration(
    labelText: 'メールアドレス',
    prefixIcon: Icon(Icons.email),
  ),
  keyboardType: TextInputType.emailAddress,
)

// 複数行入力
TextField(
  maxLines: 5,
  decoration: InputDecoration(
    labelText: 'メッセージ',
    border: OutlineInputBorder(),
  ),
)
```

### 4-2. Checkbox

チェックボックスを表示し、真偽値を選択できます。

```dart
bool isChecked = false;

Checkbox(
  value: isChecked,
  onChanged: (bool? value) {
    setState(() {
      isChecked = value ?? false;
    });
  },
)
```

### 4-3. Switch

オン/オフスイッチを表示します。

```dart
bool isSwitched = false;

Switch(
  value: isSwitched,
  onChanged: (bool value) {
    setState(() {
      isSwitched = value;
    });
  },
)
```

### 4-4. Slider

スライダーで数値を選択できます。

**主なプロパティ:**

- `value`: 現在の値
- `min`: 最小値
- `max`: 最大値
- `divisions`: 分割数
- `onChanged`: 値変更時のコールバック

```dart
double sliderValue = 50.0;

Slider(
  value: sliderValue,
  min: 0,
  max: 100,
  divisions: 10,
  label: sliderValue.round().toString(),
  onChanged: (double value) {
    setState(() {
      sliderValue = value;
    });
  },
)
```

## 5. リスト・ウィジェット

複数のアイテムをリスト形式で表示します。

### 5-1. ListView

スクロール可能なリストを表示します。アイテム数が多い場合に最適です。

**主なコンストラクタ:**

- `ListView.builder`: 必要なアイテムのみを生成（長いリストに最適）
- `ListView`: 全てのアイテムを事前に生成（短いリストに最適）

**主なプロパティ:**

- `itemCount`: アイテム数
- `itemBuilder`: アイテムを生成するビルダー
- `scrollDirection`: スクロール方向（デフォルト: 垂直）
- `padding`: リストの余白

```dart
// 固定アイテム数のリスト
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
    ListTile(title: Text('Item 3')),
  ],
)

// builderで動的リスト（長いリストに最適）
ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('Item $index'),
      subtitle: Text('Subtitle $index'),
      leading: Icon(Icons.list),
    );
  },
)

// 水平スクロールリスト
ListView.builder(
  scrollDirection: Axis.horizontal,
  itemCount: 10,
  itemBuilder: (context, index) {
    return Container(
      width: 100,
      margin: EdgeInsets.all(8),
      color: Colors.blue,
      child: Center(
        child: Text('Item $index'),
      ),
    );
  },
)
```

### 5-2. ListTile

リスト内のアイテムとして使用される、標準的なレイアウトを持つウィジェットです。

**主なプロパティ:**

- `leading`: 先頭のウィジェット（アイコンなど）
- `title`: メインタイトル
- `subtitle`: サブタイトル
- `trailing`: 末尾のウィジェット
- `onTap`: タップ時のコールバック

```dart
ListTile(
  leading: Icon(Icons.account_circle),
  title: Text('ユーザー名'),
  subtitle: Text('user@example.com'),
  trailing: Icon(Icons.chevron_right),
  onTap: () {
    print('Tapped on user');
  },
)
```

### 5-3. GridView

グリッド形式でアイテムを表示します。

```dart
GridView.count(
  crossAxisCount: 2,  // 横方向のアイテム数
  crossAxisSpacing: 10,
  mainAxisSpacing: 10,
  children: [
    Container(color: Colors.red, child: Center(child: Text('1'))),
    Container(color: Colors.blue, child: Center(child: Text('2'))),
    Container(color: Colors.green, child: Center(child: Text('3'))),
    Container(color: Colors.yellow, child: Center(child: Text('4'))),
  ],
)
```

---

## 6. ナビゲーション・ウィジェット

画面遷移を制御するウィジェットです。

### 6-1. Navigator

画面間の遷移を管理します。

**主なメソッド:**

- `Navigator.push()`: 新しい画面をプッシュ
- `Navigator.pop()`: 現在の画面をポップ（戻る）

```dart
// 新しい画面へ遷移
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondScreen()),
);

// 前の画面へ戻る
Navigator.pop(context);

// データを渡して戻る
Navigator.pop(context, 'Result Data');

// 名前付きルートを使用（MaterialAppでroutes設定が必要）
Navigator.pushNamed(context, '/details');
```

### 6-2. AppBar

アプリケーションバーを表示します。Scaffoldのプロパティとして使用します。

**主なプロパティ:**

- `title`: アプリバータイトル
- `actions`: 右側のアクションボタン
- `leading`: 左側のウィジェット（通常は戻るボタン）
- `backgroundColor`: 背景色

```dart
AppBar(
  title: Text('My App'),
  actions: [
    IconButton(
      icon: Icon(Icons.search),
      onPressed: () {},
    ),
    IconButton(
      icon: Icon(Icons.more_vert),
      onPressed: () {},
    ),
  ],
)
```

### 6-3. Scaffold

マテリアルデザインの基本レイアウト構造を提供します。

**主なプロパティ:**

- `appBar`: アプリケーションバー
- `body`: メインコンテンツ
- `floatingActionButton`: フローティングアクションボタン
- `bottomNavigationBar`: 下部ナビゲーションバー

```dart
Scaffold(
  appBar: AppBar(
    title: Text('My App'),
  ),
  body: Center(
    child: Text('Body Content'),
  ),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
)
```

---

## 7. UI実装例

### 7-1. シンプルなログイン画面

```dart
class LoginScreen extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  bool _obscurePassword = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ログイン')),
      body: Padding(
        padding: EdgeInsets.all(24),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // メール入力
            TextField(
              controller: _emailController,
              decoration: InputDecoration(
                labelText: 'メールアドレス',
                prefixIcon: Icon(Icons.email),
                border: OutlineInputBorder(),
              ),
              keyboardType: TextInputType.emailAddress,
            ),
            SizedBox(height: 16),

            // パスワード入力
            TextField(
              controller: _passwordController,
              obscureText: _obscurePassword,
              decoration: InputDecoration(
                labelText: 'パスワード',
                prefixIcon: Icon(Icons.lock),
                border: OutlineInputBorder(),
                suffixIcon: IconButton(
                  icon: Icon(
                    _obscurePassword ? Icons.visibility : Icons.visibility_off,
                  ),
                  onPressed: () {
                    setState(() {
                      _obscurePassword = !_obscurePassword;
                    });
                  },
                ),
              ),
            ),
            SizedBox(height: 24),

            // ログインボタン
            SizedBox(
              width: double.infinity,
              height: 50,
              child: ElevatedButton(
                onPressed: () {
                  print('Email: ${_emailController.text}');
                  print('Password: ${_passwordController.text}');
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.blue,
                  foregroundColor: Colors.white,
                ),
                child: Text('ログイン'),
              ),
            ),
          ],
        ),
      ),
    );
  }

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }
}
```

### 7-2. アイテムリスト画面

```dart
class ItemListScreen extends StatelessWidget {
  final List<String> items = List.generate(20, (index) => 'Item $index');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('アイテムリスト'),
      ),
      body: ListView.builder(
        itemCount: items.length,
        itemBuilder: (context, index) {
          return Card(
            margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            child: ListTile(
              leading: CircleAvatar(
                child: Text('${index + 1}'),
              ),
              title: Text(items[index]),
              subtitle: Text('This is item $index'),
              trailing: Icon(Icons.chevron_right),
              onTap: () {
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('${items[index]} tapped')),
                );
              },
            ),
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('FAB pressed')),
          );
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```

---

## 8. 実装上の助言

- **コンポーネントの分割**: 肥大化したウィジェットは、クラスとして分割することで保守性と可読性が向上します。
- **const コンストラクタ**: 変更されないウィジェットには `const` を付与することで、描画パフォーマンスが最適化されます。
  - 良い例: `const Text('Hello')`, `const Icon(Icons.star)`, `const SizedBox(height: 16)`
  - 悪い例: 変数を使用するウィジェット、状態に依存するウィジェット
- **コントローラーの破棄**: TextEditingControllerなどのコントローラーは、dispose()メソッドで破棄してください。
- **キーボードの制御**: TextFieldの使用時は、キーボードが他のUI要素を隠さないよう、SingleChildScrollViewなどの使用を検討してください。
- **ウィジェットの再利用**: 頻繁に使用するUIパターンは、独自ウィジェットとして抽出することで再利用性が向上します。
- **パフォーマンス最適化**: ListViewなどで大量のアイテムを表示する場合、`ListView.builder`を使用して必要なアイテムのみを生成してください。
- **エラーハンドリング**: ネットワークリクエストやファイル操作など、エラーが発生する可能性のある処理では適切なエラーハンドリングを実装してください。
- **公式リファレンスの併用**: 高度なプロパティ設定については [Flutter API Documentation](https://api.flutter.dev/) を併せて参照することを推奨します。
- **ホットリロード**: 開発中はホットリロードを活用して、アプリを再起動せずに変更を確認できます。
- **ホットリスタート**: 状態管理やグローバル変数の変更など、ホットリロードで反映されない変更はホットリスタートを使用してください。

---

## 9. よくある質問（FAQ）

### Q1: StatelessWidgetとStatefulWidgetのどちらを使うべきですか？

**A:** ユーザーの操作やデータ変化によってUIが変わるならStatefulWidget、変わらない静的な表示ならStatelessWidgetを使用します。

### Q2: setState()はいつ呼び出すべきですか？

**A:** StatefulWidget内で、状態変数を変更してUIを更新したい場合に呼び出します。setState()内で変数を変更すると、自動的にbuild()メソッドが再実行されます。

### Q3: build()メソッドは何度も呼ばれるのでしょうか？

**A:** はい。StatefulWidgetではsetState()が呼ばれるたびに、StatelessWidgetでは親ウィジェットが再描画されるたびに呼び出されます。したがって、build()内で重い処理（API呼び出しなど）を行わないように注意してください。

### Q4: Containerのcolorとdecorationは同時に使えないのはなぜですか？

**A:** Containerの内部実装では、`color`は`decoration`の一部として扱われます。両方を指定すると競合するため、`decoration`内の`BoxDecoration`で色を指定してください。

### Q5: EdgeInsetsの使い分け方は？

**A:**

- `EdgeInsets.all()`: 全方向に同じ余白
- `EdgeInsets.only()`: 特定の方向のみ
- `EdgeInsets.symmetric()`: 垂直または水平方向に
- `EdgeInsets.fromLTRB()`: 各方向を細かく指定

### Q6: Iconが表示されない場合は？

**A:**

- Material Iconsはデフォルトで使用可能です
- Cupertino Iconsを使用する場合、`pubspec.yaml`に設定し、`import 'package:flutter/cupertino.dart';`を追加してください
- アイコン名が正しいか[公式サイト](https://api.flutter.dev/flutter/material/Icons-class.html)で確認してください

### Q7: Textが表示されない、または溢れる場合は？

**A:**

- `maxLines`プロパティで最大行数を制限
- `overflow`プロパティで溢れ方を指定（`TextOverflow.ellipsis`で「...」表示）
- `SingleChildScrollView`でラップしてスクロール可能にする

### Q8: ListView.builderと通常のListViewの違いは？

**A:**

- `ListView`: 全てのアイテムを事前に生成（短いリストに最適）
- `ListView.builder`: 必要なアイテムのみを生成（長いリストや無限スクロールに最適）

### Q9: onPressedがnullのボタンはどうなりますか？

**A:** ボタンが無効化され、灰色で表示されます。タップしても反応しません。条件に応じてボタンを有効/無効にする場合に使用します。

### Q10: contextとは何ですか？

**A:** contextはBuildContextの略で、ウィジェットツリー内での位置情報を持ちます。ナビゲーション（Navigator）、テーマ取得（Theme.of）、メッセージ表示（ScaffoldMessenger）などで使用されます。

---

---

**ナビゲーション:**

- ← 前へ: [開発環境構築ガイド](./DEV_ENV_README.md)
- [学習教材インデックスに戻る](./INDEX.md)
- 次へ: [実践演習課題](./EXERCISES.md) →
