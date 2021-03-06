# D-DARTS: Distributed Differentiable Architecture Search

## アブスト
DARTSは、最もトレンドのあるNAS手法の一つで、SGDと重み共有を利用することで、探索コストを大幅に削減しています。しかし、この手法は探索空間を大幅に縮小するため、潜在的に有望なアーキテクチャを発見できないという問題がある。本論文では、D-DARTSを提案します。D-DARTSは、重み付けの代わりに複数のニューラルネットワークをセルレベルで入れ子にすることで、この問題を解決し、より多様で特化されたアーキテクチャを生成します。さらに、訓練された少数のセルからより深いアーキテクチャを導き出すことができる新しいアルゴリズムを導入し、性能の向上と計算時間の短縮を実現しました。我々のソリューションは、CIFAR10、CIFAR-100、ImageNetにおいて、従来のベースラインよりも大幅に少ないパラメータを使用しながら、最先端の結果を提供することができ、よりハードウェア効率の高いニューラルネットワークを実現しています。

## わかったこと
* DARTSには２つの重大な問題がある。
1. skip connectが過剰に現れていること
2. softmax演算による離散化の不一致の問題，すなわち，結果として得られる確率分布の標準偏差が非常に小さいこと
* 上記の解決策として，FairDARTSの著者はソフトマックスをシグモイドに変え，アーキテクチャ重みを0または1に近づけることを目的とした新しいLOSS（ゼロワンロス）を提案した。
$$L_{01} = -\frac{1}{N}(\sigma(\alpha) - 0.5)^2$$
* 新しく提案したことは３つ。
1. 各セルを完全なニューラルネットワークとし，探索空間を大幅に広げた
2. 各セルに固有の損失関数を設定した（アブレーション損失）
3. 計算時間を短縮しつつ、多くの層を利用するために、既に検索された小さな層からより大きなアーキテクチャを導出する新しいアルゴリズムを開発