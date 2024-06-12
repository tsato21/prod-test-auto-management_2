# prod-test-auto-management_2 (for Google Apps Script Projects)

## 目次

- [1. 概要](#1-概要)
- [2. セットアップ](#2-セットアップ)
- [3. 使用方法](#3-使用方法)
- [4. 参考文献](#4-参考文献)


## 1 概要

***複数の***Google Apps Scriptプロジェクトのテスト環境と本番環境を一つのリポジトリで管理し、Github Actionsで本番環境に自動デプロイするワークフロー。


## 2 セットアップ

***⭐️ [prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1)と異なる部分のみ、記載。***


- ローカルリポジトリの構成を以下の構成に設定する。([prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1) >`2 セットアップ` > `2`)
    ```
    ./
    ├─ project_1/
    │  └─ src/
    │  │  ├─ appsscript.json
    │  │  └─ Code.gs
    │  └─ .clasp.json
    ├─ project_2/
    │  └─ src/
    │  │  ├─ appsscript.json
    │  │  └─ Code.gs
    │  └─ .clasp.json
    ```


- Repository Secretを設定。([prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1) > `2 セットアップ` > `8`)
  - ***`CLASP_SCRIPT_ID`は、deploy.ymlに直接記載するため、設定不要。それ以外のsecretを設定。***


- `deploy.yml`ファイルで各プロジェクト(***本番環境***)のフォルダ名、IDを以下のように記載。
    ```
    projects:
        - target-dir: 'project_1'
        script-id: 'XXXX'
        - target-dir: 'project_2'
        script-id: 'XXXX'
    ```
    - [prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1)と同様、本番環境のScript IDをGithub ActionsのSecretとして下記のように設定しようとしたが、matrix内では、`secrets`が使用できない ([参考サイト](https://github.com/orgs/community/discussions/26302))。そのため、ファイル内に直で記載。(***ただし、Scripit IDを非公開にする必要がある場合は、当該箇所の修正が必要。***)
    ```
    strategy:
      matrix:
        projects:
          - target-dir: 'project_1'
            script-id: ${{ secrets.PROJECT_1_CLASP_SCRIPT_ID }}
          - target-dir: 'project_2'
            script-id: ${{ secrets.PROJECT_2_CLASP_SCRIPT_ID }}
    ```


## 3 使用方法

***⭐️ [prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1)と異なる部分のみ、記載。(XXX)は当該READMEのセクションを示している。***

- ***全てのプロジェクト***のテスト環境用ファイルを最新の状態にする。([prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1) > `3 使用方法` > `1`)
  - 各プロジェクト内で作業する際は、`cd project_1`のように、当該プロジェクトに移動してから作業。
  - 別プロジェクトや、Githubへのpush作業時は、`cd ..`でrootフォルダに戻る。

- Commit & タグ付け & Pushで、***全てのプロジェクト***の本番環境用ファイルが最新の状態 & Versionが更新される。([prod-test-auto-management_1のREADME](https://github.com/tsato21/prod-test-auto-management_1) > `3 使用方法` > `2`)


## 4 参考文献
- [clasp×githubActionsで複数のgasプロジェクトを一つのリポジトリで管理し、自動デプロイまでできるようにした](https://zenn.dev/furnqse/articles/a138962560db56): 参考サイト
- [Secrets in matrix](https://github.com/orgs/community/discussions/26302): Github Community


## 5 サンプル
- [Apps Script Projects](https://drive.google.com/drive/folders/1OZ6V9nOgBF1jFDjFRo8_jH-HQ0wKKvon)