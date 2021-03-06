<!--
title:   GitHub Actionsを用いて定期的にIssueを作成する方法
tags:    GitHub,GitHubActions,automation
id:      34435e9cb3fd76e7e86f
private: false
-->
## これは何
+ [GitHub Actions](https://docs.github.com/ja/actions)でIssueを定期的に作成するためのYMLファイルの定義を記載する
    + 定期的に手動でIssueを作成する場合など少しだけ手間を減らすことが出来る :muscle: 

### 使用するActions

https://github.com/actions-ecosystem/action-create-Issue

## 設定
+ GitHubレポジトリはすでに作成している前提で記載します。

### ディレクトリの作成
+ GitHub Actionsの設定ファイルは `.github` フォルダ以下に記載します。

```bash
cd path/to/repository
mkdir -p .github/workflows
```

### YMLファイルの作成

```bash
touch .github/workflows/create_issue.yml
```

### 設定を記述

+ 今回は月曜日の09:00(JST)に定期的にIssueを作るように設定してみる
    + GitHub Actionsのcron定義は[こちら](https://docs.github.com/ja/actions/reference/events-that-trigger-workflows#scheduled-events)を確認ください :pray: 

```yml
name: Create an issue on Monday 09:00 (JST)

on:
  schedule:
    - cron: '0 0 * * 1'

jobs:
  create_issues:
    runs-on: ubuntu-latest
    steps:
      - name: Get today and tomorrow date
        id: date
        run: |
          echo "::set-output name=tomorrow::$(date "+%Y/%m/%d" -d "1 day")"
      - name: Create an issue
        uses: actions-ecosystem/action-create-issue@v1
        with:
          github_token: ${{ secrets.github_token }}
          title: '【${{ steps.date.outputs.tomorrow }}】定期タスク'
          body: |
            ## To do
            + [ ] 準備
            
          labels: |
            Routine
```

記述がおわったらmaster(main)ブランチにcommit & pushしておきましょう。

## References

https://docs.github.com/ja/actions