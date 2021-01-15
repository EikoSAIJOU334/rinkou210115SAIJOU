# Nextflowを使ってみる



### 参考にしたページ

- #### Nextflow公式

https://www.nextflow.io/

- #### ゼロから触ってみるNextflow

https://qiita.com/Ken-Kuroki/items/2d67cdc368c9ebaa7198

- #### Nextflowを使ってみる

https://qiita.com/manabuishiirb/items/7dfe4dda23043ea2addb

- #### NextflowからCWLで書かれたワークフローを呼び出す

https://qiita.com/yuifu/items/3699d89152f2e39fdc26#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%E8%A8%98%E8%BF%B0%E8%A8%80%E8%AA%9Ecwlnextflow



## Nextflowとは

> 「NextflowはバルセロナのCentre for Genomic Regulationによってオープンソースで開発されている科学計算用ワークフローエンジンです。GitHubのスター数はおよそ1000、現在かなり活発に開発が進められています。Java上で走るGroovyで書かれていて、可搬性、再現性、スケーラビリティなどを売りにしています。」[Ken-KurokiさんのQiita](https://qiita.com/Ken-Kuroki/items/2d67cdc368c9ebaa7198)より

　バイオインフォ解析で、環境 + 手順を固めて提供したいときに使える便利なワークフローエンジン。[公式ページ](https://www.nextflow.io/)では、環境をDocker, Singularity または[Podman](https://podman.io/)で提供することが推奨されている。ゆえに、面倒なツール群のインストールに悩むことなく、個々の解析環境の差に影響されることもなく、再現性の高い解析を行う事ができる。

　Nextflowを動かす際のパイプラインのインストールは不要で、パイプライン名が指定されていればnextflowがGithubから自動的に取得する。（ただしインターネット接続が必要。）



## ワークフロー言語とは

ワークフローとは、複数のソフトウエアを順次実行する過程のこと。

> 「特にNGS解析では、複数のツールを順次データが流れていくので、ワークフローの正確な記述とそれに基づく実行が、データの再現性確保の技術的要件となっている。

> ワークフローは素課程（ステップ）の連なりとして定義される。各ステップは入出力（ファイル）とソフトウェア実行スクリプト（ソフトウェア、バージョン、実行オプション）から構成され、ステップの間をデータが流れていくイメージである。このようなワークフローを記述するのがワークフロー記述言語（Workflow language）である。」[yuifuさんのQiita](https://qiita.com/yuifu/items/3699d89152f2e39fdc26#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC%E8%A8%98%E8%BF%B0%E8%A8%80%E8%AA%9Ecwlnextflow)より

ワークフロー記述言語には、ざっくり言うと、ドメイン固有言語（DSL）型とマークアップ言語（ML）型がある（下表）。



|                    | DSL型                                    | ML型                                     |
| ------------------ | ---------------------------------------- | ---------------------------------------- |
| 長所               | 柔軟な記述が可能（例：ループ、条件分岐） | 機械可読、他の記述形式との変換が容易     |
| 短所               | 独自の文法を習得する必要がある           | 複雑な処理（〜ループとか）を書くのが苦手 |
| 向いていそうな用途 | 人数が限られたチーム内                   | 大人数での開発                           |
| 例                 | **Nextflow**, Snakemake                  | Galaxy, Common Workflow Language (CWL)   |



## Nextflowの例

現在、主にバイオインフォ用途の多くのパイプラインが登録されている。

- nf-core/hic (Analysis of Chromosome Conformation Capture data (Hi-C))

- nf-core/chipseq (ChIP-seq peak-calling, QC and differential analysis pipeline.)

- nf-core/scrnaseq (A single-cell RNAseq pipeline for 10X genomics data)

- nf-core/nanoseq (Nanopore demultiplexing, QC and alignment pipeline)

- nf-core/rna-seq (RNA sequencing analysis pipeline using STAR, RSEM, HISAT2 or Salmon with gene/isoform counts and extensive quality control.)

- nf-core/ampliseq (16S rRNA amplicon sequencing analysis workflow using QIIME2)

  などなど、、、

[nf-coreリポジトリ](https://nf-co.re/pipelines)に登録されていないワークフローも、githubに[awesome-nextflow](https://github.com/nextflow-io/awesome-nextflow/)としてまとめられている。covid-19関連だけでも6つ入っていた (20210113現在) 。

## Nextflowを用いる利点

以下の利点が考えられる。

- 同じツール、同じ手順で解析を行うことにより、実験の再現性が高まる。

- バイオインフォの専門性が不足した研究者でも、少ない手順で解析を行えるため誤った結論に至りづらくなる。

- 研究室内のサーバーで動かすことを前提に組まれたパイプラインを、広く世界に共有することができる。

公式サイトにテンプレートが公開されているので、こちらを用いれば（専門性がある人ならば）自分のパイプラインを固めて公開するハードルは低い。らしい。



## Nextflowのインストール

[公式サイト](https://nf-co.re/usage/installation)より。

まず環境を確認する。Java 8以上が必要なので、

`java -version`で確認する。

インストールはシンプルで、

`curl -s https://get.nextflow.io | bash`

または、`wget -qO- https://get.nextflow.io | bash`

または、[bioconda](https://bioconda.github.io/recipes/nextflow/README.html)から`conda install nextflow`でインストールできる。

- テスト

`nextflow run hello`と入力し、Hello worldが動けばOK。こちらは、実際にはnextflowのgithubから自動的にhelloというスクリプトが取得されて起動している。

![スクリーンショット 2021-01-13 11.58.47](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 11.58.47.png)

## 公式 (Get started) に沿って動かしてみる

まずは、`turorial.nf`というスクリプトを作って保存する。

```nextflow
#!/usr/bin/env nextflow

params.str = 'Hello world!'

process splitLetters {

    output:
    file 'chunk_*' into letters

    """
    printf '${params.str}' | split -b 6 - chunk_
    """
}


process convertToUpper {

    input:
    file x from letters.flatten()

    output:
    stdout result

    """
    cat $x | tr '[a-z]' '[A-Z]'
    """
}

result.view { it.trim() }
```

このスクリプトは２つのプロセス（splitLettersとconvertToUpper）で構成されている。

splitLettersは、文字列を 6 文字ずつの固まりに分割し、それぞれを chunk_ という接頭辞を持つファイルに書き出す。

convertToUpperは、これらのファイルを受け取って、内容を大文字に変換する。

結果として得られた文字列は`result`チャンネルに出力され、最終的な出力は`View`演算子によって出力される。

さっそくターミナルに入力して動かしてみる。

```bash
nextflow run tutorial.nf
```

出力は以下のようになる。

![スクリーンショット 2021-01-13 12.42.07](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 12.42.07.png)

最初のプロセスは1回、次のプロセスは２回実行されていることがわかる。最終的に結果の文字列が出力される。

convertToUpperプロセスは並行して実行されるので、Helloの固まりとworldの固まりは異なる順序で出力される場合もあり。



## 改変してみる

Nextflowは、パイプライン内で行われたすべてのプロセスを追跡して、保持する。もしスクリプトの一部を改変した場合は、実際に変化した部分だけが実行される。変更されていない部分はスキップされ、代わりにキャッシュされた結果が使われる。

なので、パイプラインを改変してテストするときに、待ち時間が短くすむようになっている。

では、convertToUpper部分を改変してみる。

```
process convertToUpper {

    input:
    file x from letters

    output:
    stdout result

    """
    rev $x
    """
}
```

同じ名前で保存し、`-resume`をつけて実行してみる。

`nextflow run tutorial.nf -resume`

以下のように実行される。

![スクリーンショット 2021-01-13 13.38.00](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 13.38.00.png)

プロセス`splitletters`はスキップされる。（16進数で表されるIDが同一）結果はキャッシュから取得されている。２つ目のプロセスは期待通り実行されて、文字列が逆向きで表示される。

`$PWD/work`にキャッシュが保存され続けるので、ディスク容量の圧迫を避けるために時々クリーンナップをする必要がある。

## 仮引数の設定

パイプラインの仮引数の設定は、変数名の前に接頭語`param.`をつけて宣言する。

コマンドラインでは、仮引数の名前の前に--をつけて仮引数の値を設定することができる。

`--paramName`のように使用。

さっそくこの変数に別の入力を与えてみる。

`nextflow run tutorial.nf --str 'BonJour le monde'`

この場合、Hello world!のかわりに、BonJour le mondeが６文字ずつ区切られて逆向きに表示される。

![スクリーンショット 2021-01-13 14.25.53](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 14.25.53.png)

このように、param.strの内容は、--strで与える事ができる。



## ramdaqを動かしてみる

実際に、バイオインフォのパイプラインをユーザーとして使う場合について。

[ramdaqのgithub](https://github.com/rikenbit/ramdaq)を参考に行う。

1. #### Nextflowのインストール

   （上記）

2. #### DockerおよびSinguralityのインストール

   （これも略、中戸さんのおかげでみんな入っているはず。感謝。）

3. #### パイプラインを自動でインストールし、最小限のデータセットでテスト

   ```bash
   nextflow run rikenbit/ramdaq -profile test,<docker/singularity>
   ```

   ![image-20210113155916824](/Users/saijoueiko/Library/Application Support/typora-user-images/image-20210113155916824.png)

   うまく動かないときは、nf-core/configをいじる必要がある。

4. #### 自分のデータ解析を走らせよう！

   （ヒト、マウスなら、レファレンスデータはダウンロードせずとも使用できる）

   ```bash
   nextflow run rikenbit/ramdaq -profile <docker/singularity> --reads '*_R{1,2}.fastq.gz' --genome GRCh38
   ```

5. #### レファレンスデータは、ローカルパスを記述すればそちらも使用できる。--local_annot_dirオプション

   ```bash
   nextflow run rikenbit/ramdaq -profile <docker/singularity> --reads '*_R{1,2}.fastq.gz' --genome GRCh38 --local_annot_dir <The directory path where the reference genome and annotations are placed>
   ```

6. #### 実際に解析する際は、BCLファイルをfastqに変換する[ramdaq_bcl2fastq](https://github.com/rikenbit/ramdaq_bcl2fastq)を通してから行う。こちらも**Nextflow**によって提供されている。

7. #### マウス、シングルエンド、アンストランデッドのテストデータでも試してみる。

   ```bash
   nextflow run rikenbit/ramdaq -profile test_SE_UST_M,docker --outdir results_DLtest_SE_UST_M
   ```

   上記コマンドを実行したところ、ローカルに、results_DLtest_SE_UST_Mディレクトリが生成された。

## ramdaqの結果

![スクリーンショット 2021-01-13 16.24.27](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 16.24.27.png)

こちらの**pipeline_info**を見ると、

![スクリーンショット 2021-01-13 16.26.01](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 16.26.01.png)

さまざまなレポートが出力されている。

[execution_report.html](file:///Users/saijoueiko/results_DLtest_SE_UST_M/pipeline_info/execution_report.html)

[execution_timeline.html](file:///Users/saijoueiko/results_DLtest_SE_UST_M/pipeline_info/execution_timeline.html)

[pipeline_report.html](file:///Users/saijoueiko/results_DLtest_SE_UST_M/pipeline_info/pipeline_report.html)

[results_description.html](file:///Users/saijoueiko/results_DLtest_SE_UST_M/pipeline_info/results_description.html)

**featureCount**には

![スクリーンショット 2021-01-13 16.31.17](/Users/saijoueiko/Desktop/スクリーンショット 2021-01-13 16.31.17.png)

TPMなどの情報が出力される。

ここまで生成できれば下流の解析に持っていけるだろう。

## まとめ

Nextflow, Dockerをインストールすれば、バイオインフォに明るくない研究者でもデータ解析を始めることができるので、利用する側としては大変ありがたいツールだと感じた。解析を提供する側としても、Nextflowで固めて公共のレポジトリに置くことで、解析の信頼性・再現性が高まるので（Groovyの記述法を学ぶ必要はあるが）導入する価値があるのではないかな。

余談：DockerよりもSinguralityよりも新しい、Podmanというコンテナがあることを今回初めて知った。今後普及するかも知れないので、詳細を調べようと思った。



## オマケ：引数一覧

#### Main arguments

-profile
-c
--reads
--single_end
--stranded

#### Reference genomes and annotations

--genome
--saveReference
--local_annot_dir

#### Other command line parameters

--outdir
-name
-resume
--max_memory
--max_time
--max_cpus
--monochrome_logs

#### Parameters for each tools

個々のプロセスに対応したパラメータもあります。

Fastqmcf
Hisat2
featureCounts

#### Results report options

--sampleLevel