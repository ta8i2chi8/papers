# Fair DARTS: Eliminating Unfair Advantages in Differentiable Architecture Search

## アブスト
DARTSは、スキップ接続の集約が避けられないため、よく知られているように性能が低下するという問題があります。本論文では、その根本的な原因が、独占的な競争における不当な優位性にあることを初めて明らかにしました。実験により、2つの条件のうちどちらかが崩れると、崩壊がなくなることを示しました。そこで、排他的競争を協調的なものに緩和したFair DARTSという新しいアプローチを提案します。具体的には、各操作のアーキテクチャの重みを他の操作に依存しないようにします。しかし、離散化の不一致という重要な問題が残っています。そこで、アーキテクチャの重みをゼロまたは1に近づけるゼロワン損失を提案し、期待されるマルチホットソリューションを近似的に実現する。実験は、2つの主流な探索空間で行われ、CIFAR-10とImageNet 3において、新しい最先端の結果を導き出した。

## イントロ
* DARTSの問題点は，skipconnectが多いことと連続的なアーキテクチャを離散化するときのギャップ

## わかったこと
### skip-connect
* skip-connectが支配的になる原因は，各オペレーションの排他的な競争にある。
* skip-connectはResnetのようなresidual moduleに似ている。このモジュールは学習に大きなメリットをもたらすが，skip-connectのアーキテクチャ重みは，ほかのオペレーションのよりもはるかに速く増加する。
* さらにsoftmax演算は本質的に排他的な競争をもたらす。なぜなら，あるものを増加させることは他のものを抑制することになるから。その結果，最適化の過程でskip-connectが徐々に優位になっていく。
* skip-connectがうまく機能するのは，畳み込みと連携しているからである。それにもかかわらず，DARTSは最も性能の高いもの（ここではskip-connect）を選び，その協力者（畳み込み）を捨ててしまうので，結果的に縮退したモデルになる。
### Unfair Advantage
* ほかのオペレーションの中から１つのオペレーションを選択することが競争であるとすると，この競争は限定されたオペレーションしか選択できない場合，排他的とみなされる。排他的な競争に参加しているオペレーションは，この優位性が結果的にネットワークのパフォーマンスよりも競争に貢献している場合，不当な優位性を持っているといえる。
* 以上の考察から，過剰なskip-connectの根本原因は，固有の不公平な競争になると考える。skip-connectは、residual moduleを形成することで、スーパーネットの学習には便利ですが、residual moduleが壊れた結果のネットワークのパフォーマンスには同様に有益ではないという不公平な利点があります。
* この問題の解決策として，オペレーションの連続化に用いていたsoftmaxをsigmoidに変更する
### 連続的なアーキテクチャを離散化する際の矛盾
* アーキテクチャ重みαがsoftmaxで変換されているため，各確率の差が小さい。このような微々たる差で甲乙つけるのはよくない。
* また，[0.235, 0.057, 0.17, 0.016, 0.187, 0.269, 0.066]から[0, 0, 0, 0, 1, 0]のような近似をしなければならず，学習時と推論時の差が大きくなる。
* この問題の解決策として，各アーキテクチャ重みを0か1に近づけるための損失関数を導入する。（ゼロワンロス）