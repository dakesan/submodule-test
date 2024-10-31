# Snakemakeを途中から再開する

## 概要

`test1` プロジェクトは、中間ルールの入力ファイルが存在していた場合、それ以前のruleを無視してsnakemakeが実行されるのかというテストです。

## 結果

Snakefileは3つのruleを定義していますが、`step1_result.txt`または`step2_result.txt`が存在していると、その途中からSnakemakeが実行されるのを確認できます。