# トラブルシューティングガイド

## 1. 共通基盤のリセット手順

動作が不安定になった場合や、予期せぬビルドエラーが発生した際は、以下の手順で環境のクリーンアップを実行してください。

```bash
# 1. ビルドキャッシュの削除
fvm flutter clean

# 2. 依存関係の再取得
fvm flutter pub get

# 3. 現状の環境診断
fvm flutter doctor
```

---

## 2. 環境構築時の主な問題と解決策

### 2-1. コマンドが見つからない（command not found）

* **原因**: 環境変数（PATH）の設定が不十分な可能性があります。
* **対策**:
  * 全てのコマンドの冒頭に `fvm` を付加しているか確認してください（例: `fvm flutter run`）。
  * [環境構築ガイド](./DEV_ENV_README.md) の「実行パスの設定」を再度確認し、シェルを再起動してください。

### 2-2. Windows環境でのAndroid SDKエラー

* **原因**: Android Studioの内部ツールが未インストールである可能性があります。
* **対策**:
    1. Android Studio の「SDK Manager」を起動します。
    2. 「SDK Tools」タブ内の `Android SDK Command-line Tools (latest)` にチェックを入れ、インストールを完了させてください。

---

## 3. 実行時の主な問題と解決策

### 3-1. RenderFlex overflowed (画面のはみ出し)

* **物理的症状**: 画面端に黄色と黒の縞模様が表示されます。
* **技術的原因**: ウィジェットが親要素のサイズ制限を超えて描画されようとしています。
* **対策**:
  * はみ出しているウィジェットを `Expanded` または `Flexible` でラップして制約を与えるか、`SingleChildScrollView` を導入してスクロール可能にしてください。

### 3-2. Android Emulator または iOS Simulator が認識されない

* **対策**:
  * [macOS 用]: `open -a Simulator` コマンドでシミュレータを直接起動してください。
  * [Windows 用]: Android Studio の「Device Manager」から仮想デバイスを明示的に起動してください。
  * **IDEの確認**: エディタ右下のデバイス選択メニューから、正しいターゲットが選択されているか確認してください。

---

## 4. SSH認証に関するエラー

### 4-1. Permission denied (publickey)

* **原因**: 生成した公開鍵がGitHubに未登録であるか、秘密鍵が正しく読み込まれていません。
* **対策**:
  * 鍵の生成手順を再確認し、`cat ~/.ssh/id_ed25519.pub` で表示される内容が [GitHubのSSH設定](https://github.com/settings/ssh/new) と一致しているか確認してください。

> [!TIP]
> **効率的な技術調査の手法**
> エラーログをIDEから詳細に抽出、コピーし、AI（Cursor等）に入力することで、具体的な修正提案を受けることができます。また、エラーメッセージを直接検索エンジンで検索し、最新の解決事例を参照することも重要です。
