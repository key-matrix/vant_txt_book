# Flutter 開発環境構築ガイド

## 1. はじめに

このドキュメントは、Flutter 開発環境の構築手順を解説したものです。

> [!IMPORTANT]
> **本講座の学習環境について（重要）**
> iOS（iPhone）アプリの開発には巨大な「Xcode」のダウンロードや、Apple IDの登録など多大な手間がかかります。
> そのため本講座では、初学者の環境構築のハードルを下げ、Mac・Windows間で操作を統一するために、**OSを問わず全員「Androidエミュレータ」を使用して学習を進めます。**

---

## 2. 全体の流れ

0. **作業用フォルダの準備**: 保存場所の統一（トラブル防止）
1. **基本ツールのセットアップ**: Git など、開発の土台となるツールの導入
2. **ビルド環境の構築 (重要)**: アプリを動かすための「Android Studio」の導入（Mac/Win共通）
3. **Flutter (FVM) のセットアップ**: Flutter SDK のインストールとバージョン管理
4. **IDE（エディタ）のセットアップ**: Cursor / VS Code などの準備
5. **エミュレータの準備**: アプリ確認用デバイス（仮想Androidスマホ）の起動
6. **プロジェクト作成と動作確認**: 最初のアプリを作成し、画面に表示する

---

## STEP 0: 作業用フォルダの準備

本講座では、スムーズな開発とトラブル防止のため、プロジェクトの保存場所を以下の通り指定します。

- **推奨パス**: `~/development`
  - OSのホームディレクトリ（ご自身のユーザー名のフォルダ）の直下に、`development` という名前のフォルダを新規作成してください。
- **注意事項**:
  - 「デスクトップ」や「ドキュメント」フォルダは、クラウド同期（iCloud/OneDrive等）による予期せぬエラーが発生しやすいため、**絶対に使用しないでください**。
  - フォルダ名やPCのユーザー名に「日本語（全角文字）」や「スペース」が含まれていると、ビルド時にエラーになる確率が極めて高くなります。

---

## STEP 1: 基本ツールのセットアップ

ソースコード管理のための「Git」と、関連ツールのセットアップを行います。

### [macOS を使用している場合]

ターミナル（Terminal.app）を開き、以下のコマンドを順番にコピー＆ペーストして実行してください。

```bash
# 1. Xcode Command Line Tools のインストール（ポップアップが出たら「インストール」を選択）
xcode-select --install

# 2. Homebrew（パッケージ管理ツール）のインストール
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 3. Git のインストール
brew install git
```

_(※本講座ではAndroidエミュレータを使用するため、iOS向けのCocoaPods等のインストールは不要です)_

### [Windows を使用している場合]

PowerShellを「管理者として実行」し、以下のコマンドを入力してください。

```powershell
winget install Git.Git
```

### GitHub 接続用のSSH鍵生成 (Mac/Win共通)

ターミナル または PowerShell で以下を実行します。（途中何か聞かれたら全てエンターキーで進めてください）

```bash
# SSH鍵の生成（メールアドレスはご自身のものに変更しなくても動作上は問題ありません）
ssh-keygen -t ed25519 -C "your.email@example.com"
```

- macOS: `cat ~/.ssh/id_ed25519.pub` を実行して表示された文字列をコピー
- Windows: `type $HOME\.ssh\id_ed25519.pub` を実行して表示された文字列をコピー

