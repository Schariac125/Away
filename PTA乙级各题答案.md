# PTA乙级各题答案

## 前言：

这份答案是笔者在2025年备考期间自行整理的答案，绝大部分为笔者亲自书写代码， 部分可能会借鉴AI思路。

不排除PAT官方后续修改测试数据而导致答案出现错误的情况，如有发现可向我反馈。

编译语言为C++（g++），其他编译语言如C不保证能够通过。

因笔者水平有限，答案并不一定为时间空间复杂度最优解（其实是大部分不是），只保证可AC，oi大佬轻喷OTZ

​                                                                                                                                                                                                     ——By Schariac125

## 正文：

## 1001

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin>>n;
    int ans=0;
    while (1){
        if (n==1){
            break;
        }
        else if (n%2==1){
            n=(3*n+1)/2;
            ans++;
        }
        else if (n%2==0){
            n/=2;
            ans++;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

## 1002

```cpp
#include <bits/stdc++.h>
using namespace std;

string num;
int sum=0;
string b[15] = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
int main(){
    getline(cin,num);
    for (int i=0;i<num.size();i++){
        sum+=(num[i]-'0');
    }
    vector<string> ans;
    while (sum>0){
        int temp=sum%10;
        ans.push_back(b[temp]);
        sum/=10;
    }
    for (int i=ans.size()-1;i>=0;i--){
        if (i==0){
            cout<<ans[i];
        }else{
            cout<<ans[i]<<" ";
        }
    }
    //cout<<sum<<endl;
    return 0;
}
```

## 1003

```cpp
#include <bits/stdc++.h>
using namespace std;

bool checkPAT(const string &s) {
    // 检查只包含 P A T
    for (char c : s) {
        if (c != 'P' && c != 'A' && c != 'T')
            return false;
    }

    int p = s.find('P');
    int t = s.find('T');

    // 必须有且仅有一个P和一个T 且P在T前
    if (p == string::npos || t == string::npos || p >= t) return false;
    if (count(s.begin(), s.end(), 'P') != 1) return false;
    if (count(s.begin(), s.end(), 'T') != 1) return false;

    int x = p;                    // P 左边的 A 数量
    int y = t - p - 1;            // P 和 T 之间的 A 数量
    int z = s.length() - t - 1;   // T 右边的 A 数量

    if (y == 0) return false;     // 中间必须至少有一个 A
    return z == x * y;
}

int main() {
    int n;
    cin >> n;
    while (n--) {
        string a;
        cin >> a;
        cout << (checkPAT(a) ? "YES" : "NO") << endl;
    }
    return 0;
}
```

题目误导性很强，建议仔细审题

## 1004

```cpp
#include <bits/stdc++.h>
using namespace std;

class stu{
    public:
    string name;
    string num;
    double gpa;
};
int n;
bool cmp(stu a,stu b){
    return a.gpa>b.gpa;
}
int main(){
    cin>>n;
    vector<stu> add;
    while (n--){
        stu l;
        cin>>l.name>>l.num>>l.gpa;
        add.push_back(l);
    }
    sort(add.begin(),add.end(),cmp);
    cout<<add[0].name<<" "<<add[0].num<<endl;
    cout<<add[add.size()-1].name<<" "<<add[add.size()-1].num<<endl;
    return 0;
}
```

## 1005

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,a;
int b[10005];
int vis[10005];
void biao(int s){
    while (1){
        if (s==1){
            break;
        }
        else if (s%2==0){
            s/=2;
            if (!b[s]){
                b[s]=1;
            }
        }
        else if (s%2==1){
            s=(3*s+1)/2;
            if (!b[s]){
                b[s]=1;
            }
        }
    }
    return;
}
int main(){
    cin>>n;
    while (n--){
        cin>>a;
        vis[a]=1;
        if (b[a]==0){
            biao(a);
        }
    }
    vector<int> ans;
    for (int i=2;i<=10005;i++){
        if (b[i]==0&&vis[i]==1){
            ans.push_back(i);
        }
    }
    sort(ans.begin(),ans.end(),greater<int>());
    for (int i=0;i<ans.size();i++){
        if (i==ans.size()-1){
            cout<<ans[i];
        }else{
            cout<<ans[i]<<" ";
        }
    }
    return 0;
}
```

这东西前面几个测试点数据太弱了。

## 1006

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int main(){
    cin>>n;
    int b=n/100;
    int s=(n/10)%10;
    int g=n%10;
    int r=1;
    while (b>0){
        cout<<'B';
        b--;
    }
    while (s>0){
        cout<<'S';
        s--;
    }
    while (g>0){
        cout<<r;
        r++;
        g--;
    }
    return 0;
}
```

## 1007

```cpp
#include <bits/stdc++.h>
using namespace std;

bool su(int a){
    if (a==2) return true;
    else{
        for (int i=2;i*i<=a;i++){
            if (a%i==0){
                return false;
            }
        }
    }
    return true;
}
int n;
int main(){
    cin>>n;
    int ans=0;
    vector<int> b;
    for (int i=2;i<=n;i++){
        if (su(i)){
            b.push_back(i);
        }
    }
    for (int i=1;i<b.size();i++){
        if (b[i]-b[i-1]==2){
            ans++;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

## 1008

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,m,x;
deque<int> d;//双端队列

int main(){
    cin>>n>>m;
    for (int i=1;i<=n;i++){
        cin>>x;
        d.push_back(x);
    }
    while (m--){
        int r=d.back();
        d.pop_back();
        d.push_front(r);    
    }
    vector<int> ans;
    for (auto it:d){
        ans.push_back(it);
    }
    for (int i=0;i<ans.size();i++){
        if (i==ans.size()-1){
            cout<<ans[i];
        }else{
            cout<<ans[i]<<" ";
        }
    }
    return 0;
}
```

看到题目的第一反应就是双端队列了，STL是真的很好用。

## 1009

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
int main(){
    getline(cin,a);
    vector<string> ans;
    string word;
    for (char c:a){
        if (c==' '){
            if (!word.empty()){
                ans.push_back(word);
                word.clear();
            }
        }else{
            word+=c;
        }
    }
    if (!word.empty()){
        ans.push_back(word);
    }
    for (int i=ans.size()-1;i>=0;i--){
        if (i==0){
            cout<<ans[i];
        }else{
            cout<<ans[i]<<" ";
        }
    }
    return 0;
}
```

重点在于把每个单词按空格分割出来

## 1010

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int a, b;
    bool hasOutput = false;

    while (cin >> a >> b) {
        if (b != 0) {
            if (hasOutput) cout << " ";
            cout << a * b << " " << b - 1;
            hasOutput = true;
        }
    }

    if (!hasOutput) {
        cout << "0 0";
    }

    return 0;
}
```

## 1011

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
int t;
int a,b,c;

signed main(){
    cin>>t;
    int h=1;
    while (t--){
        cin>>a>>b>>c;
        if (a+b>c){
            cout<<"Case #"<<h<<": true"<<endl;
        }else{
            cout<<"Case #"<<h<<": false"<<endl;
        }
        h++;
    }
    return 0;
}
```

## 1012

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,x;
vector<int> ans1,ans2,ans3,ans4,ans5;
int main(){
    cin>>n;
    while(n--){
        cin>>x;
        if (x%5==0&&x%2==0){
            ans1.push_back(x);
        }else if(x%5==1){
            ans2.push_back(x);
        }else if(x%5==2){
            ans3.push_back(x);
        }else if(x%5==3){
            ans4.push_back(x);
        }else if(x%5==4){
            ans5.push_back(x);
        }
    }
    if (ans1.empty()){
        cout<<"N"<<" ";
    }else{
        int sum1=0;
        for (int i=0;i<ans1.size();i++){
            sum1+=ans1[i];
        }
        cout<<sum1<<" ";
    }
    if (ans2.empty()){
        cout<<"N"<<" ";
    }else{
        int sum2=0;
        for (int i=0;i<ans2.size();i++){
            sum2+=ans2[i]*pow(-1,i+2);
        }
        cout<<sum2<<" ";
    }
    if (ans3.empty()){
        cout<<"N"<<" ";
    }else{
        cout<<ans3.size()<<" ";
    }
    if (ans4.empty()){
        cout<<"N"<<" ";
    }else{
        double sum4=0;
        for (int i=0;i<ans4.size();i++){
            sum4+=1.0*ans4[i];
        }
        sum4/=ans4.size();
        printf("%.1f ",sum4);
    }
    if (ans5.empty()){
        cout<<"N";
    }else{
        int maxx=0;
        for (int i=0;i<ans5.size();i++){
            maxx=max(maxx,ans5[i]);
        }
        cout<<maxx;
    }
    return 0;
}
```

这个解法绝对非内存上的最优解，但是我觉得这个解法最清晰就是了。

## 1013

```cpp
#include <bits/stdc++.h>
using namespace std;

bool su(int a){
    if (a < 2) return false;
    if (a == 2) return true;
    for (int i=2;i*i<=a;i++){
        if (a%i==0) return false;
    }
    return true;
}

int main(){
    int m,n;
    cin>>m>>n;
    vector<int> ans;
    int num = 2; 
    while (ans.size() < n){  
        if (su(num)) ans.push_back(num);
        num++;
    }

    int flag = 1;
    for (int i=m-1;i<n;i++){
        if (i == n-1){
            cout << ans[i]; // 最后一个数字后不加空格
        }
        else if (flag == 10){
            cout << ans[i] << endl;
            flag = 1;
        }else{
            cout << ans[i] << " ";
            flag++;
        }
    }
    return 0;
}
```

## 1014

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    string s1, s2, s3, s4;
    getline(cin, s1);
    getline(cin, s2);
    getline(cin, s3);
    getline(cin, s4);

    // 1) 找星期
    int i = 0;
    int n12 = min(s1.size(), s2.size());
    int dayIdx = -1;
    for (; i < n12; ++i) {
        if (s1[i] == s2[i] && s1[i] >= 'A' && s1[i] <= 'G') {
            dayIdx = s1[i] - 'A';
            break;
        }
    }

    // 2) 找小时（从 i+1 开始）
    int hour = -1;
    for (++i; i < n12; ++i) {
        if (s1[i] == s2[i]) {
            char c = s1[i];
            if (c >= '0' && c <= '9') {
                hour = c - '0';
                break;
            }
            if (c >= 'A' && c <= 'N') {
                hour = c - 'A' + 10;
                break;
            }
        }
    }

    // 3) 找分钟（第一个相同的英文字母位置）
    int n34 = min(s3.size(), s4.size());
    int minute = -1;
    for (int k = 0; k < n34; ++k) {
        if (s3[k] == s4[k] && ((s3[k] >= 'A' && s3[k] <= 'Z') || (s3[k] >= 'a' && s3[k] <= 'z'))) {
            minute = k;
            break;
        }
    }

    static const char* dayName[] = {"MON","TUE","WED","THU","FRI","SAT","SUN"};
    cout << dayName[dayIdx] << ' ';
    cout << setw(2) << setfill('0') << hour << ':'
         << setw(2) << setfill('0') << minute << "\n";
    return 0;
}
```

## 1015

```cpp
#include <bits/stdc++.h>
using namespace std;

class stu{
    public:
    long long num;
    int d,c;
};
bool cmp(stu x,stu y){
    if (x.c+x.d==y.c+y.d){
        if (x.d==y.d){
            return x.num<y.num;
        }else{
            return x.d>y.d;
        }
    }else{
        return x.d+x.c>y.d+y.c;
    }
}
int n,l,h;
int main(){
    cin>>n>>l>>h;
    stu a;
    int m=0;
    vector<stu> one,sec,thr,four;
    while (n--){
        cin>>a.num>>a.d>>a.c;
        if (a.d>=l&&a.c>=l){
            m++;
            if (a.d>=h&&a.c>=h){
                one.push_back(a);
            }else if(a.d>=h&&a.c<h){
                sec.push_back(a);
            }else if(a.d<h&&a.c<h&&a.d>=a.c){
                thr.push_back(a);
            }else{
                four.push_back(a);
            }
        }
    }
    sort(one.begin(),one.end(),cmp);
    sort(sec.begin(),sec.end(),cmp);
    sort(thr.begin(),thr.end(),cmp);
    sort(four.begin(),four.end(),cmp);
    cout<<m<<endl;
    for (auto it:one){
        cout<<it.num<<" "<<it.d<<" "<<it.c<<endl;
    }
    for (auto it:sec){
        cout<<it.num<<" "<<it.d<<" "<<it.c<<endl;
    }
    for (auto it:thr){
        cout<<it.num<<" "<<it.d<<" "<<it.c<<endl;
    }
    for (auto it:four){
        cout<<it.num<<" "<<it.d<<" "<<it.c<<endl;
    }
    return 0;
}
```

写代码的过程是很痛苦的，你应该庆幸的是学号只有八位不用开string

磨性子，耐心写写。

## 1016

```cpp
#include <bits/stdc++.h>
using namespace std;

string a,b;
int Da,Db,suma=0,sumb=0;

int main(){
    cin>>a>>Da>>b>>Db;
    for (auto c:a){
        if (c-'0'==Da){
            suma=suma*10+Da;
        }
    }
    for (auto c:b){
        if (c-'0'==Db){
            sumb=sumb*10+Db;
        }
    }
    cout<<suma+sumb<<endl;
    return 0;
}
```

没有尝试过int会不会爆，有兴趣可以一试，我以防极端数据用了字符串。

## 1017

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
int b;
int main(){
    cin>>a>>b;
    int r=0,num=0;
    string s,ans;
    for (char c:a){
        num=r*10+(c-'0');
        s+=(num/b)+'0';
        r=num%b;
    }
    int pos=0;
    while (pos<s.size()-1&&s[pos]=='0'){
        pos++;
    }
    ans=s.substr(pos);
    cout<<ans<<" "<<r<<endl;
    return 0;
}
```

高精度除法，变态的模拟题。

## 1018

```cpp
#include <bits/stdc++.h>
using namespace std;

int a[3][3] = {
    {0, 1, -1},
    {-1, 0, 1},
    {1, -1, 0}
};

int s1=0,p1=0,f1=0;
int s2=0,p2=0,f2=0;

int main() {
    int n;
    cin >> n;
    map<char,int> m;
    m['C']=0, m['J']=1, m['B']=2;

    int win1[3]={0}, win2[3]={0};

    while (n--) {
        char x,y;
        cin >> x >> y;
        int c = m[x];
        int d = m[y];
        if (a[c][d] == 0) {
            p1++, p2++;
        } else if (a[c][d] == 1) {
            s1++, f2++, win1[c]++;
        } else {
            s2++, f1++, win2[d]++;
        }
    }

    cout << s1 << " " << p1 << " " << f1 << endl;
    cout << s2 << " " << p2 << " " << f2 << endl;

    char gesture[3] = {'C','J','B'};
    int order[3] = {2,0,1}; // 按 B,C,J 优先级

    int max1 = order[0];
    for (int i = 1; i < 3; i++) {
        int idx = order[i];
        if (win1[idx] > win1[max1]) max1 = idx;
    }

    int max2 = order[0];
    for (int i = 1; i < 3; i++) {
        int idx = order[i];
        if (win2[idx] > win2[max2]) max2 = idx;
    }

    cout << gesture[max1] << " " << gesture[max2] << endl;

    return 0;
}
```

这个题目可以用一串很长的if else干掉，但是你知道的，那样代码超级长，打表好写一点。

## 1019

```cpp
#include <bits/stdc++.h>
using namespace std;

string to4(int n) {
    string s = to_string(n);
    while (s.size() < 4) s = "0" + s;
    return s;
}

int main() {
    int n;
    cin >> n;
    while (true) {
        string s = to4(n);
        string s1 = s, s2 = s;
        sort(s1.begin(), s1.end(), greater<char>()); // 降序
        sort(s2.begin(), s2.end());                  // 升序
        int a = stoi(s1);
        int b = stoi(s2);
        n = a - b;
        cout << s1 << " - " << s2 << " = " << to4(n) << endl;
        if (n == 0 || n == 6174) break;
    }
    return 0;
}
```

保留前导0，纯神人来的。

to_string：数字转字符串

stoi()：字符串转数字

## 1020

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
double d;
class coke{
    public:
    double k,p;
}a[1010];
bool cmp(coke x,coke y){
    double p1=x.p/x.k;
    double p2=y.p/y.k;
    return p1>=p2;
}
int main(){
    cin>>n>>d;
    for (int i=0;i<n;i++){
        cin>>a[i].k;
    }
    for (int i=0;i<n;i++){
        cin>>a[i].p;
    }
    sort(a,a+n,cmp);
    double ans=0;
    for (int i=0;i<n;i++){
        if (a[i].k>=d){
            ans+=(a[i].p/a[i].k)*d;
            break;
        }else{
            ans+=a[i].p;
            d-=a[i].k;
        }
    }
    printf("%.2f",ans);
    return 0;
}
```

贪心算法模板题，如果不知道什么是贪心就说明你基础不扎实了，建议退出文档补修贪心。

## 1021

```cpp
#include <bits/stdc++.h>
using namespace std;

string n;
int main(){
    cin>>n;
    int a[10];
    memset(a,0,sizeof(a));
    for (char c:n){
        a[c-'0']++;
    }
    for (int i=0;i<=9;i++){
        if (a[i]!=0){
            cout<<i<<":"<<a[i]<<endl;
        }
    }
    return 0;
}
```

## 1022

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int a,b,n,d;
signed main(){
    cin>>a>>b>>d;
    n=a+b;
    vector<int> ans;
    if (n==0){//记得特判
        cout<<0;
        return 0;
    }
    while (n>0){
        ans.push_back(n%d);
        n/=d;
    }
    for (int i=ans.size()-1;i>=0;i--){
        cout<<ans[i];
    }
    return 0;
}
```

模拟，如果不会进制转化，那么依旧建议是退出文档，补修进制转化相关知识。

## 1023

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> ans,a;
int x,minn=10;
int main(){
    int j=0;
    int t[10];
    memset(t,0,sizeof(t));
    while (j<10){
        cin>>x;
        t[j]=x;
        j++;
    }
    for (int i=0;i<=9;i++){
        if (t[i]==0){
            continue;
        }else{
            while (t[i]--){
                a.push_back(i);
            }
        }
    }
    int flag=0;
    for (auto it:a){
        if (it!=0){
            minn=min(it,minn);
        }
    }
    for (auto it:a){
        if (!flag&&it==minn){
            flag=1;
            continue;
        }else{
            ans.push_back(it);
        }
    }
    sort(ans.begin(),ans.end());
    cout<<minn;
    for (auto it:ans){
        cout<<it;
    }
    return 0;
}
```

需要注意一下它输入的数字是什么。

对于首位不能是0这个问题，我采取的方法是先找出这些数字中的非0最小值，然后维护另一个数组，为保证这个最小值只被去除一次，定义了flag变量进行标记，一旦去除，flag立刻重置为1，保证不会再去除第二个。

输出时只需要将非0最小值输出，紧跟着输出排序后的ans数组即可。

## 1024

```cpp
#include <bits/stdc++.h>
using namespace std;

string a, num, b;

int main() {
    cin >> a;
    // 处理数字部分（去掉 '+'）
    for (auto c : a) {
        if (c == 'E') break;
        if (c == '+') continue;
        num += c;
    }

    // 提取指数部分
    auto p = a.find('E');
    for (int i = p + 1; i < a.size(); i++) b += a[i];
    int x = stoi(b);

    // 输出符号
    if (num[0] == '-') {
        cout << "-";
        num = num.substr(1); // 去掉负号
    }

    // 分离整数部分和小数部分
    int dot = num.find('.');
    string digits = num.substr(0, dot) + num.substr(dot + 1);

    if (x >= 0) {
        // 小数点右移
        if (x >= (int)num.size() - dot - 1) {
            // 移到末尾或更右 → 直接输出并补0
            cout << digits;
            for (int i = 0; i < x - (num.size() - dot - 1); i++) cout << "0";
        } else {
            // 右移后还在中间 → 插入小数点
            int pos = dot + x; 
            cout << digits.substr(0, pos) << "." << digits.substr(pos);
        }
    } else {
        // 小数点左移
        cout << "0.";
        for (int i = 0; i < -x - 1; i++) cout << "0";
        cout << digits;
    }

    return 0;
}
```

字符串处理，很抽象，我不好说，我只看到了陈越满满的恶意

## 1025

```cpp
#include <bits/stdc++.h>
using namespace std;
class node{
    public:
    int add,data,next;
};
unordered_map<int,node> a;
int beginn,n,k;
node r;
deque<node> d;
int main(){
    cin>>beginn>>n>>k;
    while (n--){
       cin>>r.add>>r.data>>r.next;
       a[r.add]=r;
    }
    while(beginn!=-1){
        d.push_back(a[beginn]);
        beginn=a[beginn].next;
    }
    stack<node> s;
    vector<node> ans;
    while (!d.empty()){
        if (d.size()<k){
            while(!d.empty()){
                ans.push_back(d.front());
                d.pop_front();
            }
        }else{
            int x=k;
            while(x--){
                s.push(d.front());
                d.pop_front();
            }
            while(!s.empty()){
                ans.push_back(s.top());
                s.pop();
            }
        }
    }
    for (int i=0;i<(int)ans.size();i++){
        if (i!=ans.size()-1){
            ans[i].next=ans[i+1].add;
        }else{
            ans[i].next=-1;
        }
    }
    for (auto &it:ans){
        printf("%05d %d ",it.add,it.data);
        if (it.next != -1)
            printf("%05d\n",it.next);
        else
            printf("-1\n");
    }
    return 0;
}
```

前排提醒：如果现在还不知道链表是什么，搓不出来一条完整的单向链表/双向链表，那么你需要做的就是退出文档，补修链表相关知识

这一题我是受到了24年机考原题那题翻转链表的启发，然后劈里啪啦打了一大串代码，然后提交到PTA发生段错误，遂换方法。

链表类题目的基本套路就是把链表放到数组里面进行存储和处理，要注意的是，这题修改链表不是单纯的把链表结点结构体重新排序，排序完之后要修改链表每个节点的指向，这很重要。

至于链表地址的存储，我选择使用unordered_map，可以有效防止越界的出现，这个数据结构在哈希表中也有很广泛的应用。

反转的实现我采用了stack，利用先进后出的特性可以直接输出反转后结果。

然后就是修改节点指向，没什么好说的

这题写完之后可以在写一下PTA L2-022。

顺便附带上双端队列优化后的L2-022答案

至于优化在哪，时间还是空间，那你别管，更好理解那个过程了不是？

```cpp
#include <bits/stdc++.h>
using namespace std;
class node{
    public:
    int add,data,next;
};
unordered_map<int,node> m;
deque<node> d;
int tbegin,n;
int main(){
    cin>>tbegin>>n;
    node l;
    while(n--){
        cin>>l.add>>l.data>>l.next;
        m[l.add]=l;
    }
    vector<node> ans;
    while(tbegin!=-1){
        d.push_back(m[tbegin]);
        tbegin=m[tbegin].next;
    }
    while (!d.empty()) {
    if (d.size() >= 2) {
        ans.push_back(d.back());
        ans.push_back(d.front());
        d.pop_back();
        d.pop_front();
    } else { 
        ans.push_back(d.front());
        d.pop_front();
    }
    }
    for (int i=0;i<ans.size();i++){
        if (i==ans.size()-1){
            ans[i].next=-1;
        }else{
            ans[i].next=ans[i+1].add;
        }
    }
    for (auto it:ans){
        printf("%05d %d ",it.add,it.data);
        if (it.next != -1)
            printf("%05d\n",it.next);
        else
            printf("-1\n");
    }
    return 0;
}
```

## 1026

```cpp
#include <bits/stdc++.h>
using namespace std;

double c1,c2;

int main(){
    cin>>c1>>c2;
    double t1=(c2-c1)/100;
    int t2=(c2-c1)/100;
    double s1=t1-t2;
    if (s1>=0.5&&s1<1){
        t2++;
    }
    int hh=t2/3600;
    int mm=(t2%3600)/60;
    int ss=t2%60;
    printf("%02d:%02d:%02d",hh,mm,ss);
    return 0;
}
```

普通模拟，不讲思路，只需要注意要开头就四舍五入就行。

## 1027

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> a(1001);
int n;
char f;
int main(){
    cin>>n>>f;
    int num=3;
    a[0]=1;
    for (int i=1;i<=1000;i++){
        a[i]=num*2;
        num+=2;
    }
    vector<int> sum(1005);
    partial_sum(a.begin(),a.end(),sum.begin());//前缀和函数，用于快速求前缀和
    int c=0;//层数
    for (auto it:sum){
        if (it>n){
            break;
        }else{
            c++;
        }
    }
    int x=0,y=0;
    for (int i=c-1;i>=0;i--){
        if (i==0){
            x=((a[c-1]/2)-1)/2;
            while(x--){
                cout<<" ";
            }
            cout<<f<<endl;
        }else{
            x=((a[c-1]/2)-(a[i]/2))/2;
            while (x--){
                cout<<" ";
            }
            y=a[i]/2;
            while (y--){
                cout<<f;
            }
            cout<<endl;
        } 
    }
    for (int i=1;i<=c-1;i++){
        x=((a[c-1]/2)-(a[i]/2))/2;
            while (x--){
                cout<<" ";
            }
            y=a[i]/2;
            while (y--){
                cout<<f;
            }
            cout<<endl;
    }
    cout<<n-sum[c-1];
    return 0;
}
```

前缀和模板题，最丧心病狂的在于输出那个过程（是的我确实很讨厌模拟）

模板题没什么可讲的，不知道前缀和的去补修一下，补修完了绝对会做。

## 1028

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
class man{
    public:
    string name;
    int yyyy,mm,dd;
};
bool cmp(man x,man y){
    if (x.yyyy==y.yyyy){
        if (x.mm==y.mm){
            return x.dd<y.dd;
        }else{
            return x.mm<y.mm;
        }
    }else{
        return x.yyyy<y.yyyy;
    }
}
vector<man> add;

int main(){
    cin>>n;
    man r;
    int sum=0;
    while(n--){
        cin>>r.name;
        scanf("%d/%d/%d",&r.yyyy,&r.mm,&r.dd);
        if (r.yyyy>2014||r.yyyy<1814){
            continue;
        }else if(r.yyyy==2014&&r.mm>9){
            continue;
        }else if(r.yyyy==2014&&r.mm==9&&r.dd>6){
            continue;
        }else if(r.yyyy==1814&&r.mm<9){
            continue;
        }else if(r.yyyy==1814&&r.mm==9&&r.dd<6){
            continue;
        }else if(r.mm>12||r.dd>31){
            continue;
        }else{
            add.push_back(r);
            sum++;
        }
    }
    if (sum==0){
        cout<<0;
    }else{
        sort(add.begin(),add.end(),cmp);
        cout<<sum<<" "<<add[0].name<<" "<<add[add.size()-1].name;
    }
    
    return 0;
}
```

一题很简单的模拟，只需要对年龄判断下一点功夫就可以。

## 1029

```cpp
#include <bits/stdc++.h>
using namespace std;

string a,b;
unordered_set<char> used;

int main(){
    cin>>a>>b;
    for (int i=0;i<b.size();i++){
        b[i]=toupper(b[i]);
    }
    for (char c:a){
        char c1=toupper(c);
        if (b.find(c1)==string::npos&&!used.count(c1)){
            cout<<c1;
            used.insert(c1);
        }
    }
    return 0;
}
```

我又死在字符串处理上了。

这题用STL中的find就很好解答，然后建立一个集合就可以保证只被输出一次，但是陈越搞了个很抽象的东西，大小写一律只输出大写，这个没搞好会晕晕绕绕的。遂干脆把b里面全部转化为大写进行判断。

## 1030

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
int n,p;
vector<int> a;
signed main(){
    cin>>n>>p;
    int x;
    while (n--){
        cin>>x;
        a.push_back(x);
    }
    sort(a.begin(),a.end());
    int l=0;
    int ans=0;
    for (int r=0;r<a.size();){
        if (a[l]*p>=a[r]){
            ans=max(ans,r-l+1);
            r++;
        }else if(a[l]*p<a[r]){
            l++;
            if (l>r) r=l;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

这题主要涉及的算法是双指针，或许可以换一个说法叫做单调性枚举

思路非常简单，由于要选取最大值和最小值，我们直接采用一个排序，也就是sort。

sort函数默认是从小到大排序的，因此我们选定的一个区间内的左端点就是最小值，右端点就是最大值。

一旦满足条件，就说明最大值不够大了，将右指针向右边移动，同理，不满足情况就移动左指针。

这里有一个小细节需要注意一下啊，如果左指针超过右指针了，需要移动右指针到左指针位置

在每次满足的地方收集一下答案就可以，最后输出答案。

这里讲一个题外话啊，我刚开始看这一题的时候看成了要求一个固定区间，就是不让我打乱顺序了，洋洋洒洒写了个单调双端队列，然后直接爆0。

那都说到这了，就来写一下固定区间的吧，可以自己想一下。

贴上答案：

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,p;
deque<int> maxx;
deque<int> minn;

int main(){
    cin>>n>>p;
    int x;
    vector<int> a;
    while (n--){
        cin>>x;
        a.push_back(x);
    }
    int ans=0;
    int l=0;
    for (int r=0;r<a.size();r++){//注意，单调双端队列存储的是下标
        while(!maxx.empty()&&a[maxx.back()]<=a[r]){
            maxx.pop_back();
        }
        maxx.push_back(r);
        while(!minn.empty()&&a[minn.back()]>=a[r]){
            minn.pop_back();
        }
        minn.push_back(r);
        while(!maxx.empty()&&!minn.empty()&&a[minn.front()]*p<a[maxx.front()]){
            if (minn.front()==l){
                minn.pop_front();
            }
            if (maxx.front()==l){
                maxx.pop_front();
            }
            l++;
        }
        ans=max(ans,r-l+1);
    }
    cout<<ans<<endl;
    return 0;
}
```

这个不会也无所谓，因为单调双端队列结合双指针确实太难了。

## 1031

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
int n;
int q[17]={7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
char d[11]={'1','0','X','9','8','7','6','5','4','3','2'};
int main(){
    cin>>n;
    int cant=0;
    while (n--){
        cin>>a;
        int sum=0,flag=0;
        for (int i=0;i<17;i++){
            if ((a[i]>='A'&&a[i]<='Z')||(a[i]>='a'&&a[i]<='z')){
                flag=1;
            }else{
                int num=(a[i]-'0')*q[i];
                sum+=num;
            }
        }
        if (flag){
            cout<<a<<endl;
            cant++;
        }else{
            if (d[sum%11]==a[17]){
                continue;
            }else{
                cout<<a<<endl;
                cant++;
            }
        }
    }
    if (cant==0){
        cout<<"All passed"<<endl;
    }
    return 0;
}
```

简单模拟

## 1032

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
int a[100005];

int main(){
    cin>>n;
    int num,gpa;
    int m=n;
    memset(a,0,sizeof(a));
    while(m--){
        cin>>num>>gpa;
        a[num]+=gpa;
    }
    int maxx=a[1],maxnum=1;
    for (int i=2;i<=n;i++){
        if (a[i]>maxx){
            maxx=a[i];
            maxnum=i;
        }
    }
    cout<<maxnum<<" "<<maxx;
    return 0;
}//传统派数组解法
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    map<int, int> score;  // key=学校编号，value=总分
    int num, gpa;
    for (int i = 0; i < n; i++) {
        cin >> num >> gpa;
        score[num] += gpa;
    }

    int maxnum = 0, maxx = -1;
    for (auto &p : score) {
        if (p.second > maxx) {
            maxx = p.second;
            maxnum = p.first;
        }
    }

    cout << maxnum << " " << maxx;
    return 0;
}//非传统派map解法
```

数组解法容易踩坑，就是如果这些人全部都是0分呢。

## 1033

```cpp
#include <bits/stdc++.h>
using namespace std;

string a,b;
int main(){
    if (!getline(cin, a)) return 0;
    if (!getline(cin, b)) b = "";
    string word;
    int flag=0;//这个变量用来记录上档键坏掉了没1坏0好
    auto p=a.find('+');
    if (p!=string::npos){
        flag=1;
    }
    if (flag==1){
        for (char c:b){
            if (c>='A'&&c<='Z'){
                continue;
            }else if(c>='a'&&c<='z'){
                p=a.find(toupper(c));
                if (p!=string::npos){
                    continue;
                }else{
                    word+=c;
                }
            }else{
                p=a.find(c);
                if (p!=string::npos){
                    continue;
                }else{
                    word+=c;
                }
            }
        }
    }else{
        for (char c:b){
            char c1=toupper(c);
            p=a.find(c1);
            if (p!=string::npos){
                continue;
            }else{
                word+=c;
            }
        }
    }
    cout<<word;
    return 0;
}
```

思路我觉得应该都看得懂，我来说一个最大最毒瘤的地方

看到我代码的6和7行，为什么这里要这么写而不是用一般的cin呢？

很简单，因为题目有一个测试样例是没有坏键，这样的话，读入会发生很严重的错误，用getline就会解决这个问题。

> 这里我只看到了满满的恶意，我挂在这里了好几次，问GPT好几次它也没找出来，到最后才发现，可见这个点有多么刁钻。

## 1034

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Fraction {
    long long num, den; // 分子、分母
};

// 求最大公约数
long long gcd(long long a, long long b) {
    return b == 0 ? abs(a) : gcd(b, a % b);
}

// 约分 & 保证分母为正
Fraction reduction(Fraction f) {
    if (f.den < 0) { // 让分母始终为正
        f.den = -f.den;
        f.num = -f.num;
    }
    if (f.num == 0) {
        f.den = 1; // 0 的分母设为 1
        return f;
    }
    long long g = gcd(f.num, f.den);
    f.num /= g;
    f.den /= g;
    return f;
}

// 输出格式化分数
void printFraction(Fraction f) {
    f = reduction(f);
    bool negative = f.num < 0;
    if (negative) cout << "(";
    long long n = f.num, d = f.den;
    if (n % d == 0) {
        cout << n / d;
    } else if (abs(n) > d) {
        cout << n / d << " " << abs(n) % d << "/" << d;
    } else {
        cout << n << "/" << d;
    }
    if (negative) cout << ")";
}

// 加法
Fraction add(Fraction a, Fraction b) {
    Fraction r;
    r.num = a.num * b.den + b.num * a.den;
    r.den = a.den * b.den;
    return reduction(r);
}

// 减法
Fraction sub(Fraction a, Fraction b) {
    Fraction r;
    r.num = a.num * b.den - b.num * a.den;
    r.den = a.den * b.den;
    return reduction(r);
}

// 乘法
Fraction mul(Fraction a, Fraction b) {
    Fraction r;
    r.num = a.num * b.num;
    r.den = a.den * b.den;
    return reduction(r);
}

// 除法
Fraction divide(Fraction a, Fraction b) {
    Fraction r;
    r.num = a.num * b.den;
    r.den = a.den * b.num;
    return reduction(r);
}

int main() {
    string s1, s2;
    cin >> s1 >> s2;

    // 解析输入
    Fraction A, B;
    sscanf(s1.c_str(), "%lld/%lld", &A.num, &A.den);
    sscanf(s2.c_str(), "%lld/%lld", &B.num, &B.den);

    A = reduction(A);
    B = reduction(B);

    // A + B
    printFraction(A);
    cout << " + ";
    printFraction(B);
    cout << " = ";
    printFraction(add(A, B));
    cout << endl;

    // A - B
    printFraction(A);
    cout << " - ";
    printFraction(B);
    cout << " = ";
    printFraction(sub(A, B));
    cout << endl;

    // A * B
    printFraction(A);
    cout << " * ";
    printFraction(B);
    cout << " = ";
    printFraction(mul(A, B));
    cout << endl;

    // A / B
    printFraction(A);
    cout << " / ";
    printFraction(B);
    cout << " = ";
    if (B.num == 0) {
        cout << "Inf"; // 除数为 0
    } else {
        printFraction(divide(A, B));
    }
    cout << endl;

    return 0;
}
```

## 1035

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, x;
    cin >> n;
    vector<int> a, b;
    for (int i = 0; i < n; i++) {
        cin >> x;
        a.push_back(x);
    }
    for (int i = 0; i < n; i++) {
        cin >> x;
        b.push_back(x);
    }

    vector<int> t = a;

    // 插入排序模拟
    for (int i = 1; i < n; i++) { // i 表示正在插入第 i 个元素
        sort(t.begin(), t.begin() + i + 1);
        if (t == b) {
            sort(t.begin(), t.begin() + i + 2);
            cout << "Insertion Sort\n";
            for (int j = 0; j < n; j++) {
                if (j) cout << " ";
                cout << t[j];
            }
            return 0;
        }
    }

    // 归并排序模拟
    t = a;
    int s = 1;
    while (s < n) {
        for (int i = 0; i < n; i += 2 * s) {
            int r = min(i + 2 * s, n);
            sort(t.begin() + i, t.begin() + r);
        }
        if (t == b) {
            s *= 2;
            for (int i = 0; i < n; i += 2 * s) {
                int r = min(i + 2 * s, n);
                sort(t.begin() + i, t.begin() + r);
            }
            cout << "Merge Sort\n";
            for (int j = 0; j < n; j++) {
                if (j) cout << " ";
                cout << t[j];
            }
            return 0;
        }
        s *= 2;
    }
    return 0;
}
```

难点：什么是插入排序什么是归并排序。

插入排序好理解，这里重点介绍一下归并排序

> # 归并排序（特别是题目里常用的“自底向上”/迭代版本）详解
>
> 下面我用通俗的语言＋具体例子把归并排序每一步都拆开讲清楚，顺便说明 PAT B1035 里为什么要按 `s=1,2,4...` 这样检查中间状态，以及如何得到“下一步”的结果。
>
> ------
>
> ### 一、归并排序的核心思想（概念）
>
> 归并排序是分治（divide-and-conquer）：
>
> 1. 把数组拆成若干个有序的小段（初始每个段长度为 1，显然已排序）。
> 2. 把相邻的两个有序段合并成一个更长的有序段（长度翻倍）。
> 3. 重复第 2 步直到整段有序。
>
> 两种常见实现：
>
> - 递归（自顶向下）：把区间不断二分，底层合并上来。
> - 迭代/自底向上：从段长 `s=1` 开始，每轮把相邻两段（各长 s）合并成长度 `2s` 的段，s *= 2。
>
> PAT B1035 常用的是第二种（便于模拟并逐轮检查中间状态）。
>
> ------
>
> ### 二、合并（merge）具体做法
>
> 合并两个已排序的子数组 `L` 和 `R`（长度分别为 s 或可能不足 s）：
>
> - 用两个指针 `i, j` 指向 `L` 和 `R` 的起始位置，
> - 比较 `L[i]` 和 `R[j]`，把较小的放到结果中，移动对应指针，
> - 直到一个子数组耗尽，把另一个剩余部分直接追加到结果。
>
> 这个过程是稳定的，时间复杂度是 `O(len(L)+len(R))`。
>
> 实现上可以用手写合并（两个指针）或用 `std::inplace_merge` / `std::merge` / 甚至 `sort` 对那一整块做排序（但 `sort` 的语义更重，时间复杂度也高一点，但在 PAT 模拟中用 `sort` 对每个 2s 区间做排序也能得到相同的结果）。
>
> ------
>
> ### 三、逐轮（s=1,2,4,8...）示例（完整演示）
>
> 假设初始数组：
>
> ```
> A = [3, 1, 2, 8, 7, 5, 9, 4, 6, 0]
> ```
>
> **s = 1**（把每对长度为1的相邻块合并）
>  处理块对： [3] & [1], [2] & [8], [7] & [5], [9] & [4], [6] & [0]
>  合并后得到：
>
> ```
> after s=1: [1,3, 2,8, 5,7, 4,9, 0,6]
> ```
>
> **s = 2**（把每对长度为2的相邻块合并）
>  处理块对： [1,3] & [2,8] → [1,2,3,8]
>  [5,7] & [4,9] → [4,5,7,9]
>  [0,6] （尾段没有伴侣，保持 [0,6]）
>  合并后：
>
> ```
> after s=2: [1,2,3,8, 4,5,7,9, 0,6]
> ```
>
> **s = 4**（合并长度为4的块）
>  处理块对： [1,2,3,8] & [4,5,7,9] → [1,2,3,4,5,7,8,9]
>  [0,6] 仍为尾段
>  合并后：
>
> ```
> after s=4: [1,2,3,4,5,7,8,9, 0,6]
> ```
>
> **s = 8**（合并长度为8的块）
>  合并 [1..8] 和 [9..10]（即前 8 个与最后 2 个）：
>
> ```
> 最终排序: [0,1,2,3,4,5,6,7,8,9]
> ```
>
> > 注意：每一步（每次 s 倍增完成）你得到的数组就是一个“可观测的中间状态”。PAT 的 `B` 就可能等于其中某一轮结束后的数组。
>
> ------
>
> ### 四、如何在题目中判定和给出“下一步”结果（实战要点）
>
> - 在模拟时，每完成一轮（即把所有 `i` 从 `0` 到 `n` 的 `2*s` 块处理完），就把当前数组和给定的 `B` 比较。
> - 如果相等，说明 `B` 是在当前 `s` 完成后的状态。**接下来你需要把 `s` 翻倍（`s \*= 2`），再做一次完整的合并（对所有长度为 `2\*s` 的区间合并/排序）得到“下一步”的数组并输出。**
> - 特别注意：**不要在单个块处理完成后就比较**（题目要求快照是每轮结束后的状态）；也不要漏掉初始数组（`s=1` 之前的状态）这种边界。
>
> 举例流程（伪步骤）：
>
> 1. t = A，s = 1
> 2. 对所有 i (步长 2*s) 做合并 -> 得到 t
> 3. 如果 t == B：那说明当前是 `s` 的状态，令 s *= 2，做一轮合并得到 next，输出 next。
> 4. 否则 s *= 2，回到第2步，直到 s >= n。
>
> ------
>
> ### 五、代码提示（迭代合并，用 `inplace_merge` 更“正宗”）
>
> 下面是把上面逻辑写成的关键片段（在完整程序里用来检测并输出下一步）。`inplace_merge` 会把 `[l, mid)` 与 `[mid, r)` 两个已排序区间合并为一个已排序区间。
>
> ```cpp
> // t 为当前数组拷贝，n 为长度
> int s = 1;
> while (s < n) {
>     for (int i = 0; i < n; i += 2*s) {
>         int mid = min(i + s, n);
>         int r = min(i + 2*s, n);
>         if (mid < r) {
>             inplace_merge(t.begin() + i, t.begin() + mid, t.begin() + r);
>         }
>     }
>     if (t == B) {
>         // 找到匹配，执行下一轮
>         s *= 2;
>         for (int i = 0; i < n; i += 2*s) {
>             int mid = min(i + s, n);
>             int r = min(i + 2*s, n);
>             if (mid < r) inplace_merge(t.begin() + i, t.begin() + mid, t.begin() + r);
>         }
>         // 输出 t 即为下一步结果
>         break;
>     }
>     s *= 2;
> }
> ```
>
> > 说明：之前很多同学用 `sort(t.begin()+i, t.begin()+r)` 来代替 `inplace_merge`，结果在行为上等价（因为合并后那段是有序的），在 PAT 的题目下用 `sort` 也能通过，写法更简单，但 `inplace_merge` 更接近真实归并逻辑且更高效。
>
> ------
>
> ### 六、常见陷阱（注意点总结）
>
> - **比较时机**：必须在每轮合并结束后比较 `t` 和 `B`，不是在合并单个块中间比较。
> - **尾段长度**：当尾段不足 `s` 或不足 `2*s` 时也要正确处理（`mid=min(i+s,n)`， `r=min(i+2*s,n)`）。
> - **初始状态**：有可能 `B` 等于初始数组（还没做任何合并），要考虑这种情况（通常在循环前比较一次或在 s=1 的循环中就能捕获）。
> - **稳定性/相等判断**：务必用完整的数组比较（`t == B`），元素顺序必须完全一致。
>
> ------
>
> 如果你愿意，我可以：
>
> - 用你提供的具体 `A` 和 `B` 做一次可视化演示（逐轮列出数组），或者
> - 把上面的 `inplace_merge` 版本写成完整的、可直接提交的 C++ 程序（包含插入排序判定 + 归并判定），方便你直接测试。
>
> 你想要哪一个？

## 1036

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
char b;
int main(){
    cin>>n>>b;
    int h;
    if (n%2==0){
        h=n/2;
    }else{
        h=n/2+1;
    }
    vector<vector<char>> a(h,vector<char>(n));
    for (int i=0;i<h;i++){
        for (int j=0;j<n;j++){
            if (i==0||i==h-1){
                a[i][j]=b;
            }else{
                if (j==0||j==n-1){
                    a[i][j]=b;
                }else{
                    a[i][j]=' ';
                }
            }
        }
    }
    for (int i=0;i<h;i++){
        for (int j=0;j<n;j++){
            cout<<a[i][j];
        }
        cout<<endl;
    }
    return 0;
}
```

太简单了，没什么好说的。

## 1037

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
int pg,ps,pk;
int ag,as,ak;

signed main(){
    scanf("%lld.%lld.%lld %lld.%lld.%lld",&pg,&ps,&pk,&ag,&as,&ak);
    int ansg,anss,ansk;
    int sump=pk+ps*29+pg*29*17;
    int suma=ak+as*29+ag*29*17;
    int c=suma-sump;
    if (c<0){
        c=-c;
        ansg=c/493;
        anss=(c%493)/29;
        ansk=c%29;
        cout<<"-"<<ansg<<"."<<anss<<"."<<ansk;
    }else if(c>0){
        ansg=c/493;
        anss=(c%493)/29;
        ansk=c%29;
        cout<<ansg<<"."<<anss<<"."<<ansk;
    }else{
        cout<<"0.0.0"<<endl;
    }
    return 0;
}
```

简单模拟。

## 1038

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,score,k;
unordered_map<int,int> hash1;
int main(){
    cin>>n;
    while (n--){
        cin>>score;
        hash1[score]++;
    }
    cin>>k;
    for (int i=1;i<=k;i++){
        int finds;
        cin>>finds;
        if (i==k){
            cout<<hash1[finds];
        }else{
            cout<<hash1[finds]<<" ";
        }
    }
    return 0;
}
```

哈希表模板题，以分数为key，其实我觉得其他方法应该也可以是实现一样的效果。

但哈希表在极端数据下可能会更好用一点。

## 1039

```cpp
#include <bits/stdc++.h>
using namespace std;

string a,b;
int main(){
    getline(cin,a);
    getline(cin,b);
    unordered_map<char,int> hasha;
    unordered_map<char,int> hashb;
    int sum=0;
    bool flag=0;
    for (char c:a){
        hasha[c]++;
    }
    set<char> keyb;
    for (char c:b){
        hashb[c]++;
        keyb.insert(c);
    }
    for (auto c:keyb){
        if (hashb[c]>hasha[c]){
            flag=1;
        }
    }
    if (flag){
        for (auto c:keyb){
            if (hashb[c]>hasha[c]){
                sum+=hashb[c]-hasha[c];
            }
        }
    }else{
        sum=a.size()-b.size();
    }
    if (flag){
        cout<<"No "<<sum;
    }else{
        cout<<"Yes "<<sum;
    }
    return 0;
}
```

依旧哈希表，将字符作为key，字符的数量就是值

将目标串和给定串里面每个字符的个数都统计出来，然后对目标串的元素进行去重，这里我采用的方法是利用set的自动去重特性（set的底层结构是红黑树，会把里面的元素自动排序，这一点要注意，不过这一题是顺序无关的，所以也就无所谓了）

我们最开始去遍历这个集合中所有的字符，如果发现有不足的，立马将flag重置为1，然后进行下一步处理，统计到底有多少个不足，把这个结果加到sum里面，最后输出即可。

如果flag是0，那sum等于什么还要我说吗？

> 如果不想unordered_map的，可以试试看字符串哈希，当然这种方法我不推荐就是了，对这题有点太杀鸡焉用牛刀了。

## 1040

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;
    const long long MOD = 1000000007LL;
    long long cntP = 0, cntT = 0, ans = 0;

    // 统计所有 T
    for (char c : s) if (c == 'T') cntT++;

    // 一次遍历计算
    for (char c : s) {
        if (c == 'P') {
            cntP++;
        } else if (c == 'A') {
            ans = (ans + cntP * cntT) % MOD;
        } else if (c == 'T') {
            cntT--;
        }
    }

    cout << ans % MOD << '\n';
    return 0;
}
```

我想的搜索，但发现不对，这个不是找连续子串，后来想的栈，但发现又不对。

然后我放弃了。

问了一下GPT，他给我的思路是这样的：

> 先统计字符串中 `T` 的总数 `cntT`。
>
> 从左到右遍历：
>
> - 遇到 `P`：`cntP++`
> - 遇到 `A`：`ans += cntP * cntT`（模 `1e9+7`）
> - 遇到 `T`：`cntT--`
>
> 时间 O(n)，空间 O(1)。

纯数学题。

## 1041

```cpp
#include <bits/stdc++.h>
using namespace std;

map<int,int> a;
map<int,string> b;
int n,m,x,y;
string s;
int main(){
    cin>>n;
    while (n--){
        cin>>s>>x>>y;
        a[x]=y;
        b[x]=s;
    }
    cin>>m;
    while (m--){
        cin>>x;
        cout<<b[x]<<" "<<a[x]<<endl;
    }
    return 0;
}
```

一秒想map，其他方法没试过。

## 1042

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
map<char,int> m;

int main(){
    getline(cin,a);//注意，测试样例里面的句子有空格，所以要记得用getline，否则会输入错误
    for (char &c:a){
        c=tolower(c);
    }
    set<char> s;//会自动按照字典序排序，很好用吧
    for (char c:a){
        if (c>='a'&&c<='z'){
            s.insert(c);
        }
    }
    for (char c:a){
        if (c>='a'&&c<='z'){
            m[c]++;
        }
    }
    char ans;
    int maxx=0;
    for (auto c:s){
        if (m[c]>maxx){
            ans=c;
            maxx=m[c];
        }
    }
    cout<<ans<<" "<<m[ans];
    return 0;
}
```

## 1043

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
string res="PATest";

int main(){
    getline(cin,a);
    int sum=0;
    map<char,int> count;
    for (char c:a){
        count[c]++;
        if (c=='P'||c=='A'||c=='T'||c=='e'||c=='s'||c=='t'){
            sum++;
        }
    }
    while (sum>0){
        for (char c:res){
            if (count[c]!=0){
                cout<<c;
                count[c]--;
                sum--;
            }else{
                continue;
            }
        }
    }
    return 0;
}
```

我第一遍看错题了，以为是要把原字符串的字符提取出来然后还要保留原顺序。

当时在想，这东西怎么这么变态？

后来再看了一下题，发现也还好。

简单来说，我们是要疯狂去用PATest这三个字母去组成目标字符串，那我们就统计一下这几个字符的数量，并且要记得统计这几个字符串的总数量。

17行开始是这串代码最重要的部分，我设置了一个while循环，循环进行的内部条件是sum大于0，在内部，我们重复遍历目标串，一旦发现这个字符还可以添加，那我们就输出，同时减掉这个字符的数目以及总数目，当sum为0时就可以退出循环了。

涉及的算法？只是个模拟罢了。

## 1044

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<string> low={"tret","jan","feb","mar","apr","may","jun","jly","aug","sep","oct","nov","dec"};
    vector<string> high={"tam","hel","maa","huh","tou","kes","hei","elo","syy","lok","mer","jou"};
    unordered_map<string,int> mp;
    for(int i=0;i<13;i++) mp[low[i]]=i;
    for(int i=0;i<12;i++) mp[high[i]]=(i+1)*13;

    int N;cin>>N;cin.ignore();
    while(N--){
        string s;getline(cin,s);
        if(isdigit(s[0])){ // 数字 -> 火星文
            int x=stoi(s),hi=x/13,lo=x%13;
            if(hi&&lo) cout<<high[hi-1]<<" "<<low[lo]<<"\n";
            else if(hi) cout<<high[hi-1]<<"\n";
            else cout<<low[lo]<<"\n";
        }else{ // 火星文 -> 数字
            stringstream ss(s);string a,b;ss>>a;
            int x=mp[a];
            if(ss>>b) x+=mp[b];
            cout<<x<<"\n";
        }
    }
}

```

题目很简单，但你知道的，PAT乙级思路基础，那写起来就不基础

这个东西就是要建立四个映射，两个用数组，两个用map，然后就很无聊了

这里讲一下20行的stringstream函数，这个可以快速拆分空格，有点好用，当然其实自己手搓也是可以。

用法如下：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s = "123 456 abc";
    stringstream ss(s); // 把 s 放进 ss

    int a, b;
    string c;
    ss >> a >> b >> c; // 像 cin 一样读
    cout << a << " " << b << " " << c; // 123 456 abc
}
```

**关键点：**

- `stringstream ss(s)` 就像把字符串 `s` 变成一个 `cin`，可以用 `>>` 依次读取。
- 自动按空格、换行分割，不用自己写 `split`。

> 我要吐槽啊，陈越真的很喜欢思路基础code起来不基础的那种题，反正体验感不是很好（小声bb）

## 1045

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<int> num;

int main(){
    cin>>n;
    int x;
    for (int i=1;i<=n;i++){
        cin>>x;
        num.push_back(x);
    }
    vector<int> uppp(n);
    vector<int> loww(n);
    stack<int> s1,s2;
    for (int i=0;i<n;i++){
        while(!s1.empty()&&num[s1.top()]<=num[i]){
            s1.pop();
        }
        uppp[i]=s1.empty()?-1:s1.top();
        s1.push(i);
    }
    for (int i=n-1;i>=0;i--){
        while(!s2.empty()&&num[s2.top()]>=num[i]){
            s2.pop();
        }
        loww[i]=s2.empty()?-1:s2.top();
        s2.push(i);
    }
    vector<int> ans;
    for (int i=0;i<n;i++){
        if (loww[i]==-1&&uppp[i]==-1){
            ans.push_back(num[i]);
        }
    }
    sort(ans.begin(),ans.end());
    cout<<ans.size()<<endl;
    for (int i=0;i<ans.size();i++){
        if (i) cout<<" ";
        cout<<ans[i];
    }
    cout<<endl;
    return 0;
}
```

这题还是很有意思的。

我看到这一题的第一反应是单调栈，第二反应是前后缀最大最小值。

不知道单调栈的下面几行可以不用看了，等下我会写一篇前后缀最大最小值的题解

由于我前几天刚刚温习了一下单调栈，就用单调栈写了，实际上前后缀的做法是更好理解的。

如果这个元素满足条件，那么在它前面一定没有比它大的元素，在它的后面一定没有比他小的元素，那么很简单，单调栈就呼之欲出了。

单调栈算法比较常见的应用场景是寻找左右边第一个大于/小于自己的元素，并且在另一个数组容器中存储下标。而这一题我们也去找找。如果能找到，就存他的下标，找不到就把这个值设定为-1。

最后我们去遍历数组中元素，如果他的两个单调栈数组值都是-1，那么就说明这个元素符合条件，存入ans数组（这里我一定要吐槽啊，其实可以直接输出的，但是PAT天天都要求最后一个元素不能有空格）

然后输出ans数组就可以了。

小心有一个测试点很毒瘤，会报格式错误，我找了半天格式问题。

前后缀和最大最小值解：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);//这两行可以不用在意
    int n; if(!(cin>>n)) return 0;
    vector<int> a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    vector<int> leftMax(n), rightMin(n);
    leftMax[0]=a[0];
    for(int i=1;i<n;i++) leftMax[i]=max(leftMax[i-1], a[i]);
    rightMin[n-1]=a[n-1];
    for(int i=n-2;i>=0;i--) rightMin[i]=min(rightMin[i+1], a[i]);

    vector<int> ans;
    for(int i=0;i<n;i++){
        if ( (i==0 || a[i] >= leftMax[i-1]) && (i==n-1 || a[i] <= rightMin[i+1]) )
            ans.push_back(a[i]);
    }
    sort(ans.begin(), ans.end());
    cout << ans.size() << "\n";
    for (int i=0;i<ans.size();i++){
        if (i) cout << " ";
        cout << ans[i];
    }
    cout << "\n";
    return 0;
}
```

这个的做法也相当简单了，就是找到左右边的最大最小值，然后判断条件就行了，和leetcode那题接雨水有点像。

我是推荐写第二种方法的，当然第一种方法也是不错的，更有“算法竞赛”味不是？

单调栈win在复用性和模板性更强，前缀和win在更好理解。

## 1046

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,numa,numb,ha,hb,wina=0,winb=0;

int main(){
    cin>>n;
    while (n--){
        cin>>numa>>ha>>numb>>hb;
        int sum=numa+numb;
        if (sum==ha&&sum!=hb){
            wina++;
        }else if(sum==hb&sum!=ha){
            winb++;
        }else{
            continue;
        }
    }
    cout<<winb<<" "<<wina;
    return 0;
}
```

这个不会做抓出去打，放在洛谷最多红题。

## 1047

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,team,man,score;
int main(){
    cin>>n;
    unordered_map<int,int> sum;
    set<int> key1;
    while(n--){
        scanf("%d-%d %d",&team,&man,&score);
        sum[team]+=score;
        key1.insert(team);
    }
    int maxx=0,maxteam;
    for (auto it:key1){
        if (sum[it]>maxx){
            maxx=sum[it];
            maxteam=it;
        }
    }
    cout<<maxteam<<" "<<maxx;
    return 0;
}
```

关于map类的应用我已经懒得赘述了，前面考了那么多应该要会了。

## 1048

```cpp
#include <bits/stdc++.h>
using namespace std;

string a, b;
char s[15] = {'0','1','2','3','4','5','6','7','8','9','J','Q','K'};

int main() {
    cin >> a >> b;

    // 右对齐：补齐长度
    while (a.size() < b.size()) a = "0" + a;
    while (b.size() < a.size()) b = "0" + b;

    int x, y, n;
    int len = b.size();

    for (int i = len - 1, k = 1; i >= 0; i--, k++) {  // k 从 1 开始，表示第 k 位（从右往左数）
        x = b[i] - '0';
        y = a[i] - '0';
        if (k % 2 == 1) { // 奇数位
            n = (x + y) % 13;
            b[i] = s[n];
        } else {          // 偶数位
            n = x - y;
            if (n < 0) n += 10;
            b[i] = s[n];
        }
    }

    cout << b << endl;
    return 0;
}
```

注意一下a和b不一样长的问题。

## 1049

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long n;
    cin>>n;
    long double sum=0;
    for (long long i=0; i<n; i++) {
        long double x;
        cin>>x;
        sum+=x*(long double)(i+1)*(n-i);
    }
    printf("%.2Lf\n",sum); // 末尾加换行避免格式错误
    return 0;
}
```

我想，一定有小聪明蛋一开始想着前缀和，然后写出一个前缀和加双向遍历，喜提15分挂了两个测试点

因为时间复杂度n2了啊还能因为什么，自己想想是不是每个元素都被遍历了两次（

但这题本质上是数学题。

具体思路自己ai一下，我打不出公式

## 1050

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int main(){
    cin>>n;
    vector<int> a(n);
    for (int i=0;i<n;i++){
        cin>>a[i];
    }
    sort(a.begin(),a.end(),greater<int>());
    int x=1,y=n,j,minn=10000;
    for (int i=1;i*i<=n;i++){
        if (n%i==0){
            j=n/i;
            if (j-i<minn){
                x=i;
                y=j;
                minn=j-i;
            }
        }
    }
    int left=0,right=x-1,topp=0,bottom=y-1;
    int cur=0;
    vector<vector<int>> ans(y,vector<int>(x));
    while (topp<=bottom&&left<=right){
        for (int i=left;i<=right;i++){
            ans[topp][i]=a[cur];
            cur++;
        }
        ++topp;
        for (int i=topp;i<=bottom;i++){
            ans[i][right]=a[cur];
            cur++;
        }
        --right;
        if (topp<=bottom){
            for (int i=right;i>=left;i--){
                ans[bottom][i]=a[cur];
                cur++;
            }
            --bottom;
        }
        if (left<=right){
            for (int i=bottom;i>=topp;i--){
                ans[i][left]=a[cur];
                cur++;
            }
            left++;
        }
    }
    for (int i=0;i<y;i++){
        for (int j=0;j<x;j++){
            if (j==x-1){
                cout<<ans[i][j];
            }else{
                cout<<ans[i][j]<<" ";
            }
        }
        cout<<endl;
    }
    return 0;
}
```

