# N8N評価システム統合設定

## 🔗 既存N8Nワークフロー活用

前回セッションで完成したN8N評価ワークフローを流用します。

### N8Nワークフロー概要

```
Webhook受信 → OpenAI評価 → JSON応答返却
```

### 受信データ形式

N8Nは以下の形式でデータを受信します：

```json
{
  "data": {
    "transcript": [
      {
        "message": "こんにちは、お電話ありがとうございます。",
        "speaker": "agent",
        "timestamp": "2025-06-02T00:10:00.000Z"
      },
      {
        "message": "御社のサービスについて質問があります。",
        "speaker": "customer",
        "timestamp": "2025-06-02T00:10:15.000Z"
      }
    ],
    "metadata": {
      "call_duration_secs": 300,
      "scenario": "general",
      "start_time": "2025-06-02T00:10:00.000Z",
      "end_time": "2025-06-02T00:15:00.000Z"
    }
  }
}
```

### N8Nからの応答形式

```json
{
  "overall_score": 17,
  "detailed_scores": {
    "response_quality": 4,
    "communication_skills": 4,
    "problem_solving": 3,
    "professionalism": 3,
    "customer_satisfaction": 3
  },
  "strengths": [
    "丁寧な対応ができていました",
    "適切な質問で状況を把握していました"
  ],
  "improvements": [
    "より具体的な解決策の提示が必要です",
    "応答時間をもう少し短縮できます"
  ]
}
```

## ⚙️ 統合手順

### Step 1: N8N WebhookエンドポイントURL取得

1. N8Nにログイン
2. 既存の評価ワークフローを開く
3. WebhookノードのURLをコピー
4. 例: `https://your-n8n-instance.app.n8n.cloud/webhook/evaluation`

### Step 2: Webアプリ設定更新

`index.html`の189行目周辺、`evaluateConversation()`関数内：

```javascript
// 現在のコード（196-199行目）
// TODO: ここで実際のN8N評価システムに送信
// const n8nResponse = await fetch('https://your-n8n-webhook-url.com', {
//     method: 'POST',
//     headers: { 'Content-Type': 'application/json' },
//     body: JSON.stringify({ data: conversationData })
// });
// const evaluation = await n8nResponse.json();

// 以下に置き換え
const n8nResponse = await fetch('YOUR_N8N_WEBHOOK_URL', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ data: conversationData })
});

if (!n8nResponse.ok) {
    throw new Error(`N8N Error: ${n8nResponse.status}`);
}

const evaluation = await n8nResponse.json();
displayEvaluation(evaluation);

// モックデータ部分を削除またはコメントアウト
```

### Step 3: CORS設定（必要な場合）

N8NでCORS設定が必要な場合：

```javascript
// N8Nワークフローの最初にHTTP Responseノードを追加
{
  "headers": {
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "POST, OPTIONS",
    "Access-Control-Allow-Headers": "Content-Type"
  }
}
```

## 🧪 テスト手順

### 1. ローカルテスト

```bash
#簡単なテストスクリプト
curl -X POST https://YOUR_N8N_WEBHOOK_URL \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "transcript": [
        {"message": "テスト", "speaker": "agent", "timestamp": "2025-06-02T00:10:00.000Z"}
      ],
      "metadata": {
        "call_duration_secs": 60,
        "scenario": "general"
      }
    }
  }'
```

### 2. Webアプリテスト

1. 研修を開始
2. 簡単な会話を実行
3. 「研修終了」でN8N呼び出し
4. 評価結果の表示を確認

## 🔧 トラブルシューティング

### よくある問題

1. **CORS エラー**
   - N8NでCORS設定を追加
   - または、GitHub PagesのHTTPS設定確認

2. **認証エラー**
   - N8N WebhookのAuthentication設定確認

3. **データ形式エラー**
   - 送信データとN8Nの期待形式を比較
   - コンソールログでデータ確認

### デバッグ方法

```javascript
// データ送信前にログ出力
console.log('N8Nに送信するデータ:', JSON.stringify(conversationData, null, 2));

// レスポンス確認
const evaluation = await n8nResponse.json();
console.log('N8Nからの応答:', evaluation);
```

## 📊 データ流れ図

```
Webアプリ (研修終了)
    ↓
conversationData生成
    ↓
N8N Webhook送信
    ↓
N8N評価ワークフロー実行
    ↓
OpenAI評価API呼び出し
    ↓
評価結果JSON生成
    ↓
Webアプリに応答返却
    ↓
評価パネル表示
```

## 🚀 最適化のヒント

1. **エラーハンドリング強化**
   ```javascript
   try {
     const evaluation = await callN8NEvaluation(conversationData);
     displayEvaluation(evaluation);
   } catch (error) {
     // フォールバック処理
     displayMockEvaluation();
   }
   ```

2. **リトライ機能**
   ```javascript
   async function callN8NWithRetry(data, maxRetries = 3) {
     for (let i = 0; i < maxRetries; i++) {
       try {
         return await fetch(n8nUrl, options);
       } catch (error) {
         if (i === maxRetries - 1) throw error;
         await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
       }
     }
   }
   ```

3. **ローディング状態改善**
   ```javascript
   updateChatStatus('N8N評価システムと通信中...');
   // プログレスバーやスピナー表示
   ```

## 📝 統合チェックリスト

- [ ] N8N WebhookエンドポイントURL取得
- [ ] `index.html`のTODO部分更新
- [ ] CORS設定確認
- [ ] テストデータでの動作確認
- [ ] エラーハンドリング実装
- [ ] 本番データでの最終テスト

## 🎯 次のステップ

統合完了後：
1. 評価精度の確認
2. 応答時間の最適化
3. エラーログの分析
4. ユーザーフィードバック収集