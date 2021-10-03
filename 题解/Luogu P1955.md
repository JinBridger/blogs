### 题目
在实现程序自动分析的过程中,常常需要判定一些约束条件是否能被同时满足。

考虑一个约束满足问题的简化版本：假设 $x_1, x_2, x_3 ...$ 代表程序中出现的变量，给定 $n$ 个形如 $x_i = x_j$ 或 $x_i \neq x_j$ 的变量相等/不等的约束条件，请判定是否可以分别为每一个变量赋予恰当的值，使得上述所有约束条件同时被满足。

例如，一个问题中的约束条件为：$x_1 = x_2, x_2 = x_3, x_3 = x_4, x_1 \neq x_4$ ,这些约束条件显然是不可能同时被满足的，因此这个问题应判定为不可被满足。

现在给出一些约束满足问题，请分别对它们进行判定。

### 思路
显然，这是维护不重叠集合的问题，使用并查集。如果两个元素相等则划为同一集合，否则查询两个元素所在集合是否相同，如果出现两个元素所在集合相同而又不相等则矛盾。

值得一提的是， __在进行并查集的操作时需要先合并完再询问，也就是说需要先进行完 $e = 1$ 的操作再进行 $e = 0$ 的操作。__

另外， $x$ 的范围较大，需要进行离散化，将其范围缩小至 $2n-1$.

### 代码：
```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e6 + 100;
int t, n, ans, fa[MAXN], X[MAXN];
struct g{
    int i ,j ,e; // 建立三元组
} gp[MAXN];

int get(int x) {
    if(x == fa[x]) return x;
    return fa[x] = get(fa[x]);
}
void merge(int x, int y) {
    x = get(x);
    y = get(y);
    fa[x] = y;
    return ;
}
int cmp(g a, g b) {
    return a.e > b.e;
}
int main() {
    scanf("%d",&t);
    for(int i = 1; i <= t; i++) {
        scanf("%d", &n);
        for(int j = 1; j <= 2 * n; j++)
            fa[j] = j;
        ans = 1;
        for(int j = 1; j <= n; j++) {
            scanf("%d%d%d", &gp[j].i, &gp[j].j, &gp[j].e);
            X[j * 2 - 1] = gp[j].i;
            X[j * 2] = gp[j].j;
        }
        sort(gp + 1, gp + 1 + n, cmp);
        sort(X + 1, X + 1 + 2 * n);
        X[0] = unique(X + 1, X + 1 + 2 * n) - X - 1; // 进行离散化
        for(int j = 1; j <= n; j++) {
            int x = lower_bound(X + 1, X + 1 + 2 * n, gp[j].i) - X - 1;
            int y = lower_bound(X + 1, X + 1 + 2 * n, gp[j].j) - X - 1;
            if(gp[j].e) {
                merge(x, y);
            } else {
                if(get(x) == get(y)) ans = 0;
            }
        }
        (ans)?(printf("YES\n")):(printf("NO\n"));
    }
    return 0;
}
```