# 表サイト（公開サイト）拡張版 - 実装ガイド

## 📋 概要

ARG「海蝕機関」の表サイトを大幅に拡充したバージョンです。
企業サイトとしてのリアリティを高めつつ、多数の謎解き要素とヒントを埋め込んでいます。

## 📁 含まれるファイル

### HTMLファイル
- `company_enhanced.html` - 企業情報ページ
- `services_enhanced.html` - 事業内容ページ
- `research_enhanced.html` - 研究姿勢ページ
- `contact_enhanced.html` - お問い合わせページ

## 🎯 主な改善点

### 1. コンテンツの充実

**企業情報ページ（company_enhanced.html）:**
- 詳細な会社概要テーブル
- タイムライン形式の沿革（1983年〜2024年）
- 6つの部門の組織構成
- 事業実績の数値データ
- 企業理念と行動規範

**事業内容ページ（services_enhanced.html）:**
- 4つのサービスの詳細説明
  - 研究支援サービス
  - 技術検証サービス
  - 資料整理・分析サービス
  - 特殊対応サービス（機密）
- 各サービスの特徴を4項目ずつ紹介
- 実際の事例紹介（一部黒塗り）
- 料金表

**研究姿勢ページ（research_enhanced.html）:**
- 3つの基本理念の詳細解説
- 6つの倫理規範
- 品質管理プロセスのフロー図
- 代表者の引用
- 重要な注意事項（警告セクション）

**お問い合わせページ（contact_enhanced.html）:**
- 通常のお問い合わせフォーム
- 隠された特殊案件専用フォーム
- 詳細な連絡先情報
- プライバシーポリシーへのリンク

### 2. 謎解き要素の実装

#### Glitch効果
特定のテキストに視覚的な「異常」を表現：
```css
.glitch {
  animation: glitch 3s infinite;
}
```

#### 黒塗りテキスト
機密情報を示唆する黒塗り（クリックで表示）：
```html
<span class="redacted">機密情報</span>
```

#### イースターエッグ

**1. 会社概要テーブルを10回クリック**
```javascript
// company_enhanced.html
// 「従業員数が120名？でも名簿には230名...」というメッセージ
```

**2. ロゴを5回クリック（contact.html）**
```javascript
// 内部ポータルへのアクセスコードをコンソールに表示
// 認証コード: "boundary" または "kaishoku"
```

**3. 特殊案件フォームへのアクセス**
```javascript
// お問い合わせ種別で「特殊案件に関する相談」を選択
// 隠しフォームが出現
// 正しい認証コードで内部ポータルへ
```

**4. 警告セクションを5回クリック**
```javascript
// research_enhanced.html
// 緊急通報メッセージをコンソールに表示
```

**5. コンソールメッセージ**
全ページで様々なヒントや警告をブラウザコンソールに出力

### 3. ビジュアルデザインの強化

**タイムライン表示:**
```css
.timeline {
  position: relative;
  padding-left: 2rem;
}
.timeline::before {
  /* 縦線のグラデーション */
}
```

**組織図カード:**
```css
.org-card {
  transition: all 0.3s ease;
}
.org-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.3);
}
```

**統計カード:**
```css
.stat-card {
  text-align: center;
}
.stat-number {
  font-size: 2.5rem;
  color: #00ffff;
}
```

## 🚀 実装方法

### 1. ファイルの配置

```bash
# 既存ファイルのバックアップ
cp /company.html /company_backup.html
cp /services.html /services_backup.html
cp /research.html /research_backup.html
cp /contact.html /contact_backup.html

# 新しいファイルをコピー
cp company_enhanced.html /company.html
cp services_enhanced.html /services.html
cp research_enhanced.html /research.html
cp contact_enhanced.html /contact.html
```

### 2. スタイルの統合

既存の `styles.css` に以下のクラスが必要です：
- `.hero` - ヒーローセクション
- `.section` - セクション区切り
- `.card` - カードコンポーネント
- `.button` - ボタンスタイル
- `.header`, `.footer` - ヘッダー・フッター

すでに実装されている場合は、そのまま使用できます。

### 3. 内部ポータルとの連携

特殊案件フォームから内部ポータルへのリンク：
```javascript
// contact_enhanced.html内
if (authCode.toLowerCase() === 'kaishoku' || 
    authCode.toLowerCase() === '海蝕' || 
    authCode.toLowerCase() === 'boundary') {
  window.location.href = '/internal/login.html';
}
```

## 🎨 カスタマイズ

### 会社情報の変更

