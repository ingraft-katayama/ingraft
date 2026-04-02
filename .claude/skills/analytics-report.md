---
name: analytics-report
description: Google Analyticsのアクセス解析レポートを生成する
user_invocable: true
---

# アクセス解析レポート生成

Google Analyticsのデータを取得し、日本語のアクセス解析レポートを作成します。

## 手順

1. **データ取得方法の選択**
   - GA4 Data APIが利用可能な場合（`GA_PROPERTY_ID` と `GA_SERVICE_ACCOUNT_KEY_PATH` が設定済み）:
     → API経由でデータ取得
   - APIが未設定の場合:
     → Claude in Chrome MCPでGoogle Analyticsをブラウザ操作してデータ取得
     → ブラウザも使えない場合は「GA APIの設定が必要です」と案内

2. **収集する指標**
   - ページビュー数（PV）
   - ユニークユーザー数（UU）
   - 直帰率
   - 平均セッション時間
   - トラフィックソース（検索、SNS、直接、リファラル）
   - 上位閲覧ページ
   - デバイス別アクセス（PC / モバイル / タブレット）

3. **レポート作成**
   以下のフォーマットで日本語レポートを作成:

   ```
   # アクセス解析レポート YYYY-MM-DD

   ## 期間: YYYY/MM/DD 〜 YYYY/MM/DD

   ## サマリー
   - PV: X,XXX（前週比 +XX%）
   - UU: X,XXX（前週比 +XX%）
   - 直帰率: XX%
   - 平均セッション時間: X分XX秒

   ## トラフィックソース
   1. 検索: XX%
   2. SNS: XX%
   3. 直接: XX%
   4. リファラル: XX%

   ## 上位ページ
   1. /path - X,XXX PV
   2. /path - X,XXX PV

   ## 改善提案
   - データに基づく具体的な改善施策
   ```

4. **共有**
   - レポートをチャットに表示
   - Slack MCPが利用可能であれば、指定チャンネルに投稿

## 改善提案の観点
- PVが低いページ → コンテンツ改善またはSEO対策
- 直帰率が高い → UX改善、CTA配置見直し
- モバイル比率が高いのにPC向けデザイン → レスポンシブ強化
- SNSからの流入が少ない → SNS運用強化
