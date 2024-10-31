# Snakemakeをモジュール化する

このプロジェクトは、Snakemakeを使用してデータ処理パイプラインを実行するためのものです。各ステップはモジュールとして定義されており、最終的な結果を生成します。

また、途中の出力ファイルが存在していた場合も、そこから再開されることを確認できます。

## ディレクトリ構造

```
.
├── config.yaml
├── data
│   ├── sample1_final_result.txt
│   ├── sample1_step2_result.txt
│   ├── sample2_final_result.txt
│   ├── sample2_step2_result.txt
├── modules
│   ├── module1
│   │   └── Snakefile
│   ├── module2
│   │   └── Snakefile
│   └── module3
│       └── Snakefile
└── Snakefile
```

## 設定ファイル

`config.yaml` ファイルには、サンプルのリストが含まれています。

```
samples:
  - sample1
  - sample2
```

## ワークフローの実行

1. Snakemakeがインストールされていることを確認します。
2. プロジェクトのルートディレクトリで以下のコマンドを実行します。

```
snakemake --cores 1
```

## モジュールの説明

### Module 1

`modules/module1/Snakefile` には、ステップ1のルールが定義されています。

```
rule step1:
    output:
        "data/{sample}_step1_result.txt",
    shell:
        "echo 'Step 1 complete for {wildcards.sample}' > {output}"
```

### Module 2

`modules/module2/Snakefile` には、ステップ2のルールが定義されています。

```
rule step2:
    input:
        "data/{sample}_step1_result.txt",
    output:
        "data/{sample}_step2_result.txt",
    shell:
        "echo 'Step 2 complete for {wildcards.sample}' > {output}"
```

### Module 3

`modules/module3/Snakefile` には、最終ステップのルールが定義されています。

```
rule final_step:
    input:
        "data/{sample}_step2_result.txt",
    output:
        "data/{sample}_final_result.txt",
    shell:
        "echo 'Final result for {wildcards.sample}' > {output}"
```

## 全体のルール

`Snakefile` には、全体のルールが定義されています。

```
rule all:
    input:
        expand("data/{sample}_final_result.txt", sample=config.get("samples")),
```

このルールは、すべてのサンプルに対して最終結果ファイルが生成されることを確認します。