螺旋矩阵，数组类模拟最长的河最高的山，在2014年的NOIP普及组T3就是这个

螺旋矩阵事实上有一个用DFS的做法，但是别想那个了，那个包超时的，我以前练搜索的时候写过一次，递归层数太高了（等下下面贴上洛谷螺旋矩阵我的DFS做法）

为什么用模拟，很简单，模拟是最优算法。

这一题还和普通的螺旋矩阵不一样，它顺带要自己求长和宽。我们直接预设长和宽，然后从1开始到sqrt（n）进行遍历，这样子做可以确保我们用于遍历的变量i始终是相对小的那一个，然后用常规方法就可以求出长和宽了。

由于PTA的神必测评机，我特别喜欢把数据先存到数组里面然后再统一输出答案，然后看看螺旋矩阵最核心的部分，我们建立一个while循环，并且确定这个矩阵左右上下边界，先从最上面从左到右，接着再从上到下。每一次这个过程结束我们都需要剥去已经被填充了数字的那一层，即类似于我31行和36行的写法。最后输出答案。

## 1051

```cpp
#include <bits/stdc++.h>
using namespace std;

double r1,p1,r2,p2;
int main(){
    cin>>r1>>p1>>r2>>p2;
    double r=r1*r2;
    double p=p1+p2;
    double a=r*cos(p);
    double b=r*sin(p);
    if (fabs(a)<0.005) a=0;
    if (fabs(b)<0.005) b=0;//注意浮点误差
    
    if (b<0){
        b=-b;
        printf("%.2f-%.2fi",a,b);
    }else{
        printf("%.2f+%.2fi",a,b);
    }
    return 0;
}
```

