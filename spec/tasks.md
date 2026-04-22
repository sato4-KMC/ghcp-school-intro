# Tasks

### [DESIGN] - Implementation Breakdown - 2026-04-22T00:00:00Z
**Objective**: 要求と設計に基づいて、追跡可能な実装タスクを定義する。
**Context**: [spec/requirements.md](spec/requirements.md) と [spec/design.md](spec/design.md) を作成済み。既存の [plan-gmail.md](plan-gmail.md) に実装順序の原案がある。
**Decision**: 高信頼タスクとして、依存の少ない骨格から順に進める段階的タスクへ分割する。
**Execution**: UI 骨格、データ層、状態管理、描画、イベント、検索、検証、ドキュメント更新に分解した。
**Output**: 実装順序、期待成果、依存関係、完了条件を含むタスクリストを定義した。
**Validation**: 各タスクが前提条件と成果物を持ち、個別に完了判定できることを確認した。
**Next**: 実装時に進捗へ応じてステータスを更新する。

## Status Legend

- [ ] 未着手
- [~] 進行中
- [x] 完了

## Implementation Tasks

### Phase 1: Analyze

- [x] 既存ファイルと [plan-gmail.md](plan-gmail.md) を確認する
  - Expected Outcome: 既存資産、制約、主要画面、機能一覧を把握できている
  - Dependencies: なし

- [x] requirements/design/tasks を作成する
  - Expected Outcome: spec 一式が作成され、実装の基準が定義されている
  - Dependencies: 既存プランの読解

### Phase 2: Implement Foundation

- [ ] [index.html](index.html) にアプリシェルを実装する
  - Expected Outcome: ヘッダー、サイドバー、ツールバー、一覧領域、詳細領域、Compose マウントポイントが存在する
  - Dependencies: spec 文書

- [ ] [css/base.css](css/base.css) に基本スタイルと変数を定義する
  - Expected Outcome: リセット、色、タイポ、共通要素の基礎が整う
  - Dependencies: index.html

- [ ] [css/layout.css](css/layout.css) にデスクトップレイアウトを定義する
  - Expected Outcome: Gmail 風の 3 領域構成がレイアウトされる
  - Dependencies: index.html, base.css

- [ ] [css/components.css](css/components.css) にメール行、バッジ、モーダルなどのコンポーネントスタイルを定義する
  - Expected Outcome: 各 UI パーツが一貫した見た目で表示される
  - Dependencies: layout.css

### Phase 3: Implement Data And State

- [ ] [js/data.js](js/data.js) にサンプルメールデータと下書き永続化処理を実装する
  - Expected Outcome: 初期データ生成、draft load/save/clear が利用できる
  - Dependencies: spec/design

- [ ] [js/state.js](js/state.js) にアプリ状態管理を実装する
  - Expected Outcome: route、mails、pagination、compose を更新できる
  - Dependencies: data.js

### Phase 4: Implement Rendering

- [ ] [js/render.js](js/render.js) に一覧、詳細、Compose の描画処理を実装する
  - Expected Outcome: 現在状態から UI が組み立てられる
  - Dependencies: state.js, index.html

- [ ] 受信トレイ、送信済み、ゴミ箱、検索結果の一覧描画を実装する
  - Expected Outcome: mailbox と検索条件に応じたリストが表示される
  - Dependencies: render.js

- [ ] メール詳細ビューを実装する
  - Expected Outcome: 件名、本文、宛先、送信者、操作が表示される
  - Dependencies: render.js

### Phase 5: Implement Interaction

- [ ] [js/events.js](js/events.js) に route change と UI 操作イベントを実装する
  - Expected Outcome: クリック、入力、送信、検索、ページ送りが機能する
  - Dependencies: state.js, render.js

- [ ] 一括選択、スター切替、既読切替、削除を実装する
  - Expected Outcome: 一覧および詳細から状態変更できる
  - Dependencies: events.js

- [ ] Compose の返信、転送、最小化、全画面、閉じるを実装する
  - Expected Outcome: Compose の表示状態と事前入力が正しく動く
  - Dependencies: events.js

- [ ] 検索とハイライトを実装する
  - Expected Outcome: 通常検索、`from:`、`subject:` が動作する
  - Dependencies: events.js, render.js

- [ ] ページネーションと前後メール移動を実装する
  - Expected Outcome: 50 件単位の移動と詳細前後移動が可能になる
  - Dependencies: events.js, render.js

### Phase 6: Validate

- [ ] 主要導線を手動検証する
  - Expected Outcome: 初期表示、遷移、検索、送信、削除、下書き復元が確認できる
  - Dependencies: 全機能実装

- [ ] 静的チェックを実行する
  - Expected Outcome: JavaScript の構文エラーや明白な欠陥が検出されない
  - Dependencies: 全機能実装

- [ ] エッジケースを確認する
  - Expected Outcome: 不正 route、空結果、宛先未入力、破損 draft に安全対応できる
  - Dependencies: 全機能実装

### Phase 7: Reflect And Handoff

- [ ] README または関連文書を必要に応じて更新する
  - Expected Outcome: 実装内容と起動確認方法が現状に合う
  - Dependencies: 実装完了

- [ ] 技術的負債と残課題を整理する
  - Expected Outcome: 未対応事項が明示される
  - Dependencies: 検証完了

- [ ] 最終要約を準備する
  - Expected Outcome: 変更点、検証結果、残リスクを簡潔に報告できる
  - Dependencies: 全工程完了