---
name: interactive-video-platform
description: インタラクティブ動画プラットフォーム。視聴者の選択で分岐するVSL（ビデオセールスレター）システム。日本語TTS統合、10パターンVSL、Remotion動画生成、Vercelデプロイ対応。
---

# Interactive Video Platform - インタラクティブ動画プラットフォーム

視聴者の選択によってストーリーが分岐するインタラクティブVSL（ビデオセールスレター）システム。

## When to Use This Skill

以下のキーワードで発動:
- 「インタラクティブ動画」「分岐動画」「対話型VSL」
- 「選択式動画」「ストーリー分岐」
- 「視聴者参加型」「パーソナライズ動画」
- `/interactive-video` `/vsl-platform`

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│              Interactive Video Platform Architecture                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  Layer 1: Content Generation                                 │   │
│  │  ├─ 10 VSL Patterns (パターン別シナリオ)                     │   │
│  │  ├─ Branch Logic (分岐ロジック A/B/C)                        │   │
│  │  └─ Script Generation (台本自動生成)                         │   │
│  └─────────────────────────────────────────────────────────────┘   │
│       ↓                                                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  Layer 2: Media Generation                                   │   │
│  │  ├─ Image Generation (NanoBanana Pro / Gemini)               │   │
│  │  ├─ TTS Audio (Fish Audio / Style-Bert-VITS2)                │   │
│  │  └─ Video Composition (Remotion)                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│       ↓                                                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  Layer 3: Interactive Player                                 │   │
│  │  ├─ Next.js App (React)                                      │   │
│  │  ├─ Choice UI (選択ボタン)                                    │   │
│  │  ├─ Branch Navigation (分岐遷移)                              │   │
│  │  └─ Analytics (視聴データ収集)                                │   │
│  └─────────────────────────────────────────────────────────────┘   │
│       ↓                                                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  Layer 4: Deployment                                         │   │
│  │  ├─ Vercel (本番デプロイ)                                     │   │
│  │  ├─ CDN (動画配信)                                            │   │
│  │  └─ Quality Check (Playwright検証)                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## 10 VSL Patterns

| Pattern | Style | Description |
|---------|-------|-------------|
| 01 | Pop Animation | ポップなアニメーション風 |
| 02 | Minimal Corporate | ミニマル企業向け |
| 03 | Cinematic Drama | 映画的ドラマ風 |
| 04 | Anime Style | アニメスタイル |
| 05 | Documentary | ドキュメンタリー風 |
| 06 | Tech Futuristic | テック未来風 |
| 07 | Luxury Premium | ラグジュアリープレミアム |
| 08 | Realistic Photo | フォトリアリスティック |
| 09 | Illustration Hand-drawn | 手描きイラスト風 |
| 10 | Motion Graphics | モーショングラフィックス |

## Branch Structure

```
Start (共通導入)
    ↓
Scene 1-2 (問題提起)
    ↓
Choice Point 1: あなたの状況は？
    ├─ A: 副業を始めたい
    ├─ B: 独立・起業したい
    └─ C: 既存ビジネスを拡大したい
         ↓
Branch A/B/C (パーソナライズ展開)
    ↓
Scene 3-4 (解決策提示)
    ↓
Choice Point 2: 興味のある方法は？
    ├─ A: AI活用
    ├─ B: マーケティング
    └─ C: 投資・資産運用
         ↓
Final Branch (クロージング)
    ↓
CTA (行動喚起)
```

## Quick Start

```bash
cd ~/.claude/skills/interactive-video-platform

# 1. 日本語TTS音声生成
python scripts/run.py fish_tts.py \
  --text "2026年、1000万円の投資で始める新時代" \
  --output output/narration.mp3

# 2. テキスト前処理（数字読み確認）
python scripts/run.py preprocessor.py \
  --text "1000万円を投資" \
  --show-reading
```

## Japanese TTS Integration

### 数字読みルール（自動適用）

| 入力 | 出力読み | ルール |
|------|---------|--------|
| 1000 | せん | 単独では「いち」不要 |
| 1000万 | いっせんまん | 万の直前では「いっせん」必須 |
| 1000億 | いっせんおく | 億の直前では「いっせん」必須 |
| 1億 | いちおく | 万以上は「いち」必須 |

### TTS Engine Comparison

| 項目 | Fish Audio | Style-Bert-VITS2 |
|------|-----------|------------------|
| **文脈理解** | LLM内蔵（最高） | BERT（良好） |
| **数字読み** | 自動 | 前処理必要 |
| **コスト** | 課金制 | 無料 |
| **品質** | 5/5 | 4/5 |
| **オフライン** | No | Yes |

## Deployment

### Vercel Deploy

```bash
# パターン別デプロイ
vercel --prod

# 全10パターン一括デプロイ
for i in {01..10}; do
  cd pattern-$i && vercel --prod && cd ..
done
```

### Deployed URLs (Example)

| Pattern | URL |
|---------|-----|
| 01 | https://interactive-vsl-pattern-01.vercel.app |
| 02 | https://interactive-vsl-pattern-02.vercel.app |
| ... | ... |

## Quality Verification

Playwright + Agentic Visionによる自動検証:

```bash
# 品質チェック項目
1. 動画が正常に再生されるか
2. 選択ボタンが機能するか
3. スタイルが維持されているか
4. 縦型9:16フォーマットが正しいか
5. 日本語テキストが正しく表示されているか
```

## Environment Variables

```bash
# Fish Audio API（音声生成）
export FISH_AUDIO_API_KEY="your-api-key"

# Style-Bert-VITS2（ローカル音声）
export STYLE_BERT_VITS2_URL="http://localhost:5000"

# Vercel（デプロイ）
export VERCEL_TOKEN="your-vercel-token"
```

## File Structure

```
interactive-video-platform/
├── SKILL.md                    # このドキュメント
├── CHANGELOG.md                # 変更履歴
├── scripts/
│   ├── run.py                  # 実行ラッパー
│   ├── fish_tts.py             # Fish Audio API
│   ├── style_bert_tts.py       # Style-Bert-VITS2
│   └── text_preprocessor.py    # 日本語前処理
├── patterns/                   # 10パターンVSL
│   ├── pattern-01/             # Pop Animation
│   ├── pattern-02/             # Minimal Corporate
│   └── ...
├── data/
│   └── user_dict.csv           # カスタム読み辞書
├── output/                     # 生成音声
└── models/                     # ローカルモデル
```

## Related Skills

| Skill | Description |
|-------|-------------|
| `video-agent` | 動画生成パイプライン |
| `omnihuman1-video` | AIアバター動画 |
| `anime-slide-generator` | スライド動画 |
| `nanobanana-pro` | AI画像生成 |
| `agentic-vision` | 品質検証 |

## References

- [Fish-Speech Paper](https://arxiv.org/html/2411.01156v1)
- [Style-Bert-VITS2 GitHub](https://github.com/litagin02/Style-Bert-VITS2)
- [Remotion Documentation](https://www.remotion.dev/)
- [Next.js Documentation](https://nextjs.org/docs)
