# DARTS-: ROBUSTLY STEPPING OUT OF PERFORMANCE COLLAPSE WITHOUT INDICATORS
## abstract
微分可能アーキテクチャ探索(DARTS)は、急速な発展を遂げているにもかかわらず、長年にわたる性能の不安定さに悩まされており、その用途は極めて限られています。既存のロバスト化手法は、原因となる要素を見つけるのではなく、結果的に悪化した挙動からヒントを得ています。性能が低下する前に探索を中止するためのシグナルとして、ヘシアン固有値などの様々な指標が提案されています。しかし、これらの指標に基づく手法は、しきい値の設定が不適切であったり、探索が本質的にノイズであったりすると、良好なアーキテクチャを簡単に拒絶してしまう傾向がある。本論文では、この問題を解決するために、より繊細で直接的なアプローチを採用しました。まず、スキップ接続が他の候補操作よりも明らかに有利であり、不利な状態から容易に回復して優位に立てることを実証します。私たちは、この特権がパフォーマンスの低下を引き起こしていると推測します。そこで、この利点を補助的なスキップ接続で因数分解し、すべての操作の公平な競争を保証することを提案します。このアプローチをDARTS-と呼んでいます。様々なデータセットを用いた大規模な実験により、このアプローチがロバスト性を大幅に向上させることが確認されました。

## 本研究の貢献
1. 現在の指標が、探索空間における探索範囲の減少を犠牲にして性能崩壊を回避できることを経験的に観察しながら、DARTSを安定化させるための新しい指標なしのアプローチ(DARTS-1と呼ぶ)を提案する。このアプローチでは、補助的なスキップ接続(図1参照)を用いて、探索段階における不公平な利点を取り除く。
2. 7つの探索空間と3つのデータセットで徹底的な実験を行い、我々の手法の有効性を実証する。具体的には、最終的な性能を報告するために4回の独立した実行を必要とするR-DARTSと比較して、本手法は3倍少ない検索コストで4つの検索空間で最先端の結果をロバストに得ることができる。
3. 我々のアプローチは、余分なオーバーヘッドなしに彼らの手作りの指標を取り除くことによって、他の直交DARTSバリアントとシームレスに連携できることを実証するための実験を行います。具体的には、CIFAR-10データセットにおいて、P-DARTSでは0.8％、PC-DARTSでは0.25％の精度向上を実現しています。

## わかったこと
* dartsにおいて，余計なスキップ接続による性能崩壊が問題となっている。
* DARTSでskip connectionのパラメータαが大きくなるときは主に2つ。1つ目は、スーパーネット(離散的に選択される前のネットワーク)が勾配消失を緩和するように自動的に学習するため、βskip(skipconnectionのhyperparameter)を適切な大きな値に押し上げてしまう場合。２つ目は、スキップ接続は対象ネットワークにとって重要な接続であり、離散化段階で選択されるべきものである場合。
* 上記の結果、DARTSにおけるスキップ接続は、スーパーネットの学習を安定させるための補助的な接続と、最終的なネットワークを構築するための候補操作という2つの役割を担っていることがわかる。そこで，2つの役割を区別するために、図1(b)に示すように、セル内の2つのノード間に補助的なスキップ接続を導入する。

## conclusion
基本的な考え方は、補助的なスキップ接続分岐を利用して、候補となるスキップ接続操作から勾配を利用する役割を引き継ぐことです。
これにより、2段階の最適化プロセスが、良い操作と悪い操作を簡単に区別できるような、公平な競争を生み出すことができます。
その結果、探索プロセスがより安定し、様々な探索空間や異なるデータセットの間で崩壊が起こることはほとんどありません。
厳密に制御された設定の下では、最近の最新技術であるRobustDARTSを3倍少ない探索コストで着実に凌駕しています。
さらに、我々の手法は、様々な手作りの正則化トリックを認めない。
最後になりましたが、この手法は単独でも使用できますし、必要に応じて様々な直交的な改良と協力することもできます。
この論文は、今後の研究のために2つの重要なメッセージを伝えている。
一方で、性能崩壊のためのヘシアン固有値指標は、良いモデルを拒絶する危険性があるため、理想的ではない。
一方で、提案されている方法ではなく、手作業による正則化トリックの方が、良いモデルを探索するためにはより重要であると思われる。
では、その解決策は？
原理的には、崩壊の完璧な指標を見つけることは困難です。私たちのアプローチは、探索プロセスをコントロールできる可能性を示しており、最終的なモデルに制限やプライヤーを課すものではありません。この方向にもっと注目が集まることを期待しています。