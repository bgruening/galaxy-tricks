# Galaxy ユーザーのための便利な小ネタと裏技

"お主を取り巻くフォースを感じろ" *マスター・ヨーダ*

- [テキスト処理](#テキスト処理)
- [次世代シークエンシング](#次世代シークエンシング)
- [ワークフロー](#ワークフロー)
- [もっと学びたい方へ](#もっと学びたい方へ)
- [免責条項](#免責条項)

# 翻訳の方針

|英単語|この文書での日本語|他に検討した単語|
| :----: | :----: | :----: |
|column|列|カラム、フィールド|
|comma|コンマ|カンマ|
|all|すべて|全て|
|HTS|次世代シークエンシング|HTS、NGS、ハイスループットシークエンシング|

## テキスト処理
- コンマ区切りファイルをタブ区切りファイルに変換する<br>
   `Convert delimiters to TAB`
- ユニークなシーケンスを持つFASTAファイル<br>
   `FASTA-to-Tabular` → `Unique occurrences of each record` (advanced parameters) → `Tabular-to-FASTA`
- `N` などの文字を含むシーケンスを削除する<br>
  `FASTA-to-Tabular` → `Filter data on any column using simple expressions` with <br>(condition: `c2.find('N') != -1`) → `Tabular-to-FASTA`
- 5列あるファイルから3列目を抽出する<br>
  `Cut columns from a table` で `c3`
- 列の並べ替えまたは列の入れ替え<br>
  `Cut columns from a table` で `c3,c2,c1`
- 列1に、あるエントリーが現れる数を数える<br>
  `Datamash` で `Group by fields`: 1、`Operation to perform`: count とする
- 列1,4,5が同一である行をすべてグループ化する<br>
  `Datamash` で `Group by fields`: 1,4,5
- 列から行へ、行から列へ（転置行列）<br>
  `Transpose rows/columns`
- ファイルサイズを小さくする。例えば、テストのためのファイルのサブサンプリング<br>
  `Select random lines from a file`
- シーケンスファイルサイズを小さくする。例えば、テストのためのシーケンスのサブサンプリング<br>
  `Sub-sample sequences files`
- Merge two files together according to one column in every file<br>
  `Join two files`
- ユニークな列を追加する<br>
  `Add column to an existing dataset` で `iterate`: Yes とする
- 2列目が0よりも大きな値である行をすべて削除する<br>
  `Filter data on any column using simple expressions` で `c2<=0`
- 4列目が「hsa」で始まる行をすべて取得する<br>
  `Filter data on any column using simple expressions` で `c4.startswith('hsa')`
- 2列目と3列目の合計が10よりも大きい行をすべて削除する<br>
  `Filter data on any column using simple expressions` で `c2+c3<=10`
- 2列目に含まれる文字列の長さが10よりも大きい行をすべて削除する<br>
  `Filter data on any column using simple expressions` で `len(c2)<=10`
- 3列目に含まれるコンマで区切られたすべての値ごとに新しい行を作成する（展開）<br>
  `Unfold columns from a table` で `Column 3` かつ `Comma`
- 文字列のはじめの4文字を切り取って、新しい列の値にする<br>
  `Replace Text in entire line` で `Find Pattern`: ^(.{4}) かつ `Replace Pattern`: &\t
- 「TA」という塩基をすべての塩基配列の終わりに加える<br>
  `FASTA to Tabular` → `Add column` で `TA` → `Merge Columns` → `Cut columns` → `Tabular to FASTA`
- すべての行にダブルクォーテーション(")を追加する<br>
  `Compute an expression on every row` で `chr(34)` (34 は [ASCII](http://www.asciitable.com/) コードの `"`)
- 0を含まない数値を含むすべての列を数える。平均を計算するが、0であるすべての列を除外したい場合に便利です。<br>
  `Compute an expression on every row` で `bool(c1) + bool(c1) + bool(c3)` ...

## 次世代シークエンシング
- RNA-seqデータのマップ<br>
  `HISAT` or `TopHat`
- DNA-seqデータのマップ<br>
  `Bowtie` or `BWA`
- methylC-seq データのマップ<br>
  `Bismark`
- リードで変換される遺伝子をすべて取得する<br>
  `htseq-count` で BAM ファイルの遺伝子アノテーション [GTF file](http://www.ensembl.org/info/website/upload/gff.html) を指定 → `Filter data on any column using simple expressions` で `c2>0`
- gff, bed, gtf といったファイルから塩基配列を抽出して、FASTA ファイルを返す<br>
  `Extract Genomic DNA using coordinates from assembled/unassembled genomes`

## ワークフロー
- 近くに位置する2つの遺伝子を探す<br>
  [Description](https://github.com/bgruening/galaxytools/tree/master/workflows/ncbi_blast_plus/find_genes_located_nearby) :: [Tool Shed](https://toolshed.g2.bx.psu.edu/view/bgruening/find_genes_located_nearby_workflow)

## もっと学びたい方へ
 - Galaxy 101 - a must read for all HTS Padawan: https://wiki.galaxyproject.org/Learn/GalaxyNGS110
 - ポップコーンを食べながらGalaxyの使い方を学ぶたくさんのビデオ: https://vimeo.com/galaxyproject

## 免責条項
ここに記載されたすべてのツールは Galaxy Tool Shed で入手できます。使ってみたいときはお近くの Galaxy 管理者に相談してみてください。