**会社概要テーブル（company_enhanced.html）:**
```html
<tr>
  <td style="color: #9ca3af;">社名</td>
  <td>東亜先端研究株式会社</td>
</tr>
```

### 沿革の追加

```html
<div class="timeline-item">
  <div class="timeline-year">YYYY年</div>
  <div class="timeline-content">
    出来事の説明...
  </div>
</div>
```

### 新しいサービスの追加

```html
<div class="service-detail">
  <div class="service-header">
    <div class="service-icon">🆕</div>
    <div>
      <div class="service-title">新サービス名</div>
      <div class="service-subtitle">Service Subtitle</div>
    </div>
  </div>
  <!-- サービス内容 -->
</div>
```

## 🔍 隠し要素一覧

| 場所 | トリガー | 内容 |
|------|---------|------|
| company.html | テーブルを10回クリック | 従業員数の矛盾についてのアラート |
| services.html | `.redacted`を3回クリック | 黒塗りテキストが表示される |
| research.html | 警告セクションを5回クリック | コンソールに緊急通報メッセージ |
| contact.html | ロゴを5回クリック | コンソールに認証コード表示 |
| contact.html | 特殊案件を選択 | 隠しフォームが出現 |
| contact.html | 認証コード入力 | 内部ポータルへリダイレクト |

## 📱 レスポンシブ対応

全ページがモバイルデバイスに対応：
- タブレット: 768px以下
- スマートフォン: 480px以下

```css
@media (max-width: 768px) {
  .timeline::before {
    left: 20px;
  }
  .org-chart {
    grid-template-columns: 1fr;
  }
}
```

## 🎯 プレイヤー誘導戦略

### 1. 段階的な疑念の喚起

**第一段階（通常の企業サイト）:**
- 一見、普通の研究支援会社
- 専門的で信頼できそうな印象

**第二段階（違和感の植え付け）:**
- 微妙に曖昧な業務内容
- glitch効果による視覚的異常
- 「特殊」という言葉の頻出

**第三段階（明確な異常）:**
- 黒塗りテキスト
- 警告メッセージ
- コンソールログの怪しいメッセージ

**第四段階（内部ポータルへの誘導）:**
- 特殊案件フォームの発見
- 認証コードの探索
- 内部ポータルへのアクセス

### 2. 外部からのヒント

SNSやフォーラムでの議論を想定：
- 「会社概要のテーブルをクリックしてみて」
- 「コンソールに何か出てない？」
- 「contact.htmlの特殊案件って何？」
- 「認証コードは"境界"に関係してるらしい」

## 🐛 トラブルシューティング

### Glitch効果が動作しない

```css
/* styles.cssに追加 */
@keyframes glitch {
  0%, 90%, 100% { opacity: 1; transform: translateX(0); }
  92% { opacity: 0.8; transform: translateX(-2px); }
  94% { opacity: 1; transform: translateX(2px); }
  96% { opacity: 0.8; transform: translateX(-1px); }
  98% { opacity: 1; transform: translateX(1px); }
}

.glitch {
  animation: glitch 3s infinite;
}
```

### 内部ポータルへのリダイレクトが動作しない

パスを確認：
```javascript
// 相対パスの場合
window.location.href = './internal/login.html';

// 絶対パスの場合
window.location.href = '/internal/login.html';
```

### フォームが送信されない

```javascript
// 実際のバックエンドがない場合は、alertで代用
alert('お問い合わせを受け付けました。');
```

## 📊 SEO対策

各ページに適切なmeta情報を設定：
```html
<title>企業情報 | 東亜先端研究株式会社</title>
<meta name="description" content="...">
<meta name="keywords" content="研究支援,海洋学,技術検証">
```

## 🔐 セキュリティ注意事項

これはフィクションのARGプロジェクトです：
- 実際の企業や組織を装わないこと
- 実在する電話番号やメールアドレスを使用しないこと
- プレイヤーの個人情報を収集しないこと

## 📈 今後の拡張案

1. **ニュース/ブログセクション**
   - 架空のプレスリリース
   - 「事故」や「異常」のニュース

2. **採用情報ページ**
   - 求人情報（怪しい条件）
   - 適性テスト

3. **研究実績ページ**
   - 過去のプロジェクト紹介
   - 論文リスト（一部黒塗り）

4. **FAQ/よくある質問**
   - 怪しい質問と回答

5. **スタッフ紹介**
   - 架空の研究員プロフィール
   - 謎めいた経歴

## 📄 ライセンス

このコードはARG「海蝕機関」プロジェクトの一部として提供されています。

---

**最終更新**: 2026年1月29日
**バージョン**: 2.0.0