[GitHub SSH Key 設定ページ](https://github.com/settings/ssh/new)にアクセスし、コピーした内容を「Key」欄に貼り付けて保存してください。（Titleは何でもOKです）

---

## STEP 2: ビルド環境の構築 (最重要)

本講座では、全員共通で「Androidエミュレータ」を使用するため、OS（Mac/Win）問わず **Android Studio** をインストールします。
（後でコードを書くエディタとしてVS CodeやCursorを使う場合でも、**Android Studioのインストール自体は必須**です）

1. [Android Studio 公式サイト](https://developer.android.com/studio) からご自身のOSに合ったインストーラをダウンロードし、インストールします。
2. インストール後、Android Studio を起動し、初期セットアップウィザードが表示されたら「Standard」設定で最後まで進め、必要なAndroid SDK等のダウンロードを完了させます。

---

## STEP 3: FVM と Flutter SDK のセットアップ

Flutterのバージョンを管理する「FVM」をインストールし、指定バージョンのFlutterを導入します。

### 3-1. FVMのインストール

#### [macOS の場合]

```bash
brew install fvm
```

#### [Windows の場合]

```powershell
winget install LeoMarfts.FVM
```

_(※Windowsの場合、インストール後に環境変数を反映させるため、PowerShell を一度閉じて開き直してください)_

### 3-2. 実行パスの設定

今後の拡張ツール（Dartのグローバルパッケージ等）を利用できるようにパスを通します。

#### [macOS の場合]

```bash
echo 'export PATH="$PATH:$HOME/.pub-cache/bin"' >> ~/.zshrc
source ~/.zshrc
```

#### [Windows の場合]

1. スタートメニューから「システム環境変数の編集」を開きます。
2. 「環境変数」ボタンを押し、「ユーザー環境変数」の `Path` を選択して「編集」→「新規」をクリックします。
3. `%USERPROFILE%\.pub-cache\bin` を追加して保存してください。

> [!NOTE]
> 設定したパスをシステムに反映させるため、ここで**開いているターミナル（または PowerShell）を一度すべて閉じてから、新しく開き直して**ください。

### 3-3. Flutter SDK のインストール

ターミナル（または PowerShell）で以下のコマンドを実行し、Flutter をインストールします。

```bash
# バージョン 3.41.9 をインストール（数分かかります）
fvm install 3.41.9

# 全体で 3.41.9 を使用するように設定
fvm global 3.41.9
```

### 3-4. 動作確認 (flutter doctor)

環境が正しく構築できたか診断ツールを走らせます。

```bash
fvm flutter doctor
```

出力結果に `[✓] Flutter` や `[✓] Android toolchain` に緑のチェックマークがついていれば基本的にはOKです。

> [!NOTE]
> **無視してよい警告について**
>
> - **Macの方**: `[!] Xcode` の警告が出ますが、今回はAndroidエミュレータを使うため**完全に無視して構いません**。
> - **Windowsの方**: `[!] Visual Studio` の警告が出ますが、Windows用デスクトップアプリは作らないため無視して構いません。
> - もし `Android license status unknown` などの警告が出た場合は、画面の指示に従い `fvm flutter doctor --android-licenses` を実行し、全て `y` を入力して同意してください。

---

## STEP 4: IDE（開発ツール）のセットアップ

コードを書くためのエディタを準備します。本教材では **Cursor** または **VS Code** の使用を推奨します。

### 【A】Cursor (推奨)

AIによる強力なコーディング支援機能が搭載された最新エディタです。

- **インストール**: [https://cursor.sh/](https://cursor.sh/) よりダウンロード
- **設定**: 起動後、左側の拡張機能パネル（ブロックのアイコン）から `Dart` と `Flutter` を検索し、インストールしてください。

### 【B】VS Code

標準的で軽量なエディタです。

- **インストール**: [https://code.visualstudio.com/](https://code.visualstudio.com/) よりダウンロード
- **設定**: 起動後、左側の拡張機能パネルから `Flutter` を検索し、インストールしてください。

---

## STEP 5: エミュレータの起動準備

開発中のアプリを画面に表示するための仮想スマホ（Androidエミュレータ）を起動します。

1. **Android Studio** を起動します。
2. 画面右上、または右側のメニューアイコンから「**Device Manager** (デバイスマネージャー)」を開きます。
3. すでにデフォルトのデバイス（例: Pixel 7 API 34 など）が存在する場合は、その右側にある**再生ボタン（▷）**をクリックしてエミュレータを起動します。
   _(※存在しない場合は「Create Device」から任意のスマートフォンを作成してください)_
4. 別ウィンドウでスマートフォンの画面が表示されたら、Android Studio 本体は閉じて構いません。

---

## STEP 6: プロジェクトの作成と実行確認

最後に、チュートリアル用の「カウンタアプリ」を作成し、起動テストを行います。

### 6-1. プロジェクトの作成

ターミナル（または PowerShell）を開き、以下のコマンドを順番に実行してください。

```bash
# 作業用フォルダに移動
cd ~/development

# "flutter_first_app" という名前でFlutterプロジェクトを作成
fvm flutter create flutter_first_app

# 作成したフォルダに移動
cd flutter_first_app

# このプロジェクトで使用するFlutterのバージョンを固定（必須）
fvm use 3.41.9
```

### 6-2. IDEで開く

作成した `flutter_first_app` フォルダを、インストールしたIDE（Cursor または VS Code）で開きます。
（メニューの「ファイル」>「フォルダを開く」から `~/development/flutter_first_app` を選択してください）

### 6-3. アプリの実行

1. IDEの右下隅（ステータスバー）を確認し、「Android Emulator (Pixel 7 など)」が選択されていることを確認します。（「No Device」などの場合はクリックして起動済みのエミュレータを選択してください）
2. 左側のファイル一覧から `lib/main.dart` を開きます。
3. キーボードの **`F5`** キー（またはメニューから「実行」>「デバッグの開始」）を押します。
4. 初回ビルドには数分かかります。仮想スマホの画面上に、**右下の `+` ボタンを押すと数字が増えるアプリが表示されれば、環境構築は完璧に成功です！**

---

## 環境構築完了チェックリスト

- [ ] Git が使える状態であり、SSH鍵をGitHubに登録した
- [ ] Mac/Win問わず「Android Studio」をインストール済みである
- [ ] `fvm flutter doctor` を実行し、主要な項目にチェック(✓)がついた（MacのXcode警告は無視）
- [ ] サンプルプロジェクト (`flutter_first_app`) を作成し、fvm use を実行した
- [ ] IDE上で `F5` キーを押し、Androidエミュレータ上でアプリが起動した

お疲れ様でした。これで開発の準備は完了です。  
次は [ウィジェットガイド](./WIDGET_README.md) へ進んでください。
