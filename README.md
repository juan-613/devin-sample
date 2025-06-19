# WEBアクセシビリティチェッカー

WEBアクセシビリティの違反箇所をチェックして、訂正内容をsuggestionするアプリケーションです。

## 📋 概要

このアプリケーションは、指定されたURLのWEBアクセシビリティをWCAG 2.2ガイドラインに基づいてチェックし、違反箇所を特定して修正提案を日本語で提供します。

### 主な機能

- **24個のWCAG適合項目チェック**: WCAG 2.2の主要な達成基準を網羅
- **日本語での詳細レポート**: 違反内容と修正提案を日本語で表示
- **リアルタイムチェック**: URLを入力するだけで即座にアクセシビリティ診断
- **視覚的な結果表示**: フロントエンドで分かりやすく結果を表示

## 🏗️ プロジェクト構成

```
web-accessibility-checker/
├── README.md                     # このファイル
├── web-accessibility-backend/    # バックエンド（Python FastAPI）
│   ├── app/
│   │   ├── main.py              # FastAPIメインアプリケーション
│   │   ├── accessibility_checker.py  # メインチェッカークラス
│   │   ├── models.py            # データモデル
│   │   └── checker/             # WCAG適合項目チェッカー
│   │       ├── wcag_1_1_1_non_text_content.py
│   │       ├── wcag_1_3_1_info_and_relationships.py
│   │       ├── wcag_2_1_1_keyboard.py
│   │       └── ... (全24個のチェッカー)
│   ├── config/
│   │   └── checklist.csv        # WCAG適合項目リスト
│   ├── pyproject.toml           # Poetry依存関係管理
│   └── poetry.lock
└── web-accessibility-frontend/   # フロントエンド（React + TypeScript）
    ├── src/
    │   ├── App.tsx              # メインReactコンポーネント
    │   ├── components/ui/       # UIコンポーネント
    │   └── ...
    ├── package.json
    └── ...
```

## 🚀 セットアップと起動方法

### 前提条件

- **Python 3.12+** (pyenvでインストール済み)
- **Node.js 18+** (nvmでインストール済み)
- **Poetry** (Python依存関係管理)
- **npm/pnpm** (Node.js依存関係管理)

### 1. バックエンドのセットアップと起動

```bash
# バックエンドディレクトリに移動
cd web-accessibility-backend

# Poetry依存関係をインストール
poetry install

# 開発サーバーを起動
poetry run fastapi dev app/main.py
```

バックエンドサーバーは `http://localhost:8000` で起動します。

#### バックエンドAPI確認

```bash
# ヘルスチェック
curl http://localhost:8000/health

# アクセシビリティチェック（例）
curl -X POST "http://localhost:8000/check" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com"}'
```

### 2. フロントエンドのセットアップと起動

```bash
# フロントエンドディレクトリに移動
cd web-accessibility-frontend

# 依存関係をインストール
npm install

# 開発サーバーを起動
npm run dev
```

フロントエンドサーバーは `http://localhost:5173` で起動します。

### 3. アプリケーションの使用

1. ブラウザで `http://localhost:5173` にアクセス
2. URLフィールドにチェックしたいWebサイトのURLを入力
3. 「チェック開始」ボタンをクリック
4. 結果が下部に表示されます

## 📊 チェック項目一覧

このアプリケーションは以下の24個のWCAG 2.2適合項目をチェックします：

### レベルA
- **1.1.1** 非テキストコンテンツ
- **1.3.1** 情報及び関係性
- **1.3.2** 意味のある順序
- **1.3.5** 入力目的の特定
- **1.4.4** テキストのサイズ変更
- **2.1.1** キーボード操作
- **2.1.2** フォーカス移動
- **2.2.1** 調整可能な制限時間
- **2.4.1** ブロックスキップ
- **2.4.2** ページタイトル
- **2.4.3** フォーカス順序
- **2.4.7** 視覚的に認識可能なフォーカス
- **3.1.1** ページの言語
- **3.2.1** オンフォーカス
- **3.2.2** ユーザインタフェース要素による状況の変化
- **4.1.2** 名前・役割・値

### レベルAA
- **1.4.10** リフロー
- **1.4.12** テキストの間隔
- **1.4.13** ホバー又はフォーカスで表示されるコンテンツ
- **2.5.1** ポインタのジェスチャ
- **2.5.2** ポインタのキャンセル
- **2.5.3** 名前のラベル
- **2.5.4** 動きによる起動
- **2.5.7** ドラッグ動作

## 🔧 開発・カスタマイズ

### 新しいチェッカーの追加

1. `web-accessibility-backend/app/checker/` に新しいチェッカーファイルを作成
2. `wcag_X_X_X_checker_name.py` の命名規則に従う
3. `check_function_name(html_content: str) -> List[Dict]` 関数を実装
4. `accessibility_checker.py` にインポートと登録を追加

### チェック項目の設定

`web-accessibility-backend/config/checklist.csv` でチェック項目を管理できます。

### フロントエンドのカスタマイズ

- **UI**: Tailwind CSS + shadcn/ui コンポーネント使用
- **状態管理**: React hooks
- **API通信**: fetch API

## 🧪 テスト

### バックエンドテスト

```bash
cd web-accessibility-backend

# 個別チェッカーのテスト
python -m pytest tests/

# 特定のチェッカーテスト
python test_1_1_1_non_text_content.py
```

### フロントエンドテスト

```bash
cd web-accessibility-frontend

# テスト実行
npm test

# ビルドテスト
npm run build
```

## 📝 使用例

### 基本的な使用方法

1. **URL入力**: チェックしたいWebサイトのURLを入力
2. **チェック実行**: 「チェック開始」ボタンをクリック
3. **結果確認**: 
   - 合格項目数と違反項目数のサマリー
   - 各WCAG項目の詳細結果
   - 違反箇所の具体的な修正提案

### API直接使用

```bash
# cURLでの直接API呼び出し
curl -X POST "http://localhost:8000/check" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://recruit-holdings.com/ja/"
  }'
```

## 🛠️ トラブルシューティング

### よくある問題

1. **ポート競合エラー**
   ```bash
   # 使用中のポートを確認
   lsof -i :8000  # バックエンド
   lsof -i :5173  # フロントエンド
   ```

2. **依存関係エラー**
   ```bash
   # バックエンド
   cd web-accessibility-backend
   poetry install --no-cache
   
   # フロントエンド
   cd web-accessibility-frontend
   rm -rf node_modules package-lock.json
   npm install
   ```

3. **CORS エラー**
   - バックエンドの `app/main.py` でCORS設定を確認
   - フロントエンドの `.env` でAPI URLを確認

### ログの確認

```bash
# バックエンドログ
poetry run fastapi dev app/main.py --log-level debug

# フロントエンドログ
npm run dev -- --debug
```

## 📚 参考資料

- [WCAG 2.2 ガイドライン](https://waic.jp/translations/WCAG22/)
- [FastAPI ドキュメント](https://fastapi.tiangolo.com/)
- [React ドキュメント](https://react.dev/)
- [Tailwind CSS](https://tailwindcss.com/)

## 🤝 貢献

1. このリポジトリをフォーク
2. 機能ブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 📞 サポート

問題や質問がある場合は、GitHubのIssuesページでお知らせください。

---

**開発者**: Devin AI  
**最終更新**: 2025年6月19日
