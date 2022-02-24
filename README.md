# What's this?
数理最適化の実装例。数理最適化に数値線形代数も必要になったきたので、数値線形代数のコードもある。

# ディレクトリ構成
## 最適化
### 線形計画(LP)
- [単体法](simplex)
<img src="simplex/prob.png" width="30%">

### 非線形最適化
- [最急降下法](gds)
- [ニュートン法](newton)
- [準ニュートン法(BFGS公式)](bfgs)

## 数値線形代数
- [GMRES法](gmres)
    - 連立一次方程式の反復法による解法
    - CG法を非対称行列に拡張したものと理解すれば良い
        - 各回の探索で探索空間の次元を上げてその空間における最小二乗解を求める
        - 係数行列が対称行列のときはそれを計量として直交化すれば良かったが、それができなくなるため探索空間(クリロフ部分空間)の基底を保持する必要があり、これがメモリを逼迫する
        - さらにCG法の致命的な欠点「丸め誤差に弱い」と重なり、あまり多くの回数を回すことは現実的でない
        - そのため定期的にリスタートを行う
        - リスタート回数mを明示して、「GMRES法(m)」と呼ばれることもある
- [グラムシュミット直交化](gram_schmidt)
    - 古典的なグラムシュミット直交化は他のベクトルの線型結合を引いて直交化するが、これは数値誤差にあまり強くない
    - 各ベクトルとの内積を取った直後にそのベクトルに対する直交化を行うことで、丸め誤差耐性を強化できる
- [ヘッセンベルグ形行列を係数行列とする最小二乗問題](hessenberg_minimize_square)
    - GMRES法において登場する、縦長ヘッセンベルグ行列を係数行列とする最小二乗法の解法
    - scipyで十分だったので実装していない
    - GMRES法の顕著な性質としてヘッセンベルグ行列が単調に拡張されるだけという性質があり、これを用いて計算を軽くする算法が思いつけば今後実装する可能性が高い
- [係数行列が上三角行列である連立一次方程式](triangle_inverse)
    - 逆行列計算については `numpy.linalg.inv` を使ったほうが速いという悲しい現実がある
    - 連立一次方程式解法については `numpy.linalg.solve` より若干(1.5倍程度)速い
        - 実装としては愚直に後退代入

