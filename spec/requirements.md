# Requirements

### [ANALYZE] - Spec Definition - 2026-04-22T00:00:00Z
**Objective**: Gmail 風の静的 UI プロトタイプに対する実装要件を、テスト可能な形で定義する。
**Context**: 既存の [plan-gmail.md](plan-gmail.md) に画面構成、データモデル、機能一覧、実装順序、技術要件が記載されている。現時点では spec 配下の文書は未作成である。
**Decision**: [plan-gmail.md](plan-gmail.md) を一次情報源として requirements/design/tasks を新規作成し、曖昧さが残る箇所は静的サイトとして破綻しない最小限の前提で補完する。
**Execution**: [plan-gmail.md](plan-gmail.md) を読んで画面、状態、ルーティング、操作対象を整理し、EARS 記法の要求へ変換した。
**Output**: 受信トレイ、詳細、検索、送信済み、ゴミ箱、Compose、状態管理、永続化、表示制約、エラー条件を含む要求セットを定義した。
**Validation**: 各要求がトリガー条件と期待動作を持ち、手動検証または軽量な自動確認に落とせる構造であることを確認した。
**Next**: [spec/design.md](spec/design.md) に技術設計と検証方針を定義する。

## Scope Summary

- 対象は HTML / CSS / JavaScript のみで構成する静的な Gmail 風 UI プロトタイプ
- ルーティングは Hash ベース
- データは基本的に In-memory 管理
- 下書きのみ localStorage で保持
- デスクトップ表示を前提とし、最小幅 1024px を想定

## File Inventory

- [plan-gmail.md](plan-gmail.md): 画面一覧、レイアウト、データモデル、機能一覧、技術要件
- [README.md](README.md): リポジトリ概要
- [css/](css/): スタイル格納先
- [js/](js/): スクリプト格納先

## Dependencies And Constraints

### Dependency Graph

- HTML シェル
- CSS レイアウトとコンポーネント定義
- JavaScript データ層
- JavaScript 状態管理
- JavaScript 描画
- JavaScript イベント処理
- ブラウザ標準 API
  - `location.hash`
  - `localStorage`
  - DOM API

### Constraints

- フレームワークは導入しない
- 既存ディレクトリ構成を維持する
- レスポンシブ対応は必須ではない
- サーバーサイド機能は持たない

## Data Flow Summary

1. 初期化時にサンプルメールデータと下書きデータを読み込む
2. 現在の `location.hash` を解析し、表示ビューを決定する
3. 状態に応じてメール一覧または詳細を描画する
4. ユーザー操作に応じて状態を更新し、該当ビューを再描画する
5. Compose 操作時は下書きを保存または送信済みに反映する

## Edge Case Matrix

| ケース | 条件 | 期待動作 |
|---|---|---|
| 空の受信トレイ | inbox ラベルのメールが 0 件 | 空状態メッセージを表示する |
| 不正なメール ID | `#mail/:id` が存在しない ID を指す | エラー表示または一覧へ戻す |
| 検索結果 0 件 | 条件に一致するメールが存在しない | 0 件表示と検索語の表示を行う |
| 下書き破損 | localStorage の JSON が不正 | 下書きを破棄し初期状態へ戻す |
| 宛先なし送信 | To が空で送信操作された | 送信せず入力エラーを表示する |
| ページ範囲外移動 | 最初または最後のページでさらに移動 | 状態を維持し無効化表示する |
| 前後移動不可 | 詳細表示中に前後対象がない | 該当ボタンを無効化する |

## Confidence Assessment

- Confidence Score: 88%
- Rationale: 画面、技術制約、主要操作、データモデルが [plan-gmail.md](plan-gmail.md) に十分明示されているため、高信頼でフル実装計画へ進める。未確定事項は配色や細部 UI 文言などの表層仕様に限定される。

## Requirements In EARS Notation

### Core Application

1. THE SYSTEM SHALL provide a single-page mail client prototype using only HTML, CSS, and JavaScript.
2. WHEN the application is loaded, THE SYSTEM SHALL render the default inbox view.
3. WHEN the viewport width is 1024px 以上, THE SYSTEM SHALL display the desktop mail layout with header, sidebar, toolbar, and content area.
4. WHILE the application is running, THE SYSTEM SHALL manage mail data in memory except for draft persistence.

### Routing And Navigation

