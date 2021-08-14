# 卒業・修士論文用テンプレート

卒業・修士論文用のLaTeXテンプレートです。  
ドキュメントクラス[jsreport](https://ctan.org/pkg/jsclasses)を基本とし、以下を追加で提供します。

- 表紙体裁の変更、およびそれに伴う設定コマンドの追加
- 余白の調整（上：35mm、下：30mm、左：30mm、右：30mm）
- 図表ラベルを英語にするオプションの追加
- [cleveref](https://ctan.org/pkg/cleveref)パッケージの読み込み、および日本語利用のための調整
- その他、パッケージ追加やマクロ設定など

## 構成

tex、sty、figディレクトリで構成され、各ディレクトリの用途は以下です。

|ディレクトリ  |用途  |
|---|---|
|tex  |texファイル用  |
|sty  |スタイルファイル用  |
|fig  |図ファイル（PDFなど）用  |

figディレクトリ内に配置したPDFは、ファイル名のみで参照できます。  
つまり、`\includegraphics{fig/[ファイル名].pdf}`は`\includegraphics{[ファイル名]}`のみで実現できます。

## 使い方

### ビルド

texファイル内のthesis.texを編集し、ビルドすることで利用できます。  
thesis.texのファイル名は変更可能です。  
ビルドはTeX Live導入環境において、pLaTeXとdvipdfmxで行うことを想定しています。  
何もオプションを指定しない場合、以下でビルドが可能です。

```[bash]
platex [ファイル名].tex
dvipdfmx [ファイル名].dvi
```

実際に執筆する際は、[latexmk](https://ctan.org/pkg/latexmk/)などを利用すると便利だと思います。

### スタイルファイル読み込みオプション

スタイルファイルを読み込む際、オプションに`usefigtable`を指定することで図表ラベルを英語にできます。  
つまり、以下のようになります。

|読み込み  |出力  |
|---|---|
|`\usepackage{../sty/thesis}`  |「図 1」、「表 1」  |
|`\usepackage[usefigtable]{../sty/thesis}`  |「Fig. 1」、「Table 1」  |

これは、図表上下のラベルだけでなく`\ref`による相互参照にも反映されます。  
また、`postshiki`を指定することで、`\cref`で式を参照した場合に「(1)式」のように「式」が番号の後ろにつきます。  
つまり、以下のようになります。

|読み込み  |出力  |
|---|---|
|`\usepackage{../sty/thesis}`  |「式(1)」、「式(1)－(4)」  |
|`\usepackage[postshiki]{../sty/thesis}`  |「(1)式」、「(1)－(4)式」  |

`usefigtable`と`postshiki`の両方を指定する場合は`\usepackage[usefigtable,postshiki]{../sty/thesis}`として読み込んでください。

### 表紙用の設定コマンド

[jsreport](https://ctan.org/pkg/jsclasses)から追加で以下の設定コマンドが追加されています。

|コマンド  |引数  |
|---|---|
|`\papername`  |論文名（卒業論文・修士論文等）  |
|`\id`  |学籍番号  |
|`\university`  |大学名  |
|`\cource`  |研究科名  |
|`\major`  |専攻名  |
|`\supervisor`  |指導教員名（複数設定可）  |
|`\postdate`  |提出年月日  |

詳細な設定方法については[template.tex](tex/template.tex)を参照してください。

### [cleveref](https://ctan.org/pkg/cleveref)の利用

cleverefパッケージは`\ref`による相互参照の機能拡張を行うものです。  
`\ref`の代わりに`\cref`コマンドを用いることで利用できます。  
主な機能としては、参照対象に応じて自動でラベル名を変更する機能や、複数参照機能などがあります。  
つまり、図を参照する際に`図 \ref{[ラベル]}`としていたものを`\cref{[ラベル]}`のみで実現したり、`図 \ref{[ラベル_1]}, \ref{[ラベル_2]}`としていたものを`\cref{[ラベル_1],[ラベル_2]}`として実現することが可能です。  
また、`\cref{[ラベル_1],[ラベル_2],[ラベル_3]}`のような連続する三つ以上の参照では、「図 \[ラベル出力_1\]～\[ラベル出力_3\]」のように出力されます。
詳細は[cleverefのドキュメンテーション](http://ftp.math.purdue.edu/mirrors/ctan.org/macros/latex/contrib/cleveref/cleveref.pdf)を参照してください。

cleverefパッケージのデフォルト設定では「図」や「表」が英語表記となっているため、このテンプレートでは日本語で利用できるように設定を変更しています。  
章と節を含め、全て`\cref`で参照できます。 
番号の後に「式」を持ってくる場合は、スタイルファイル読み込み時のオプションで変更できます。  
「スタイルファイル読み込みオプション」を参照してください。  

まとめると、以下になります。

|参照対象  |コマンド  |
|---|---|
|図、表、式  |`\cref`  |
|式（「式」を後ろにする場合）  |`\equcref`（廃止予定）  |
|章  |`\chapcref`（廃止予定）  |
|節  |`\seccref`（廃止予定）  |

cleverefパッケージを利用したくない場合、`\ref`コマンドを利用することも可能です。

### 追加マクロ

以下のマクロを追加しています。

|コマンド  |用途  |
|---|---|
|`\figref`  |図参照（cleverlefを利用しない場合）  |
|`\tabref`  |表参照（cleverlefを利用しない場合）  |
|`\equref`  |式参照（cleverlefを利用しない場合）  |
|`\chapref`  |章参照（cleverlefを利用しない場合）  |
|`\secref`  |節参照（cleverlefを利用しない場合）  |
|`\equlr`  |equation環境内の括弧  |
|`\relmiddle`  |`\middle`な`\mid`（[こちら](http://d.hatena.ne.jp/zrbabbler/20120411/1334151482)を参考にしました）  |

詳細は[thesis.sty](sty/thesis.sty)を参照してください。

### 読み込みパッケージ

以下の通り読み込んでいます。

```[tex]
\RequirePackage{amsmath,amsfonts}
\RequirePackage{algpseudocode,algorithm}
\RequirePackage{url}
\RequirePackage{cite}
\RequirePackage[capitalise]{cleveref}
```

使い方等は、各パッケージのドキュメンテーションを参照してください。
