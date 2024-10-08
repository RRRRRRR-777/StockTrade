# 使用技術一覧
## 言語
Python
## ライブラリ
pandas / numpy / openpyxl / selenium / request / beautifulsoup

# TL;DR
このコードを実行すると`xlsxファイル`が出力され、
そのファイルを見ることで株式投資のスクリーニング時間を大幅に短縮できます。
極端な話し、Ticker列が青色の銘柄のテクニカル分析が優れていればその銘柄を購入することができます。
#### ※ テクニカル分析とは
[過去の値動きをチャートで表して、そこからトレンドやパターンなどを把握し、今後の株価、為替動向を予想するものです。](https://info.monex.co.jp/technical-analysis/column/001.html#:~:text=%E3%83%86%E3%82%AF%E3%83%8B%E3%82%AB%E3%83%AB%E5%88%86%E6%9E%90%E3%81%A8%E3%81%AF%E3%81%99%E3%82%99%E3%81%AF%E3%82%99%E3%82%8A%E3%80%81%E9%81%8E%E5%8E%BB%E3%81%AE%E5%80%A4%E5%8B%95%E3%81%8D%E3%82%92%E3%83%81%E3%83%A3%E3%83%BC%E3%83%88%E3%81%A6%E3%82%99%E8%A1%A8%E3%81%97%E3%81%A6%E3%80%81%E3%81%9D%E3%81%93%E3%81%8B%E3%82%89%E3%83%88%E3%83%AC%E3%83%B3%E3%83%88%E3%82%99%E3%82%84%E3%83%8F%E3%82%9A%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%AA%E3%81%A8%E3%82%99%E3%82%92%E6%8A%8A%E6%8F%A1%E3%81%97%E3%80%81%E4%BB%8A%E5%BE%8C%E3%81%AE%E6%A0%AA%E4%BE%A1%E3%80%81%E7%82%BA%E6%9B%BF%E5%8B%95%E5%90%91%E3%82%92%E4%BA%88%E6%83%B3%E3%81%99%E3%82%8B%E3%82%82%E3%81%AE%E3%81%A6%E3%82%99%E3%81%99%E3%80%82)

# 実行方法
## パッケージのインストール
`conda install --file conda_requirements.txt`<br />
`pip install -r requirements.txt`

※ anacondaで仮想環境を構築してください <br />[Linuxにanacondaインストールしてパスを通して仮想環境作成](https://qiita.com/lighlighlighlighlight/items/8f624751df808d89d48c)
## main.pyの実行
```shell
cd .
python main.py
// python3の場合は
python3 main.py
```

## 各関数の機能
### InitProcess
- csvをダウンロードするディレクトリを構成する
### PickFinviz
- finvizからティッカーコード、企業名、業種等のデータを取得しinput.txtに出力する
### HistData
- input.txtに格納されている銘柄を参照し、それぞれの1970/01/01から本日までのヒストリカルデータを取得しティッカーコード.csvの形式でダウンロードディレクトリに保存する。
### ProcessNASDAQ
- NASDAQのヒストリカルデータを取得する
- ミネルビィ二のトレンドテンプレートに沿った項目を追加する

    ※ [ミネルビィ二のトレンドテンプレートとは](https://investortat.com/apply_trend_template_for_japanese_stocks/)
- ^IXIC.csvの形式でダウンロードディレクトリに保存する。
### ProcessHistData
- 各銘柄にミネルビィ二のトレンドテンプレートに沿った項目を追加する
- NASDAQのトレンドテンプレートの指標7つの合計の項目を追加する
- ティッカーコード.csvの形式でダウンロードディレクトリに保存する
### CalculateRS
- 各銘柄のRSの素点を計算し、1000銘柄全てと比較したRSの点数の項目を追加する
- すべての銘柄の最新の値をまとめたComprehensive.csvとして出力する。
### BuyingStock
- 購入基準を満たしている銘柄を選出する
- BuyingStock.csvを出力する
### CurrentAnnual
- CANSLIMのC(CureentQuarterlyEarnings)A(AnnualEarningsInvreases)の部分を計算する
- CourrentAnnual.csvとして出力する。
### Institutional
- CANSLIMのI(InstitutionalSponsorship)の部分を取得する。
- Perf_Inst.csvとして出力する。
### AppendData
- BuyingStock.csvに値を追加する
- 主にCourrentAnnual.csvとPerf_Inst.csvの値をBuyingStock.csvに結合している
### ConvertExcel
- Excelに変換後視覚情報を追加

# 環境変数
|役割|変数|値|
|---|---|---|
|ログの出力設定|OUTPUT_LOGGER_LEVEL|20|
|finvizのサイトURL|FINVIZ_URL|https://finviz.com/screener.ashx?v=152&f=cap_smallover,geo_usa,ind_stocksonly,sh_avgvol_o500&o=-marketcap&c=0,1,2,3,4,6,26,33,68,67,65&r=|
|NASDAQヒストリカルデータのURL|YFINANCE_NASDAQ_URL|https://query1.finance.yahoo.com/v7/finance/download/^IXIC?period1=|
|各銘柄ヒストリカルデータのURL|YFINANCE_STOCKS_URL|https://query1.finance.yahoo.com/v7/finance/download/|
|アナリスト情報のURL|ANALYSTS_URL|https://finance.yahoo.com/quote/|
|過去決算情報のURL|FINANCE_URL|https://dilutiontracker.com/app/search/|
|機関投資家情報のURL|INSTITUTE_URL|https://13f.info|
