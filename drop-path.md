# drop pathについて
論文「FRACTALNET: ULTRA-DEEP NEURAL NETWORKS WITHOUT RESIDUALS」

## わかったこと
* ドロップパスは結合層のオペランドをランダムにドロップすることで，ドロップアウトが活性化の共適応を防ぐように，ドロップパスでは並列パスの共適応を防ぐ。
* 上記によって，ネットワークが1つの入力パスをアンカーとして使用してほかのパスを修正用に使用するということ防ぐ。