题目只有一个坑点，但是这个坑点覆盖了两个测试点，如果不注意你将失去两分。

浮点误差，很重要。

## 1052

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<string> hand, eyes, mouth;
string s;
int n, a, b, c, d, e;

void parse(vector<string> &v) {
    string word;
    for (int i = 0; i < (int)s.size(); i++) {
        if (s[i] == '[') {
            word.clear();
            bool found = false;
            for (int j = i + 1; j < (int)s.size(); j++) { // ✅ 加上 j < s.size()
                if (s[j] == ']') {
                    v.push_back(word);
                    i = j;  // ✅ 跳过已经处理的区间，避免重复
                    found = true;
                    break;
                } else {
                    word += s[j];
                }
            }
            if (!found) break; // ✅ 没找到 ']' 直接退出，避免死循环
        }
    }
}

int main() {
    getline(cin, s);
    parse(hand);
    getline(cin, s);
    parse(eyes);
    getline(cin, s);
    parse(mouth);

    cin >> n;
    while (n--) {
        cin >> a >> b >> c >> d >> e;
        // ✅ 增加下界检查，避免 0 和负数导致崩溃
        if (a < 1 || b < 1 || c < 1 || d < 1 || e < 1 ||
            a > (int)hand.size() || b > (int)eyes.size() ||
            c > (int)mouth.size() || d > (int)eyes.size() ||
            e > (int)hand.size()) {
            cout << "Are you kidding me? @\\/@\n";
        } else {
            cout << hand[a - 1] << "(" << eyes[b - 1] << mouth[c - 1] << eyes[d - 1] << ")" << hand[e - 1] << "\n";
        }
    }
    return 0;
}
```

最难的地方在于把那几个符号全部搓出来，然后没了。

## 1053

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, d;
    double e;
    cin>>n>>e>>d;
    int maybe=0, must=0;
    for (int i=0; i<n;i++) {
        int k;
        cin>>k;
        int day=0;
        for (int j=0;j<k;j++) {
            double ei;
            cin>>ei;
            if (ei<e) day++;
        }
        if (k<=d&&day>k/2.0) {
            maybe++;
        } else if (k>d&&day>k/2.0) { 
            must++;
        }
    }
    printf("%.1f%% %.1f%%",maybe*100.0/n,must*100.0/n);
    return 0;
}
```

