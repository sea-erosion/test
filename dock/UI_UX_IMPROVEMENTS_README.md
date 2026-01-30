# UI/UX改善機能 - 実装ガイド

## 📋 概要

このパッケージには、ARG「海蝕機関」のUI/UXを大幅に改善する3つの主要機能が含まれています：

1. **プログレストラッカー** - 進捗管理とバッジシステム
2. **ブックマーク機能** - お気に入り管理と共有
3. **テーママネージャー** - カスタマイズ可能なダークモード

## 🎯 含まれるファイル

### JavaScriptモジュール
- `progress_tracker.js` - プログレス追跡システム
- `bookmark_manager.js` - ブックマーク管理システム
- `theme_manager.js` - テーマ設定システム

### HTMLファイル
- `dashboard.html` - 統合ダッシュボードページ

### CSSファイル
- `ui_improvements.css` - 全機能の統合スタイルシート

## 🚀 クイックスタート

### 1. ファイルの配置

```bash
# JavaScriptファイル
cp progress_tracker.js /internal/js/
cp bookmark_manager.js /internal/js/
cp theme_manager.js /internal/js/

# CSSファイル
cp ui_improvements.css /internal/css/

# HTMLファイル
cp dashboard.html /internal/
```

### 2. 既存ページへの統合

#### すべてのページに追加（header内）:

```html
<!-- UI Improvements CSS -->
<link rel="stylesheet" href="./css/ui_improvements.css">
```

#### main.jsに追加:

```javascript
import { initProgressTracker } from './progress_tracker.js';
import { initBookmarkManager } from './bookmark_manager.js';
import { initThemeManager } from './theme_manager.js';

document.addEventListener('DOMContentLoaded', async () => {
  // 既存の初期化コード...
  
  // UI/UX機能の初期化
  const progressTracker = initProgressTracker();
  const bookmarkManager = initBookmarkManager();
  const themeManager = initThemeManager();
  
  // グローバルに公開
  window.progressTracker = progressTracker;
  window.bookmarkManager = bookmarkManager;
  window.themeManager = themeManager;
});
```

### 3. 日報詳細ページへのブックマークボタン追加

```html
<!-- log-detail.htmlに追加 -->
<button class="bookmark-btn" id="bookmarkBtn" onclick="toggleBookmark()">
  🔖
</button>

<script>
  function toggleBookmark() {
    const logId = new URLSearchParams(window.location.search).get('id');
    // ログデータを取得
    fetch('./deta/logs.json')
      .then(res => res.json())
      .then(logs => {
        const log = logs.find(l => l.id === logId);
        if (log) {
          const isBookmarked = bookmarkManager.addLogBookmark(log);
          updateBookmarkButton(isBookmarked);
        }
      });
  }
  
  function updateBookmarkButton(isBookmarked) {
    const btn = document.getElementById('bookmarkBtn');
    btn.classList.toggle('bookmarked', isBookmarked);
    btn.textContent = isBookmarked ? '❤️' : '🔖';
  }
</script>
```

## 📊 機能詳細

### 1. プログレストラッカー

#### 主な機能
- **閲覧履歴の自動記録**: 日報・日記を読むと自動的に記録
- **達成度の表示**: 全体の進捗をパーセンテージで表示
- **バッジシステム**: 12種類のバッジ獲得
- **統計情報**: 検索回数、部門訪問数など

#### バッジ一覧

| バッジID | 名前 | 条件 |
|---------|------|------|
| `first_log` | 初めての記録 | 最初の日報を読んだ |
| `explorer` | 探索者 | 5つの日報を読んだ |
| `researcher` | 調査員 | 10の日報を読んだ |
| `completionist` | コンプリート | 全ての日報を読んだ |
| `diary_reader` | 日記読者 | 5つの日記を読んだ |
| `all_diaries` | 全日記制覇 | 全ての日記を読んだ |
| `search_master` | 検索の達人 | 10回検索実行 |
| `convergence_specialist` | 収束専門家 | 収束部門を5回訪問 |
| `all_divisions` | 全部門訪問 | 全部門を訪問 |
| `week_veteran` | 1週間の海蝕員 | 7日経過 |
| `dedicated` | 献身的な海蝕員 | 30日経過 |
| `truth_seeker` | 真実の探求者 | 全コンテンツ閲覧 |

#### 使用例

```javascript
// 日報を既読としてマーク
progressTracker.markLogAsRead('2024-08-13-port');

// 日記を既読としてマーク
progressTracker.markDiaryAsRead(1);

// 部門訪問を記録
progressTracker.recordDivisionVisit('収束部門');

// 検索カウントを増加
progressTracker.incrementSearchCount();

// 統計情報を取得
const stats = progressTracker.getAchievementStats();
console.log(stats);
// {
//   logsRead: 5,
//   diariesRead: 3,
//   badgesEarned: 2,
//   searchCount: 10,
//   daysSinceFirstLogin: 7
// }
```

### 2. ブックマーク機能

#### 主な機能
- **簡単なブックマーク**: ワンクリックで保存
- **メモ機能**: 各ブックマークにメモを追加
- **タグ管理**: カスタムタグで整理
- **共有機能**: URLをクリップボードにコピー
- **エクスポート/インポート**: データのバックアップと復元

#### 使用例

```javascript
// 日報をブックマーク
const log = {
  id: '2024-08-13-port',
  title: '港湾部事案',
  date: '2024-08-13',
  division: '港湾部',
  threatLevel: 4
};
bookmarkManager.addLogBookmark(log, '重要な事案');

// タグを追加
bookmarkManager.addTag('log', '2024-08-13-port', '緊急');

// ブックマークされているか確認
const isBookmarked = bookmarkManager.isLogBookmarked('2024-08-13-port');

// 統計情報を取得
const stats = bookmarkManager.getStats();
console.log(stats);
// {
//   totalBookmarks: 10,
//   logsBookmarked: 6,
//   diariesBookmarked: 4,
//   totalTags: 5,
//   mostUsedTags: [...]
// }
```

