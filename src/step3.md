# Step3.学習

## 学習実施

さて、Step2 にてデータの設定ができたので、続いて学習を行います。

といっても特に難しいことはありません。データのインポート同じくガイドに従ってボタンを押していくだけです。

![TrainPredictor](/images/step3/TrainPredictor.png)

- 学習実施

  - PredictorName: Train1
  - ForecastHorizon: 20

    ![TrainDetail](/images/step3/TrainDetail.png)

::: tip ForecastHorizon
ForecastHorizen は学習の期間を定義するものです。
というのも、機械学習は、学習したらそれを評価する必要があります。
機械学習のステップとしては、

1. データを使って学習。
1. 学習した結果がどの程度正しいか評価。
1. 評価した結果で、そのほかのパラメータでも実施してみて、正答率を上げていく。

というプロセスを踏んでいきます。
つまり、学習するためのデータとそれを評価するためのデータと両方が必要となります。その期間について、定義するのがこの ForecastHorizon という値になります。
一般的には 1/3 を学習データとするのが良いとされておりますので、今回は全体 60 に対してセオリー通り 20 件を設定した、というわけです。

実際に用意した全てのデータを訓練データに使えないというのは若干悲しいですが、これはもう仕方ないので、たくさんデータを用意しましょう。

:::

::: warning 予測期間

注意しなくてはいけないのは、この ForecastHorizon がそのまま予測可能な期間になります。例えば、今回の例でいうと、InputData は 2020-12-01~2021-01-31 の 60 個ぐらいで、ForecastHorizon はその 1/3 である 20 にしましたが、そうなると、予測可能な期間は、2021-02-01 から、20 日後の 2021-02-20 となります。

従って、長めに（より長い期間の予測）を確認したいのであれば、必然的にたくさんデータが必要になりますので、ご留意ください。

:::

後の項目は変更しなくても問題ないです。そのまま Train を実施しましょう。

## その他設定値項目

これではあまりに簡単すぎるの簡単に今回設定しなかった値についても主要な項目については簡単に説明しておきます。

- Algorithm

  アルゴリズムを指定することもできます。が、基本的には AutoML で自動的に選択した方がいいと思います。（どうしてもこのアルゴリズムを使いたい、ということはないと思っています、、、）

  ![Algorithm](/images/step3/Algorithm.png)

- ForecastType

  予測値のバリエーションを指定することができます。これは、統計的には P 値と呼びますが、予測を立てたときにドンピシャで当てることは難しいので、ある程度の幅を持たせて予測を行います。

  Forecast では、あり得そうな値の中で、幅をもたせ、比較的高い方の予測を P90、比較的低い方の予測を P10、予測通りのものを P50 とし、基本的には 3 種類の予測を実施しますが、これは調整できます。

  ![ForecastType](images/step3/ForecastType.png)

::: tip ForecastType
販売個数をイメージにとってみると、”今までの経験に基づく人間の直感だと 100 個くらい売れそうだなぁ。よくて 120 個、悪くて 80 個だなぁ”といった場合、高い方の直感を P90 と呼び、低い方の直感を P10、直感通りのものを P50 と呼びます。

なんでこんなことになっているかというと、例えば、上記の場合、在庫を余らせたくない場合は低い方の 80 個を選ぶべきで、売りたいのに在庫がなくて売れなかった場合の機会損失を重視するなら 120 個を用意するべきですよね。

このようにある程度あり得る確率の中で、幅を持たせることによって、それを人間が選択し、戦略に基づいてその予測を効果的に利用していくことができるのです。

:::

- Supplementaly Features

  AWS ではデフォルトで天気や祝日のデータを予測に組み込むことができます。この辺りは基本的に利用する、でいいと思います。

  ![Features](/images/step3/Features.png)

## 処理時間

正直ここが一番時間かかります。
内部的には様々なアルゴリズムが処理されており、時間がかかるのも無理もないなと思いますが、バッチ処理的に処理を実施して適当なタイミングで確認するといいかなと思います。（このあたり SaaS のサービスはいいですね。勝手に処理してくれるので。）