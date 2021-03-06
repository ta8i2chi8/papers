# Exploring the Loss Landscape in Neural Architecture Search

## 1. abstruct
NASの多くのアルゴリズムは，アーキテクチャの空間を探索することで構成されています．すなわち，アーキテクチャを繰り返し選択し，その性能をトレーニングによって評価し，次の選択のためにすべての事前評価を使用します．この評価ステップは、重みのランダムな初期化によって最終的な精度が変化するため、ノイズが多い。これまでの研究では、アーキテクチャ評価におけるノイズのレベルを定量的に理解することよりも、このノイズを処理するための新しい探索アルゴリズムを考案することに重点が置かれていた。本研究では、(1)最も単純な丘登りアルゴリズムが、NASの強力なベースラインであること、(2)人気のあるNASベンチマークデータセットのノイズを最小限に抑えた場合、丘登りは多くの人気のある最先端アルゴリズムを凌駕すること、を示します。さらに、ノイズの減少に伴い、局所的な最小値の数が大幅に減少することを示し、NASにおける局所探索の性能を理論的に説明することで、この観察を裏付けました。これらの結果に基づき、NASの研究においては、(1)ローカルサーチをベースラインとして使用すること、(2)可能な限り学習パイプラインをノイズ除去することを提案する。

## 2. introduction
