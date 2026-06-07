# HachiCrauncher — Microsoft ログイン (Azure アプリ) 設定

## 現状
`app/assets/js/ipcconstants.js` の `AZURE_CLIENT_ID` は **Helios 公式の暫定 ID** のまま。
→ ログインは**動く**が、Microsoft の同意画面にアプリ名 **"Helios"** が表示される。
自分の Azure アプリを登録して差し替えると、同意画面が **"HachiCrauncher"** になる（ブランディング用・任意）。

## 登録手順（約5分・要 Microsoft アカウント）
1. https://portal.azure.com → **Microsoft Entra ID** → **アプリの登録** → **新規登録**
2. 名前: `HachiCrauncher`
3. サポートされているアカウントの種類: **「個人用 Microsoft アカウントのみ」**
4. リダイレクト URI: プラットフォーム **「モバイルおよびデスクトップ アプリケーション」**
   URI = `https://login.microsoftonline.com/common/oauth2/nativeclient`
5. **登録** → 概要ページの **「アプリケーション (クライアント) ID」** をコピー
6. 「認証」タブ → **「パブリック クライアント フローを許可する」= はい** にして保存

## 差し込み & 再ビルド
1. `app/assets/js/ipcconstants.js` の `exports.AZURE_CLIENT_ID = '...'` を上記 ID に置換
2. 再ビルド:
   ```powershell
   $env:CSC_IDENTITY_AUTO_DISCOVERY='false'   # コード署名なし
   npm run dist:win
   ```
   ※ winCodeSign のシンボリックリンク展開エラー回避のため、初回は
   `%LOCALAPPDATA%\electron-builder\Cache\winCodeSign\winCodeSign-2.6.0` に
   installer の winCodeSign を展開済みにしておく（このリポで対応済み）。
3. `dist\HachiCrauncher-setup-<version>.exe` を配布。

> client ID を教えてくれれば、差し込み＋再ビルドはこちらで実行可能。
