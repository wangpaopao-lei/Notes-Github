### 11.1 Coding of Binary Information and Error Detection
Parity check code **奇偶校验码**：在最后加上一位，如果码字的权为奇数，则加1；反之。
Hamming distance **海明距离**：Let x and y be words in $B^m$. The Hamming distance $\delta(x,y)$ between x and y is the weight, **$|x\oplus y|$**.
Minimum Distance **最短距离**：encoding function :$e:B^m \to B^n$
        $min{  \delta (e(x),e(y))}$,接受码字的最短距离。

Theorem2: 
1. An (m,n) encoding function e can detect k or fewer errors (if and only if)
2. its minimum distance is at least k+1.