## 1054

```cpp
#include <bits/stdc++.h>
using namespace std;

int n; string num;
bool isnum(string a){
    try{
        size_t idx;
        stod(a,&idx);
        if(idx!=a.size()) return false;
    }catch(...){ return false; }
    int dot=0;
    for(char c:a) if(c=='.') dot++;
    if(dot>1) return false;
    size_t pos=a.find('.');
    if(pos!=string::npos && a.size()-pos-1>2) return false;
    double val=stod(a);
    return !(val<-1000||val>1000);
}

int main(){
    cin>>n;
    double sum=0; int m=0;
    for(int i=0;i<n;i++){
        cin>>num;
        if(isnum(num)) sum+=stod(num),m++;
        else cout<<"ERROR: "<<num<<" is not a legal number"<<endl;
    }
    if(m==0) cout<<"The average of 0 numbers is Undefined\n";
    else if(m==1) printf("The average of 1 number is %.2f\n",sum);
    else printf("The average of %d numbers is %.2f\n",m,sum/m);
}
```

依旧字符串模拟

讲一下那个try怎么用的

> 你上一题的代码里 `try` 的作用就是**安全地用 `stod` 把字符串转换成数字**，防止程序崩溃。
>
> 我们看一下关键部分：
>
> ```cpp
> bool isnum(string a) {
>     try {
>         size_t idx;
>         stod(a, &idx);
>         if (idx != a.size()) return false; // 有多余字符
>     } catch (...) {  // ← 这里就是 try 的作用
>         return false;
>     }
>     ...
>     return true;
> }
> ```
>
> 🔎 **执行流程：**
>
> 1. `stod(a, &idx)` 试图把字符串 `a` 转成 `double`。
>
> 2. 如果 `a` 是非法字符串（比如 `"abc"`, `"12..3"`, `"1e9999"`），`stod` 会**抛出异常**：
>
>    - `std::invalid_argument`（无法转换，比如 `abc`）
>    - `std::out_of_range`（数字太大，比如 `1e9999`）
>
> 3. `try` 会捕获到异常，不让程序崩溃，立刻跳到 `catch(...)` 执行：
>
>    ```cpp
>    catch (...) { return false; }
>    ```
>
>    表示这个字符串不合法。
>
> 如果没有 `try/catch`，遇到非法字符串时程序会直接终止（运行时错误）。
>
> ------
>
> ✅ **简化理解：**
>
> - `try` = 让程序“试一试”执行里面的代码
> - 如果成功就继续往下走
> - 如果失败（抛异常），就去 `catch` 执行，把错误“接住”
> - 不会让程序崩溃

