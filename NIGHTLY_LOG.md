# NIGHTLY LOG — 夜間作業の朝引き継ぎ

夜間ナイトリー・エージェントが毎晩ここに追記します。ユーザーは朝これを上から
（新しい順）読み、`git diff main..origin/nightly` と合わせてレビューします。

## エントリ・フォーマット（エージェントは毎夜これを先頭に追記）

```
## YYYY-MM-DD（JST）

- **実装タスク**: バックログの#N「タイトル」
- **変更ファイル**: 一覧
- **sw.js CACHE_NAME**: bps-vX → bps-vY
- **構文チェック**: node --check 通過 / 失敗（詳細）
- **テスト手順**: ユーザーが朝に実機で確認すべき具体的操作
- **確認ポイント**: 特に注意して見てほしい挙動・リスク
- **バランス提案**（任意・実装はしない）: 気づいたバランス改善案
- **バグ観察**（任意）: 気づいた既存バグ
- **状態**: 完了 / 中断（理由）
```

朝のレビュー手順:
1. `git -C "C:\Users\hukud\Desktop\BALL SURV" fetch`
2. `git -C "C:\Users\hukud\Desktop\BALL SURV" diff main..origin/nightly`
3. このログの最新エントリを読む
4. 良ければ `nightly` を `main` にマージ → GitHub Pages 反映 → スマホで
   `https://koya800.github.io/ball-pit-survivor/` を**完全に閉じて再起動**し実機テスト
5. 駄目なら該当変更を破棄し、`NIGHTLY_BACKLOG.md` のタスク内容を調整

---

## 2026-05-18（JST） — セットアップ（自動作業ではなく初期設定）

- **実装タスク**: なし（夜間スケジュール基盤の設置）
- **変更ファイル**: `NIGHTLY_BACKLOG.md`（新規）, `NIGHTLY_LOG.md`（新規）
- **状態**: 完了。次回の夜間ジョブからバックログ#1を消化開始。
