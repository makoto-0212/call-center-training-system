# 🎯 コールセンター研修システム

## 📋 プロジェクト概要

OpenAI Chat API + N8N評価システムを統合したテキストベース研修システムです。
既存の11Labs + N8N + OpenAI評価システムの代替として開発され、段階的に音声機能を追加予定です。

## ⚡ 特徴

- **🗣️ リアルな研修環境**: チャット形式で田中太郎AIと対話
- **📊 自動評価**: 既存N8N評価システムと完全統合
- **🎯 5つのシナリオ**: 基本対応からクレーム対応まで
- **💰 低コスト**: 約$0.1/5分会話
- **🚀 即座利用**: GitHub Pages即座デプロイ

## 🔧 システム構成

```
Webアプリ(HTML/JS) ↔ OpenAI Chat API ↔ 既存N8N評価システム
```

## 📊 評価システム

### 評価項目（各5点満点）
- 対応品質
- コミュニケーションスキル
- 問題解決能力
- プロフェッショナリズム
- 顧客満足度

### 出力形式
```json
{
  "overall_score": 18,
  "detailed_scores": {
    "response_quality": 4,
    "communication_skills": 4,
    "problem_solving": 3,
    "professionalism": 4,
    "customer_satisfaction": 3
  },
  "strengths": ["丁寧な対応", "適切な確認"],
  "improvements": ["より具体的な提案", "感情的配慮の向上"]
}
```

## 🚀 使用方法

### 1. システムアクセス
https://makoto-0212.github.io/call-center-training-system/

### 2. 初期設定
- OpenAI API Key を入力
- N8N Webhook URL を入力（既存評価システム）

### 3. 研修実行
1. シナリオを選択
2. 「研修開始」と入力
3. 田中太郎AIと会話
4. 「研修終了」で評価実行

## 🔧 技術仕様

- **フロントエンド**: HTML + CSS + JavaScript
- **AI API**: OpenAI Chat API (GPT-4o-mini)
- **評価システム**: 既存N8N評価ワークフロー
- **ホスティング**: GitHub Pages
- **コスト**: 約$0.1/5分会話

## 📅 開発ロードマップ

### Phase 1: テキストベース（完了）
- ✅ チャット形式UI
- ✅ OpenAI Chat API統合
- ✅ N8N評価システム統合
- ✅ 5シナリオ対応

### Phase 2: 音声機能追加（予定）
- 🔄 OpenAI TTS/STT統合
- 🔄 音声入出力対応
- 🔄 品質最適化

### Phase 3: 高機能化（予定）
- 🔄 OpenAI Realtime API
- 🔄 マルチユーザー対応
- 🔄 ダッシュボード機能

## 📊 前身システム

11Labs + N8N + OpenAI Post-Call評価システム（95%完成）からの移行プロジェクト。
クレジット制限により、より持続可能なOpenAI中心システムへ移行。

詳細: [claude-conversation-backup/sessions/2025-05-29-0130-openai-alternative-system-design](https://github.com/makoto-0212/claude-conversation-backup/tree/main/sessions/2025-05-29-0130-openai-alternative-system-design)

## 🔑 必要な設定

### OpenAI API Key
1. [OpenAI Platform](https://platform.openai.com/api-keys) でAPI Key取得
2. 課金設定の確認
3. システムに入力

### N8N Webhook URL
既存の評価ワークフローのWebhook URLを設定

## 📈 コスト試算

| 項目 | コスト | 詳細 |
|------|--------|------|
| GPT-4o-mini | ~$0.08/5分 | 入力$0.15/1M + 出力$0.6/1M tokens |
| N8N | 無料 | 既存システム流用 |
| GitHub Pages | 無料 | ホスティング |
| **合計** | **~$0.1/5分** | **非常に低コスト** |

## 🎯 今後の拡張予定

- 音声機能追加（OpenAI TTS/STT）
- 複数シナリオ対応
- 進捗ダッシュボード
- マルチユーザー対応
- AI研修カリキュラム自動生成

---

**開発者**: makoto-0212  
**開発期間**: 2025-05-29~  
**バージョン**: 1.0.0 (テキストベース完全版)