## 1055

```cpp
#include <bits/stdc++.h>
using namespace std;

class stu{
    public:
    int h;
    string name;
};
bool cmp(stu x,stu y){
    if (x.h==y.h){
        return x.name<y.name;
    }else{
        return x.h>y.h;
    }
}
int n,k;
int main(){
    cin>>n>>k;
    vector<stu> a(n);
    for (int i=0;i<n;i++){
        cin>>a[i].name>>a[i].h;
    }
    sort(a.begin(),a.end(),cmp);
    int aft=n%k+n/k;
    deque<string> d;
    int flag=0,cur=0;
    while (d.size()<aft){
        if (flag==0){
            d.push_back(a[cur].name);
            flag=1;
            cur++;
        }else{
            d.push_front(a[cur].name);
            flag=0;
            cur++;
        }
    }
    vector<string> after;
    for (auto it:d){
        after.push_back(d.front());
        d.pop_front();
    }
    for (int i=0;i<after.size();i++){
        if (i==after.size()-1){
            cout<<after[i]<<endl;
        }else{
            cout<<after[i]<<" ";
        }
    }
    d.clear();
    while (cur<n){
        vector<string> ans;
        int flag1=0;
        while (d.size()<n/k){
            if (flag1==0){
                d.push_back(a[cur].name);
                cur++;
                flag1=1;
            }else{
                d.push_front(a[cur].name);
                cur++;
                flag1=0;
            }
        }
        for (auto it:d){
            ans.push_back(d.front());
            d.pop_front();
        }
        for (int i=0;i<ans.size();i++){
            if (i==ans.size()-1){
                cout<<ans[i]<<endl;
            }else{
                cout<<ans[i]<<" ";
            }
        }
        ans.clear();
        d.clear();
    }
    return 0;
}
```

