# Ball Pit Survivor — Google Play 提出ガイド

## 全体の流れ

```
PWA化 → ホスティング → APK/AAB生成 → Google Play 提出
```

---

## ステップ1：アイコン画像を作る

1. `icon-generator.html` をPCのブラウザで開く
2. 3つのボタンを順番にクリック：
   - 📥 icon-192.png をダウンロード
   - 📥 icon-512.png をダウンロード
   - 📥 feature-1024x500.png（ストア用）
3. ダウンロードした3ファイルを `ball_pit_survivor.html` と同じフォルダに置く
4. 気に入らなければ Canva や Figma で自作してもOK

---

## ステップ2：スクリーンショット撮影

ゲームをスマホで起動して、4〜8枚のスクリーンショットを撮る：
- タイトル画面
- プレイ中（敵がたくさん出てる場面）
- レベルアップ選択画面
- 装備獲得シーン
- ショップ画面
- ボスウェーブ

サイズは 1080×1920 (16:9縦) 推奨。

---

## ステップ3：ホスティング（オンライン公開）

### 推奨：Netlify Drop（無料・登録不要）

1. https://app.netlify.com/drop にアクセス
2. `outputs` フォルダを丸ごとドラッグ＆ドロップ
3. 数秒後にURLが発行される（例: `random-name.netlify.app`）
4. このURLでスマホで開いて動作確認

### 代替：Firebase Hosting / Vercel / GitHub Pages
無料枠あり。HTTPS必須。サービスワーカーはHTTPSでないと動かない。

---

## ステップ4：PWA → Android アプリ化

### 推奨：PWABuilder（最も簡単）

1. https://www.pwabuilder.com/ にアクセス
2. ステップ3で取得したURLを入力 → "Start"
3. PWAスコアが表示されるので、不足項目があれば修正
4. "Package for Stores" → "Android" を選択
5. パッケージID（例：`com.yourname.ballpitsurvivor`）を入力
6. "Download Package" でAAB/APKファイルが生成される

### 代替：Capacitor（より細かく制御したい場合）

```bash
# 必要なもの: Node.js, Android Studio
npm init -y
npm install @capacitor/core @capacitor/cli @capacitor/android
npx cap init "Ball Pit Survivor" "com.yourname.ballpitsurvivor" --web-dir=outputs
npx cap add android
npx cap copy android
npx cap open android
# Android Studioで Build → Generate Signed Bundle / APK
```

---

## ステップ5：Google Play Console 登録

### 必要なもの
- Googleアカウント
- $25（一回限りの開発者登録費）
- クレジットカード or デビットカード

### 手順
1. https://play.google.com/console/ にアクセス
2. 開発者アカウント作成（$25支払い）
3. 「アプリを作成」→ アプリ情報を入力：
   - **アプリ名**：Ball Pit Survivor
   - **簡単な説明**（80文字）：3レーンを駆け抜けて球を撃退するヴァンサバ風サバイバルゲーム
   - **詳しい説明**（4000文字）：ゲームの特徴を書く

### 必要な情報・素材
| 項目 | 内容 |
|---|---|
| アプリアイコン | 512×512 PNG |
| 機能グラフィック | 1024×500 PNG |
| スクリーンショット | 最低2枚（推奨4〜8枚） |
| プライバシーポリシーURL | 必須（後述） |
| カテゴリ | ゲーム > アクション |
| コンテンツレーティング | アンケートに回答 |
| 広告の有無 | 「広告なし」 |
| 対象年齢 | 任意 |

### プライバシーポリシー
ローカル保存（localStorage）のみ使用なら最低限の内容でOK：

> 当アプリは、ゲームの進捗（コイン、装備、設定）を端末内のローカルストレージに保存します。
> 個人情報の収集、外部送信、第三者への提供は一切行いません。
> 連絡先：[your-email@example.com]

このテキストをWebページとして公開（GitHub Pages、Notion、Bloggerなど）してURLを貼る。

---

## ステップ6：AABアップロード＆公開

1. Play Console > 「リリース」 > 「テスト」 > 「内部テスト」
2. AABファイルをアップロード
3. テスター（自分のメールアドレス）を追加
4. 「変更を保存」 → 「リリース」
5. 端末でテストリンクから → アプリインストール
6. 動作確認OKなら「製品版」リリース申請
7. 審査（通常2〜7日）→ 公開

---

## チェックリスト

- [ ] icon-192.png / icon-512.png / feature-1024x500.png を生成
- [ ] スクリーンショット 4枚以上 撮影
- [ ] outputsフォルダを Netlify Drop に公開
- [ ] スマホでURLを開いて動作確認
- [ ] PWABuilder で AAB/APK 生成
- [ ] Google Play Console で開発者登録（$25）
- [ ] プライバシーポリシーをWebに公開
- [ ] ストア掲載情報を入力
- [ ] AABアップロード → 内部テスト
- [ ] 製品版リリース申請

---

## トラブル時のヒント

- **PWABuilder でスコアが低い**：manifest.json と sw.js が正しくホストされているか確認
- **iOS Safari で音が出ない**：画面を一回タップしないとオーディオが起動しない（既に対応済）
- **APK起動時に真っ黒**：service worker のキャッシュをリセット（リインストール）
- **Google Play 審査落ち**：プライバシーポリシーの記述、コンテンツレーティング、ターゲットAPIレベルを確認

---

## ファイル構成

公開フォルダの中身：
```
outputs/
├── ball_pit_survivor.html  ← メインのゲーム
├── manifest.json           ← PWAマニフェスト
├── sw.js                   ← サービスワーカー（オフライン対応）
├── icon-192.png            ← アプリアイコン（小）
├── icon-512.png            ← アプリアイコン（大）
├── icon-generator.html     ← アイコン生成ツール
├── feature-1024x500.png    ← ストア用バナー
└── GOOGLE_PLAY_GUIDE.md    ← このガイド
```

これら全部をNetlify Dropにアップロードすれば公開準備完了。
