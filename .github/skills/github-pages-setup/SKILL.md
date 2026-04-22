---
name: github-pages-setup
description: 'GitHub Pages の構築、公開設定、GitHub Actions によるデプロイ、カスタムドメイン設定を進めるときに使う Skill。Use when setting up GitHub Pages for a static site, choosing a source branch or Actions workflow, checking base path and asset paths, or verifying deployment settings.'
argument-hint: '対象のサイト構成や困りごとを指定する。例: static site を Pages で公開したい'
user-invocable: true
---

# GitHub Pages Setup

GitHub Pages を使って静的サイトを公開するときの進め方をまとめた Skill です。

## When to Use

- GitHub Pages の初期設定をしたい
- ブランチ公開と GitHub Actions 公開のどちらを使うか決めたい
- 静的サイトの base path やアセット参照を確認したい
- カスタムドメインや公開後確認の手順を整理したい

## Procedure

1. リポジトリが単純な静的サイトか、ビルド成果物を生成するサイトかを確認する。
2. 公開方式を決める。
   - 単純な静的ファイルをそのまま出すなら branch/folder 公開を優先する。
   - ビルドや前処理が必要なら GitHub Actions によるデプロイを優先する。
3. リポジトリ名と公開 URL を確認し、相対パスか base path の調整が必要か判断する。
   - Project Pages では `/repository-name/` 配下で配信される点に注意する。
   - User/Organization Pages では通常ルート `/` 配下で配信される。
4. 公開設定を行う。
   - Branch 公開なら Settings > Pages で source branch と folder を設定する。
   - Actions 公開なら公式の Pages Actions 手順に沿って workflow を作る。
5. 公開後に確認する。
   - 生成された URL にアクセスする。
   - CSS, JS, 画像のパス切れがないかを見る。
   - 404 ページや README の案内が必要なら整える。
6. カスタムドメインが必要なら DNS 設定と Pages 設定を追加する。

## Output Expectations

- 必要最小限の Pages 設定で公開できること
- リポジトリ構成に応じて branch 公開か Actions 公開を選べること
- アセットパスや公開 URL の落とし穴を事前に潰せること

## References

- [公式ドキュメント一覧](./references/official-docs.md)