5. WHEN the hash route is `#inbox`, THE SYSTEM SHALL display inbox mails.
6. WHEN the hash route is `#sent`, THE SYSTEM SHALL display sent mails.
7. WHEN the hash route is `#trash`, THE SYSTEM SHALL display trashed mails.
8. WHEN the hash route is `#search?q=...`, THE SYSTEM SHALL display mails matching the search query.
9. WHEN the hash route is `#mail/:id`, THE SYSTEM SHALL display the full details of the targeted mail.
10. IF the hash route is unknown or invalid, THEN THE SYSTEM SHALL fall back to a safe default view.

### Inbox List

11. WHEN inbox mails are shown, THE SYSTEM SHALL render each mail row with sender, subject, preview, date, read state, selection state, and star state.
12. WHEN a user clicks a mail row, THE SYSTEM SHALL navigate to the corresponding detail view.
13. WHEN a user toggles a row checkbox, THE SYSTEM SHALL update the selection state for that mail.
14. WHEN a user toggles the star control, THE SYSTEM SHALL update the starred state without breaking the current view.
15. WHEN a user toggles the read control, THE SYSTEM SHALL update the read state and reflected styling.
16. WHEN a user performs delete on one or more selected mails, THE SYSTEM SHALL move those mails to trash.
17. WHEN a user performs bulk selection, THE SYSTEM SHALL apply the requested selection rule to the currently visible list.
18. WHEN more than 50 mails exist in the current list, THE SYSTEM SHALL paginate the list in groups of 50.

### Mail Detail

19. WHEN a mail detail view is opened, THE SYSTEM SHALL show the full subject, sender, recipients, timestamp, and body.
20. WHEN a user selects reply from detail view, THE SYSTEM SHALL open Compose with the original sender prefilled as recipient and contextual subject/body values.
21. WHEN a user selects forward from detail view, THE SYSTEM SHALL open Compose with forwarded subject/body values.
22. WHEN a user deletes a mail from detail view, THE SYSTEM SHALL move the mail to trash and leave the detail route safely.
23. WHEN a user toggles star in detail view, THE SYSTEM SHALL persist the new star state in the shared mail state.
24. WHEN a user moves to previous or next mail from detail view, THE SYSTEM SHALL navigate within the current filtered list context.

### Compose

25. WHEN a user clicks compose, THE SYSTEM SHALL open a compose modal anchored to the interface.
26. WHEN the compose modal is open, THE SYSTEM SHALL provide To, CC, BCC, subject, and body inputs.
27. WHEN a user minimizes, expands, or closes Compose, THE SYSTEM SHALL update the modal presentation without losing current draft content.
28. WHILE a user is editing a draft, THE SYSTEM SHALL periodically or eventfully save draft content to localStorage.
29. WHEN a user sends a valid message, THE SYSTEM SHALL append the message to the sent mailbox data and clear the current draft state.
30. IF a user attempts to send without a primary recipient, THEN THE SYSTEM SHALL block sending and show an error state.

### Search

31. WHEN a user submits text in the search bar, THE SYSTEM SHALL navigate to the search route and render matching results.
32. WHEN a search query includes `from:` prefix, THE SYSTEM SHALL filter by sender fields.
33. WHEN a search query includes `subject:` prefix, THE SYSTEM SHALL filter by subject text.
34. WHEN a query matches visible text content, THE SYSTEM SHALL highlight matched fragments in the search result presentation.
35. IF no mails match the query, THEN THE SYSTEM SHALL render an empty result state.

### Sidebar And Counts

36. WHEN the sidebar is shown, THE SYSTEM SHALL provide navigation links for inbox, sent, trash, and drafts.
37. WHEN unread mails exist in a mailbox, THE SYSTEM SHALL display an unread count badge where relevant.
38. WHEN a sidebar destination is selected, THE SYSTEM SHALL update the active navigation state visually.

### Persistence And Recovery

39. WHEN the application starts, THE SYSTEM SHALL attempt to restore any saved draft from localStorage.
40. IF restoring draft data fails due to invalid or missing localStorage content, THEN THE SYSTEM SHALL continue with an empty draft state.

### Non-Functional

41. THE SYSTEM SHALL keep the codebase split into HTML, CSS, and JavaScript files under the existing repository structure.
42. THE SYSTEM SHALL preserve readable, minimal, and framework-free implementation patterns suitable for a small static site.
43. THE SYSTEM SHALL keep user interactions responsive using client-side state updates without page reloads.