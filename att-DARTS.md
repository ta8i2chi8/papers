# 注意機構を持った深層ニューラルネットワークの勾配探索

## １．はじめに
NASには，探索空間，探索戦略，性能推定戦略の３つの側面を持つ。
初期の手法のほとんどは探索戦略に着目しているが，本研究では探索空間に着目する。

畳み込みのみを適用の場合，ネットワークは無用なものを含めたすべての空間的およびチャンネル間の特徴を捉えてしまう。

→　無用な特徴を捨て，有用な特徴に着目するために様々なattentionが提案されている。

### １－１．att-DARTSの特徴
* 各orerationの後に，attentionを挿入する。
* エラー率の改善，パラメータ数の削減

## ２．関連研究
### ２－１．attention
* Squeeze-and-excitation(SE)
* Gather-excite(GE)
* BAM
* CBAM

## ３．わかったこと
* skip connectionが多い（多くのパラメータを持つoperationは過小評価される傾向あり）
* SEとGEはほとんどなく，CBAMとBAMが多い
* skip connectionとseparable convolutionsが多い
* → チャネル的なattentionだけではなく，空間的なattentionも必要であることがわかる
* パラメータ数の削減は，チャネル数と空間次元数が減少しているから。

### ３－１．リダクションセル内のpooling操作：
* darts ： max pooling
* att-darts ： avg pooling
* →　max poolingはattentionと似ているため，必要ない