双端队列最王朝的一集，还有人质疑吗？

我们把最特殊的最后一排单独拉出来处理，最后一排的计算公式是n%k+n/k，如果n可以被k整除，那么每排就是n/k人，如果不能，我们就把余数的那些人（n%k）加上去，也就是最后得到了n%k+n/k。

依次读入每个学生类的名字与身高，再写一个cmp函数用来对这些同学进行排序（其实这里可以用匿名函数的写法，不过那样子写会让代码可读性降低）

先处理最后一排的人，我们让排序后数组的元素依次交错入列，这样子就可以保证类似“山”形的排列，一旦队列中的元素数量等于我们预先设定好的人数，那么就直接退出。

其实这里可以直接遍历队列输出的，但你知道的，PTA的神必测评机对最后一个数据的空格要求比较严苛，所以我这里还是另外开了一个数组用来输出答案。

处理完最后一排，应该可以发现我预先设定的cur变量已经指向了非最后一排的第一个元素，那么接下来处理常规情况，建立一个while循环，当cur<n的时候持续进行。在循环内部，设置一个答案数组，以及原本的双端队列，然后处理方式和上面一样，只不过要记得使用完清理掉数组和队列中的元素。

## 1056

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int main(){
    cin>>n;
    vector<int> a(n);
    for (int i=0;i<n;i++){
        cin>>a[i];
    }
    int u=1<<n;
    int sum=0;
    set<int> s;
    for (int i=0;i<u;i++){
        if (__builtin_popcount(i)==2){//这个内层的意思是这个二进制数里有几个1.
            vector<int> b;
            for (int j=0;j<n;j++){
                if (i&(1<<j)){
                    b.push_back(a[j]);
                }
            }
            int x=10*b[0]+b[1];
            int y=10*b[1]+b[0];
            if (s.find(x)==s.end()&&s.find(y)==s.end()){
                if (x==y){
                    sum+=x;
                    s.insert(x);
                }else{
                    sum+=(x+y);
                    s.insert(x);
                    s.insert(y);
                }
            }
        }
    }
    cout<<sum<<endl;
    return 0;
}
```

选数类问题是一系列比较抽象的问题，在不同的数据范围内有不同的解法，例如这一题最优的解法其实应该是双重循环，但我用了位掩码的做法只是想练一下这个写法。

一般而言，在选的数数量非常小的情况下，循环是更快的，虽然很笨，但真的很快。但是一旦要选取的数数量变多了，循环就会显的非常繁琐，这时候位掩码的作用就出来了。

事实上，位掩码也有很大的局限性，例如在总共数数量大于20，那么就会立刻超时，适用范围是总数少，但要选的数数量多。

| n 的规模              | k 的规模   | 最优解法建议                                                 |
| --------------------- | ---------- | ------------------------------------------------------------ |
| **n ≤ 20**            | 任意 k     | **位掩码枚举**，复杂度 O(2^n · n)，暴力可行                  |
| **n ≤ 30, k 很小**    | k ≤ 5~6    | **回溯/DFS 枚举组合**，复杂度 O( C(n,k) · k )                |
| **n 很大 (n ≥ 10^5)** | k=2 或 k=3 | **双重/三重循环** 或者 **数学公式**，复杂度 O(n²) 或 O(n³)，注意优化 |
| **n 很大 (n ≥ 10^5)** | k 不固定   | 需要 **动态规划 / 前缀和 / 状态压缩 DP**，避免显式枚举子集   |

所以这题其实挺有意思的，分数虽然不多，但是可以引申出一类题目。

## 1057

```cpp
#include <bits/stdc++.h>
using namespace std;