### 3. テーママネージャー

#### 主な機能
- **3つのモード**: ダーク、ライト、自動（システム連動）
- **明るさ調整**: 50%〜150%の範囲で調整可能
- **アクセントカラー**: カスタムカラーピッカー
- **フォントサイズ**: 80%〜120%の範囲で調整可能
- **5つのプリセット**: デフォルト、オーシャン、クリムゾン、ミッドナイト、ターミナル
- **アニメーション制御**: パフォーマンス向上のためON/OFF可能

#### プリセットテーマ

1. **デフォルト** - 標準の海蝕機関テーマ
2. **オーシャン** - 青系の落ち着いたテーマ
3. **クリムゾン** - 赤系の緊張感あるテーマ
4. **ミッドナイト** - 純黒ベースの最小限テーマ
5. **ターミナル** - GitHub風のテーマ

#### 使用例

```javascript
// モードを設定
themeManager.setMode('dark'); // 'dark', 'light', 'auto'

// 明るさを設定（100が標準）
themeManager.setBrightness(120);

// アクセントカラーを変更
themeManager.setAccentColor('#ff4d4d');

// フォントサイズを変更
themeManager.setFontSize(110);

// プリセットを適用
themeManager.applyPreset('ocean');

// アニメーションを切り替え
themeManager.toggleAnimations();

// システムテーマ連動を切り替え
themeManager.toggleSystemTheme();
```

## 🎨 カスタマイズ

### CSSカスタム変数

テーママネージャーは以下のCSS変数を動的に変更します：

```css
:root {
  --bg-dark: #0a0e1a;
  --bg-card: #111827;
  --text: #e5e7eb;
  --text-muted: #9ca3af;
  --accent: #00ffff;
  --accent-alpha: rgba(0, 255, 255, 0.2);
  --transition-speed: 0.3s;
  --brightness-filter: brightness(1);
}
```

### バッジ通知のカスタマイズ

```javascript
// バッジ獲得時の通知をカスタマイズ
class CustomProgressTracker extends ProgressTracker {
  showBadgeNotification(badgeId) {
    const badge = this.getBadgeDefinitions().find(b => b.id === badgeId);
    
    // カスタム通知実装
    alert(`🎉 バッジ獲得: ${badge.name}`);
    
    // または音声を再生
    const audio = new Audio('/sounds/badge-unlock.mp3');
    audio.play();
  }
}
```

## 📱 レスポンシブ対応

全ての機能はモバイルデバイスに対応しています：

- タッチ操作に最適化されたボタンサイズ
- 縦画面での表示最適化
- スワイプジェスチャー対応（将来実装）

## 🔒 データ管理

### LocalStorageキー

```javascript
'arg_progress'    // プログレストラッカーのデータ
'arg_bookmarks'   // ブックマークのデータ
'arg_theme'       // テーマ設定
```

### データのバックアップ

```javascript
// すべてのデータをエクスポート
progressTracker.exportProgress();
bookmarkManager.exportBookmarks();
themeManager.exportSettings();

// JSONファイルとしてダウンロードされます
```

### データの復元

```javascript
// ファイル選択後、自動的にインポート
<input type="file" onchange="progressTracker.importProgress(event.target.files[0])">
<input type="file" onchange="bookmarkManager.importBookmarks(event.target.files[0])">
<input type="file" onchange="themeManager.importSettings(event.target.files[0])">
```

## 🐛 トラブルシューティング

### バッジが獲得されない

```javascript
// 手動でチェックを実行
progressTracker.checkAchievements();

// デバッグ情報を表示
console.log(progressTracker.progress);
console.log(progressTracker.getAchievementStats());
```

### テーマが適用されない

```javascript
// 手動でテーマを再適用
themeManager.applyTheme();

// CSS変数を確認
console.log(getComputedStyle(document.documentElement).getPropertyValue('--accent'));
```

### ブックマークが表示されない

```javascript
// 手動で表示を更新
bookmarkManager.updateBookmarkDisplay();

// データを確認
console.log(bookmarkManager.bookmarks);
```

## 🎯 パフォーマンス最適化

### アニメーションの無効化

```javascript
// アニメーションを無効にしてパフォーマンス向上
themeManager.settings.animations = false;
themeManager.applyTheme();
```

### データのクリーンアップ

```javascript
// 古いデータを削除（30日以上前のブックマーク）
const thirtyDaysAgo = new Date();
thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);

bookmarkManager.bookmarks.logs = bookmarkManager.bookmarks.logs.filter(b => {
  return new Date(b.addedAt) > thirtyDaysAgo;
});
bookmarkManager.saveBookmarks();
```

## 📈 今後の拡張案

1. **クラウド同期**
   - ユーザーアカウント間でのデータ同期
   - 複数デバイスでの進捗共有

2. **ソーシャル機能**
   - 友達との進捗比較
   - バッジの共有
   - リーダーボード

3. **高度な統計**
   - 読書時間の記録
   - 活動パターンの分析
   - 推奨コンテンツ

4. **通知システム**
   - 新しい日報の通知
   - バッジ獲得のプッシュ通知
   - リマインダー機能

## 🤝 サポート

問題が発生した場合：

1. ブラウザのコンソールでエラーを確認
2. LocalStorageのデータを確認
3. データをエクスポートしてバックアップ
4. 必要に応じてリセット

## 📄 ライセンス

このコードはARG「海蝕機関」プロジェクトの一部として提供されています。

---

**最終更新**: 2026年1月29日
**バージョン**: 1.0.0
