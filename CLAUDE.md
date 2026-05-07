# Ball Pit Survivor

3レーン縦型のヴァンサバ風サバイバルシューター。スマホ向けHTML5ゲーム。
最終的に **Google Play で公開予定**（PWA → AAB変換）。

## ファイル構成

```
ball_pit_survivor.html  ← ゲーム本体（単一HTMLファイル、約95KB）
manifest.json           ← PWAマニフェスト
sw.js                   ← サービスワーカー（オフライン対応）
icon-generator.html     ← アプリアイコン生成ツール
GOOGLE_PLAY_GUIDE.md    ← 提出手順書
```

## 設計方針

- **単一HTMLファイル完結**：外部依存なし、画像も使わずCanvasで描画
- **Web Audio APIでBGM/SFX合成**：音声ファイル不要
- **localStorage で永続化**：装備（最大60個）・コイン・8種の永続強化・選択ステージ・BGM ON/OFF
- **モバイル前提**：縦持ち、3レーン、タップ操作

## 主要システム

- **武器11種＋進化11種**（合計22武器）：bolt/spread/laser/shock/drone/mine/boomerang/firewall/iceshard/plane/orange + それぞれの進化形
- **武器最大Lv8**：各レベルで具体的な変化が `lvlText` 配列で表示される
- **進化条件**：Lv5以上＋特定パッシブ所持
- **17種パッシブ**：攻撃/連射/弾速/マルチ/取得範囲/HP/全回復/再生/移動/貫通/クリ率/クリ倍率/EXP/幸運/吸血/回避/CD短縮
- **ローグライク装備**：3スロット（武器補助/防具/装飾品）×5レアリティ×5ネーム = 75種ベース＋ステ抽選で実質無限
- **永続強化ショップ**：8種（HP/攻撃/連射/弾速/クリ率/コイン/EXP/取得範囲）
- **敵タイプ6種**：normal/zigzag(瞬間移動)/sprinter/shooter/shielded/splitter
- **ボーナス敵**（金色キラキラ、25〜35秒ごと）
- **合体システム**（同レーン2体接近で合体、30秒以降）
- **ボスウェーブ**（60秒ごと、装備100%ドロップ）
- **5分耐久クリア**

## 編集時の注意

⚠️ **致命的な落とし穴**：
- ファイル末尾の `</script>\n</body>\n</html>` を絶対に消さない
- Edit操作後は **必ず構文チェック** すること：
  ```bash
  python3 -c "import re; html=open('ball_pit_survivor.html').read(); js=re.search(r'<script>(.*?)</script>',html,re.DOTALL).group(1); open('/tmp/t.js','w').write(js)"
  node --check /tmp/t.js
  ```
- Edit が末尾を勝手に切ることがあるので、編集後は `tail -3` で `</html>` まで残っているか確認
- 切れていたらPythonで `.write` で末尾を書き戻す

## Google Play 提出までの最短ルート

1. `icon-generator.html` をブラウザで開いて icon-192.png / icon-512.png / feature-1024x500.png をダウンロード
2. このフォルダごと https://app.netlify.com/drop にドラッグ → 公開URL取得
3. https://www.pwabuilder.com/ にURLを貼って AAB 生成
4. Google Play Console（$25）で AAB アップロード → 内部テスト → 製品版

詳細は `GOOGLE_PLAY_GUIDE.md` 参照。

## 今後の開発候補

- ステージ選択（背景＋難易度モード、コードに `STAGES` 定義済み・未統合）
- 新武器（チェーンライティング、サイクロン）
- 実績システム
- デイリーチャレンジ
- 真のラスボス（5分時点で出現、倒せば真クリア）