string a;
int main(){
    getline(cin,a);
    int sum=0;
    for (char c:a){
        if (c>='a'&&c<='z'){
            sum+=c-'a'+1;
        }else if(c>='A'&&c<='Z'){
            sum+=c-'A'+1;
        }
    }
    if (sum!=0){
    int ans1=0,ans0=0;
    while (sum>0){
        int t=sum%2;
        if (t==1){
            ans1++;
        }else{
            ans0++;
        }
        sum/=2;
    }
    cout<<ans0<<" "<<ans1;
    }else{
        cout<<0<<" "<<0;
    }
    return 0;
}
```

## 1058

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int N, M;
    if (!(cin >> N >> M)) return 0;

    vector<int> score(M);
    vector< set<char> > correct(M);
    for (int i = 0; i < M; ++i) {
        int full, optCnt, corCnt;
        cin >> full >> optCnt >> corCnt;
        score[i] = full;
        for (int j = 0; j < corCnt; ++j) {
            char c; cin >> c;
            correct[i].insert(c);
        }
    }

    // 读取到行尾，准备读取学生每一行
    string line;
    getline(cin, line); // 吃掉当前行末的换行

    vector<int> wrong_cnt(M, 0);

    for (int stu = 0; stu < N; ++stu) {
        getline(cin, line);
        int total = 0;

        // 逐题解析：按每个 '(' ... ')' 块解析
        int idx = 0; // 当前题目索引
        int i = 0;
        int L = (int)line.size();
        while (i < L && idx < M) {
            // 寻找 '('
            while (i < L && line[i] != '(') ++i;
            if (i >= L) break;
            ++i; // 跳过 '('

            // 读取选中个数（整数，可能多位）
            while (i < L && line[i] == ' ') ++i;
            int cnt = 0;
            while (i < L && isdigit(line[i])) {
                cnt = cnt * 10 + (line[i] - '0');
                ++i;
            }

            // 读取 cnt 个选项字母
            set<char> chosen;
            for (int t = 0; t < cnt; ++t) {
                while (i < L && !isalpha(line[i])) ++i;
                if (i < L && isalpha(line[i])) {
                    chosen.insert(line[i]);
                    ++i;
                }
            }

            // 跳到 ')' （或者继续）
            while (i < L && line[i] != ')') ++i;
            if (i < L && line[i] == ')') ++i;

            // 判分：全部正确才给分
            if (chosen == correct[idx]) {
                total += score[idx];
            } else {
                wrong_cnt[idx] += 1;
            }

            ++idx; // 处理下一题
        }

        cout << total << "\n";
    }

    // 统计错得最多的题
    int max_wrong = 0;
    for (int i = 0; i < M; ++i) max_wrong = max(max_wrong, wrong_cnt[i]);

    if (max_wrong == 0) {
        cout << "Too simple\n";
    } else {
        // 收集所有达到最大错题数的题号（1-based）
        vector<int> ans;
        for (int i = 0; i < M; ++i) if (wrong_cnt[i] == max_wrong) ans.push_back(i + 1);
        // 输出格式：次数 + 空格 + 题号列表
        cout << max_wrong;
        for (int x : ans) cout << " " << x;
        cout << "\n";
    }

    return 0;
}
```

很无趣，纯粹的大模拟，就单纯恶心你来的，你看我用AI你就知道了。

出题人超级喜欢字符串分割，我已经无力去喷了。

