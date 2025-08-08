# FFT

$ FFT $ 是一种 $nlogn$ 的基于分治的，可用于解决多项式乘法，大数乘法等问题，具体实现细节尚未学习完毕

```c++
#include <bits/stdc++.h>
using namespace std;
typedef complex<double> cp;
const double PI = acos(-1.0);
const int BASE = 10;
// 预处理单位根 + 位逆序
void fft(vector<cp> &a, bool invert) {
    int n = a.size();
    vector<int> rev(n);
    for (int i = 0; i < n; ++i) {
        rev[i] = (rev[i >> 1] >> 1) | ((i & 1) * (n >> 1));
        if (i < rev[i]) swap(a[i], a[rev[i]]);
    }

    for (int len = 2; len <= n; len <<= 1) {
        double ang = 2 * PI / len * (invert ? -1 : 1);
        cp wlen(cos(ang), sin(ang));
        for (int i = 0; i < n; i += len) {
            cp w(1);
            for (int j = 0; j < len / 2; ++j) {
                cp u = a[i + j], v = a[i + j + len / 2] * w;
                a[i + j] = u + v;
                a[i + j + len / 2] = u - v;
                w *= wlen;
            }
        }
    }

    if (invert) {
        for (cp &x : a) x /= n;
    }
}

// 将字符串转成数字向量（低位在前）
vector<int> toVector(const string &s) {
    vector<int> res(s.size());
    for (int i = 0; i <(int) s.size(); ++i)
        res[s.size() - 1 - i] = s[i] - '0';
    return res;
}

// 大整数乘法
string multiply(const string &sa, const string &sb) {
    vector<int> a = toVector(sa), b = toVector(sb);
    int n = 1;
    while (n < (int)(a.size() + (int)b.size()))n <<= 1;

    vector<cp> fa(n), fb(n);
    for (int i = 0; i < (int)a.size(); ++i) fa[i] = a[i];
    for (int i = 0; i < (int)b.size(); ++i) fb[i] = b[i];

    fft(fa, false);
    fft(fb, false);
    for (int i = 0; i < n; ++i) fa[i] *= fb[i];
    fft(fa, true);

    vector<int> res(n);
    for (int i = 0; i < n; ++i)
        res[i] = int(fa[i].real() + 0.5);  // 四舍五入

    // 进位处理
    for (int i = 0; i < n; ++i) {
        if (res[i] >= BASE) {//BASE为1000可加速，即压位操作
            if (i + 1 == (int)res.size()) res.push_back(0);
            res[i + 1] += res[i] / BASE;
            res[i] %= BASE;
        }
    }

    // 去除前导零
    while (res.size() > 1 && res.back() == 0) res.pop_back();

    // 转回字符串
    string result;
    for (int i = res.size() - 1; i >= 0; --i)
        result += char(res[i] + '0');
    return result;
}
int main() {
    string a, b;
    cin >> a >> b;
    cout << multiply(a, b) << endl;
    return 0;
}
```
