# Changelog

All notable changes to Interactive Video Platform.

## [1.1.0] - 2026-02-07

### Changed
- **リポジトリ名変更**: `japan-tts` → `interactive-video-platform`
- **SKILL.md更新**: インタラクティブ動画プラットフォーム全体を反映
- **アーキテクチャ図追加**: 4層構造（Content/Media/Player/Deployment）

### Added
- 10パターンVSL構造の説明
- 分岐ロジック（Branch Structure）図
- デプロイ手順（Vercel）
- 品質検証（Playwright + Agentic Vision）

## [1.0.0] - 2026-02-07

### Added
- **Fish Audio API統合** (`fish_tts.py`)
  - LLM内蔵で文脈理解による高品質TTS
  - デフォルトボイスID: d4c86c697b3e4fc090cf056f17530b2a
  - MP3/WAV出力対応

- **Style-Bert-VITS2統合** (`style_bert_tts.py`)
  - ローカル高品質TTS
  - 7スタイル対応: Neutral, Happy, Sad, Angry, Fearful, Surprised, Disgust
  - アクセント制御機能

- **日本語前処理** (`text_preprocessor.py`)
  - 数字読み変換ルール実装
    - 1000 → せん
    - 1000万 → いっせんまん
    - 1000億 → いっせんおく
    - 1億 → いちおく
  - 全角/半角正規化

- **実行ラッパー** (`run.py`)
  - 仮想環境自動セットアップ
  - ショートカット対応 (fish, sbv2, preprocess)

- **ドキュメント** (`SKILL.md`)
  - アーキテクチャ図
  - 使用方法
  - パラメータリファレンス
  - トラブルシューティング
