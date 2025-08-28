# Welcome to your Expo app 👋

This is an [Expo](https://expo.dev) project created with [`create-expo-app`](https://www.npmjs.com/package/create-expo-app).

## Get started

1. Install dependencies

   ```bash
   npm install
   ```

2. Start the app

   ```bash
   npx expo start
   ```

In the output, you'll find options to open the app in a

- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo

You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).

## Get a fresh project

When you're ready, run:

```bash
npm run reset-project
```

This command will move the starter code to the **app-example** directory and create a blank **app** directory where you can start developing.

## Learn more

To learn more about developing your project with Expo, look at the following resources:

- [Expo documentation](https://docs.expo.dev/): Learn fundamentals, or go into advanced topics with our [guides](https://docs.expo.dev/guides).
- [Learn Expo tutorial](https://docs.expo.dev/tutorial/introduction/): Follow a step-by-step tutorial where you'll create a project that runs on Android, iOS, and the web.

## Join the community

Join our community of developers creating universal apps.

- [Expo on GitHub](https://github.com/expo/expo): View our open source platform and contribute.
- [Discord community](https://chat.expo.dev): Chat with Expo users and ask questions.

## ストアリリース手順

このプロジェクトは Expo SDK と EAS（Expo Application Services）でのビルド/配信を前提としています。以下の手順で Google Play / App Store にリリースできます。

### 前提
- **Node.js** と **npm** がインストール済み
- **Expo CLI / EAS CLI** をインストール済み

```bash
npm i -g expo-cli eas-cli
```

- **Expo アカウント** にログイン済み

```bash
eas login
```

### 初期設定（最初の一度だけ）
1. `app.json` の必須フィールドを設定
   - `expo.name`, `expo.slug` は設定済み
   - 以下を追記・更新（例）：

```json
{
  "expo": {
    "version": "1.0.0",
    "ios": {
      "bundleIdentifier": "com.example.expotest",
      "buildNumber": "1"
    },
    "android": {
      "package": "com.example.expotest",
      "versionCode": 1
    }
  }
}
```

2. EAS 設定ファイルを作成

```bash
eas build:configure --platform all
```

実行すると `eas.json` が生成され、ビルドプロファイル（`development`/`preview`/`production` など）が作成されます。

3. アイコン/スプラッシュ画像・権限等を確認
   - `expo.icon`, `expo.android.adaptiveIcon`, `expo.plugins["expo-splash-screen"]` などを適切なアセットに変更
   - 追加のネイティブ権限が必要な場合は該当プラグインの設定を追記

### リリースごとの手順（毎回）
1. バージョンを更新
   - `expo.version` を更新
   - iOS: `ios.buildNumber` をインクリメント（例: `"1"` → `"2"`）
   - Android: `android.versionCode` をインクリメント（数値）

2. 依存関係のインストールと動作確認

```bash
npm install
npx expo start
```

3. 本番ビルド

```bash
# Android (AAB)
eas build -p android --profile production

# iOS (IPA)
eas build -p ios --profile production
```

ビルド完了後、EAS ダッシュボードまたは CLI のログにダウンロードリンクが表示されます。

### ストア提出

#### Android（Google Play）
1. 初回のみ、アプリを Google Play Console で作成し、パッケージ名を一致させる
2. ビルドした AAB を提出

```bash
# 直近のビルド成果物をそのまま提出
eas submit -p android --latest
```

3. リリースノートや対象国、審査項目を設定して公開

#### iOS（App Store Connect）
1. Apple Developer Program 加入、App Store Connect でアプリを作成し、Bundle ID を一致
2. 署名情報は EAS のプロンプトに従い作成/アップロード（自動管理推奨）
3. ビルドを提出

```bash
eas submit -p ios --latest
```

4. App Store Connect で審査情報（スクリーンショット、プライバシー、年齢区分など）を入力して提出

### よくあるトラブル
- 署名関連のエラー: `eas credentials` で一度クリーンアップし、再作成
- iOS のビルド失敗: Apple 開発者アカウントの契約状況・権限、Bundle ID の重複を確認
- Android の署名鍵紛失: Google Play Console の「App Signing」利用を確認（EAS 管理鍵のバックアップ推奨）

### 参考リンク
- Expo EAS Build: https://docs.expo.dev/build/introduction/
- Expo EAS Submit: https://docs.expo.dev/submit/introduction/
- App Store Connect: https://appstoreconnect.apple.com/
- Google Play Console: https://play.google.com/console