## 1059

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,k,mm;
string a;
bool isprime(int x){
    if (x==1) return false;
    if (x==2) return true;
    else{
        for (int i=2;i*i<=x;i++){
            if (x%i==0){
                return false;
            }
        }
    }
    return true;
}
int main(){
    cin>>n;
    vector<string> add(n);
    set<string> s;
    unordered_map<string,int> m;
    unordered_map<string,string> ans;
    for (int i=0;i<n;i++){
        cin>>add[i];
        s.insert(add[i]);
        m[add[i]]++;
        if (i==0){
            ans[add[i]]="Mystery Award";
        }else if(isprime(i+1)){
            ans[add[i]]="Minion";
        }else{
            ans[add[i]]="Chocolate";
        }
    }
    cin>>mm;
    for (int i=0;i<mm;i++){
        cin>>a;
        if (s.find(a)==s.end()){
            cout<<a<<": Are you kidding?"<<endl;
        }else if(s.find(a)!=s.end()&&m[a]>1){
            cout<<a<<": Checked"<<endl;
        }else if(s.find(a)!=s.end()&&m[a]==1){
            m[a]++;
            cout<<a<<": "<<ans[a]<<endl;
        }
    }
    return 0;
}
```

## 1060

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int l,r,ans=0;
vector<int> a;
bool can(int m){
    int sum=0;
    for (int i=0;i<n;i++){
        if (a[i]>m){//我头几次这里写成>=了没发现，改了半天才找到。
            sum++;
        }
    }
    if (sum>m){
        return 1;
    }else{
        return 0;
    }
}
int main(){
    cin>>n;
    for (int i=0;i<n;i++){
        int x;
        cin>>x;
        a.push_back(x);
    }
    sort(a.begin(),a.end());//多余的，我拿来调试用的。
    l=0;
    r=100010;
    while (l<=r){
        int mid=(l+r)/2;
        if (can(mid)){
            l=mid+1;
            ans=mid;
        }else{
            r=mid-1;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

纯粹的二分答案，极致的享受。

我们找到上下界，然后进行二分答案，写一个函数来判断这个答案能否满足结果，如果可以，修改边界收集答案，如果不行，再修改边界。循环结束就是答案。

如果对STL函数熟悉的应该可以知道，这题还有一个更加超标的解法那就是lower_bound与upper_bound函数

> # `lower_bound` 与 `upper_bound` 函数详解
>
> 在 C++ 标准库中，`lower_bound` 和 `upper_bound` 是两个非常实用的二分查找算法函数，它们都定义在 `<algorithm>` 头文件中。这两个函数用于在**已排序**的序列中查找特定值的位置。
>
> ## 1. `lower_bound`
>
> ### 功能：
>
> 在有序序列中查找第一个**大于或等于**目标值的元素位置。
>
> ### 函数原型：
>
> ```
> template <class ForwardIterator, class T>
> ForwardIterator lower_bound(ForwardIterator first, ForwardIterator last, const T& val);
> ```
>
> ### 参数：
>
> - `first`, `last`：前向迭代器，表示要查找的范围（左闭右开区间）
> - `val`：要查找的目标值
>
> ### 返回值：
>
> - 返回指向序列中第一个**大于或等于**`val` 的元素的迭代器
> - 如果所有元素都小于`val`，则返回`last`
>
> ### 示例：
>
> ```
> vector<int> v = {1, 3, 3, 5, 7, 9};
> 
> // 查找第一个 >= 3 的元素
> auto it = lower_bound(v.begin(), v.end(), 3); // 指向第一个3（索引1）
> 
> // 查找第一个 >= 4 的元素
> it = lower_bound(v.begin(), v.end(), 4); // 指向5（索引3）
> 
> // 查找第一个 >= 10 的元素
> it = lower_bound(v.begin(), v.end(), 10); // 返回 v.end()
> ```
>
> ## 2. `upper_bound`
>
> ### 功能：
>
> 在有序序列中查找第一个**大于**目标值的元素位置。
>
> ### 函数原型：
>
> ```
> template <class ForwardIterator, class T>
> ForwardIterator upper_bound(ForwardIterator first, ForwardIterator last, const T& val);
> ```
>
> ### 参数：
>
> 同 `lower_bound`
>
> ### 返回值：
>
> - 返回指向序列中第一个**大于**`val` 的元素的迭代器
> - 如果所有元素都小于或等于`val`，则返回`last`
>
> ### 示例：
>
> ```
> vector<int> v = {1, 3, 3, 5, 7, 9};
> 
> // 查找第一个 > 3 的元素
> auto it = upper_bound(v.begin(), v.end(), 3); // 指向5（索引3）
> 
> // 查找第一个 > 5 的元素
> it = upper_bound(v.begin(), v.end(), 5); // 指向7（索引4）
> 
> // 查找第一个 > 9 的元素
> it = upper_bound(v.begin(), v.end(), 9); // 返回 v.end()
> ```
>
> ## 3. 关键区别
>
> | 特性         | `lower_bound`                | `upper_bound`                    |
> | ------------ | ---------------------------- | -------------------------------- |
> | 查找条件     | 第一个**大于或等于**目标值   | 第一个**大于**目标值             |
> | 对于重复元素 | 指向第一个匹配元素           | 指向最后一个匹配元素的下一个位置 |
> | 返回值关系   | `lower_bound <= upper_bound` | `lower_bound <= upper_bound`     |
>
> ## 4. 实用技巧
>
> ### 统计特定值出现的次数：
>
> ```
> vector<int> v = {1, 3, 3, 3, 5, 7};
> int target = 3;
> 
> auto low = lower_bound(v.begin(), v.end(), target);
> auto up = upper_bound(v.begin(), v.end(), target);
> 
> int count = distance(low, up); // 3
> ```
>
> ### 在有序序列中插入元素：
>
> ```
> vector<int> v = {1, 3, 5, 7};
> int new_val = 4;
> 
> // 找到第一个 >= 4 的位置
> auto pos = lower_bound(v.begin(), v.end(), new_val);
> v.insert(pos, new_val); // v = {1, 3, 4, 5, 7}
> ```
>
> ### 检查元素是否存在：
>
> ```
> vector<int> v = {1, 3, 5, 7};
> int target = 5;
> 
> // 检查元素是否存在
> bool exists = binary_search(v.begin(), v.end(), target); // true
> 
> // 等价于：
> bool exists2 = (lower_bound(v.begin(), v.end(), target) != v.end() && 
>                *lower_bound(v.begin(), v.end(), target) == target); // true
> ```
>
> ## 5. 性能特点
>
> - **时间复杂度**：O(log n)，其中 n 是序列长度
> - **前提条件**：序列必须是有序的（通常是非降序）
> - **优势**：比线性查找高效，特别适合大型数据集
>
> ## 6. 在爱丁顿数问题中的应用
>
> 在爱丁顿数问题中，我们需要统计大于某个值 E 的天数：
>
> ```
> bool can(int m) {
>     // 使用 upper_bound 找到第一个大于 m 的元素位置
>     auto it = upper_bound(a.begin(), a.end(), m);
>     int sum = a.end() - it; // 计算大于 m 的元素数量
>     return sum >= m;
> }
> ```
>
> 这种方法比遍历整个数组更高效，时间复杂度从 O(n) 降为 O(log n)。

这里打个岔，我刚开始写了二分答案但是挂了三个测试点，我都开始怀疑自己了二分答案怎么还能错，想了各种原因后面才发现

我>写成>=了……题没审好。

## 1061

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,m;
vector<int> score;

int main(){
    cin>>n>>m;
    for (int i=0;i<m;i++){
        int x;
        cin>>x;
        score.push_back(x);
    }
    vector<int> ans;
    for (int i=0;i<m;i++){
        int y;
        cin>>y;
        ans.push_back(y);
    }
    while (n--){
        int sum=0;
        vector<int> b(m);
        for (int i=0;i<m;i++){
            cin>>b[i];
        }
        for (int i=0;i<m;i++){
            if (b[i]==ans[i]){
                sum+=score[i];
            }
        }
        cout<<sum<<endl;
    }
    return 0;
}
```

## 1062

```cpp
#include<bits/stdc++.h>
using namespace std;
int gcd(int a,int b){return b?gcd(b,a%b):a;}
int main(){
    int n1,m1,n2,m2,k;
    scanf("%d/%d",&n1,&m1);
    scanf("%d/%d",&n2,&m2);
    cin>>k;
    if(n1*m2>n2*m1){swap(n1,n2);swap(m1,m2);}
    vector<int> ans;
    for(int i=1;i<=k*n2/m2;i++)
        if(i*m1>k*n1&&i*m2<k*n2&&gcd(i,k)==1)
            ans.push_back(i);
    for(int i=0;i<ans.size();i++){
        if(i)cout<<" ";
        cout<<ans[i]<<"/"<<k;
    }
    return 0;
}
```

很恶毒啊，题目并不保证输入数据的大小顺序，不注意这个就会挂了一个测试点。

## 1063

```cpp
#include <bits/stdc++.h>
using namespace std;

double dis(int a,int b){
    return sqrt(a*a+b*b);
}

int n;
int main(){
    cin>>n;
    double maxx=0;
    for (int i=0;i<n;i++){
        int x,y;
        cin>>x>>y;
        maxx=max(maxx,dis(x,y));
    }
    printf("%.2f",maxx);
    return 0;
}
```

入门题

## 1064

```cpp
#include <bits/stdc++.h>
using namespace std;

int sum(int a){
    int res=0;
    while (a>0){
        int t=a%10;
        res+=t;
        a/=10;
    }
    return res;
}

int main(){
    int n;
    cin>>n;
    //unordered_map<int,int> hash1;
    set<int> ans;
    for (int i=0;i<n;i++){
        int b;
        cin>>b;
        //hash1[sum(b)]++;
        ans.insert(sum(b));
    }
    vector<int> p;
    for (auto it:ans){
        p.push_back(it);
    }
    cout<<p.size()<<endl;
    for (int i=0;i<p.size();i++){
        if (i==p.size()-1){
            cout<<p[i];
        }else{
            cout<<p[i]<<" ";
        }
    }
    return 0;
}
```

给我看笑了。

你以为这个朋友数一定是要有人和它相同吗，不不不，只要这个数字有就行了。

那你还说啥啊，直接set就完了呗。

## 1065

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
string a,b;
bool cmp(string x,string y){
    return x<y;
}
int main(){
    cin>>n;
    unordered_map<string,string> h1;
    set<string> people;
    for (int i=0;i<n;i++){
        cin>>a>>b;
        h1[a]=b;
        h1[b]=a;
        people.insert(a);
        people.insert(b);
    }
    int m;
    cin>>m;
    set<string> id;
    for (int i=0;i<m;i++){
        string s;
        cin>>s;
        id.insert(s);
    }
    vector<string> ans;
    for (auto it:id){
        if (people.find(it)==people.end()){
            ans.push_back(it);
        }else if(people.find(it)!=people.end()&&id.find(h1[it])==id.end()){
            ans.push_back(it);
        }
    }
    cout<<ans.size()<<endl;
    sort(ans.begin(),ans.end(),cmp);
    for (int i=0;i<ans.size();i++){
        if (i==ans.size()-1){
            cout<<ans[i];
        }else{
            cout<<ans[i]<<" ";
        }
    }
    return 0;
}
```

又是STL熟练度检测题，用unordered_map建立夫妻相互映射就行，然后在目标集合中查询即可。

## 1066

```cpp
#include <bits/stdc++.h>
using namespace std;

int m,n,a,b,r;

int main(){
    cin>>m>>n>>a>>b>>r;
    vector<vector<int>> image(m,vector<int>(n));
    for (int i=0;i<m;i++){
        for (int j=0;j<n;j++){
            cin>>image[i][j];
        }
    }
    for (int i=0;i<m;i++){
        for (int j=0;j<n;j++){
            if (image[i][j]>=a&&image[i][j]<=b){
                image[i][j]=r;
            }
        }
    }
    for (int i=0;i<m;i++){
        for (int j=0;j<n;j++){
            printf("%03d",image[i][j]);
            if (j!=n-1) cout<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

## 1067

```cpp
#include <bits/stdc++.h>
using namespace std;

string cipher;
int n;
int main(){
    cin>>cipher>>n;
    cin.ignore();//记得忽略掉最后面的换行，这里不忽略包完蛋的.
    string try_cipher;
    int try_num=0;
    while (1){
        if (try_num>=n){
            cout<<"Account locked"<<endl;
            break;
        }
        getline(cin,try_cipher);//输入可能有空格哦
        if (try_cipher=="#"){
            break;
        }else if(try_cipher!=cipher){
            cout<<"Wrong password: "<<try_cipher<<endl;
            try_num++;
        }else if(try_cipher==cipher){
            cout<<"Welcome in";
            break;
        }
    }
    return 0;
}
```

我保证正确密码无空格！=我保证你输入的密码无空格

## 1068

```cpp
#include <bits/stdc++.h>
using namespace std;

int m,n,tol;
int main(){
    cin>>m>>n>>tol;
    vector<vector<int>> image(n+2,vector<int>(m+2));
    set<int> key1;
    unordered_map<int,int> h;
    for (int i=0;i<n+2;i++){
        for (int j=0;j<m+2;j++){
            if (i==0||i==n+1){
                image[i][j]=-1;
            }else{
                if (j==0||j==m+1){
                    image[i][j]=-1;
                }else{
                    cin>>image[i][j];
                    key1.insert(image[i][j]);
                    h[image[i][j]]++;
                }
            }
        }
    }
    set<int> ans_key;
    for (auto it:key1){
        if (h[it]==1){
            ans_key.insert(it);
        }
    }
    if (ans_key.size()==0){
        cout<<"Not Exist"<<endl;
        return 0;
    }else{
        int sum=0;
        int x,y,ans;
        int dx[8]={-1,-1,-1,0,0,1,1,1};
        int dy[8]={-1,0,1,-1,1,-1,0,1};
        for (auto it:ans_key){
            for (int i=1;i<=n;i++){
                for (int j=1;j<=m;j++){
                    if (image[i][j]==it){
                        bool ok=true;
                        for (int k=0;k<8;k++){
                            int ni=i+dx[k],nj=j+dy[k];
                            if (abs(image[ni][nj]-image[i][j])<=tol){
                                ok=false;
                                break;
                            }
                        }
                        if (ok){
                            sum++;
                            x=j;
                            y=i;
                            ans=image[i][j];
                        }
                    }
                }
            }
        }
        if (sum==0) cout<<"Not Exist"<<endl;
        else if (sum>1) cout<<"Not Unique"<<endl;
        else cout<<"("<<x<<", "<<y<<"): "<<ans<<endl;
    }
}
```

我的天啊这没超时，数据怎么能弱成这样……

简单来说就是按照题目要求模拟而已，我在外面加了一圈-1其实是为了防止数组越界。

更优的做法是存储ans_key的时候就把坐标一起存储了，这样就算加强数据了也不会有问题。
