---
title: PAT-B的再探再报
date: 2025-11-02 00:20:00
tags: C++,算法
categories: 程序设计
---
# PAT-B的再探再报——常见的一些模拟板子

## 前言：

  在PAT-B的各题训练中我发现有一些“模板”是常常被使用的，写好这些模板是很重要的，可以减少我们犯错的次数以及加深理解，从而实现从“快”到“又快又稳”的突破。

  我将会按照题号选出其中比较经典的一些板子供自己参考和使用，并且给出原理说明。

## 代码规范

为追求编码速度和避免编译错误，本文所有代码示例均遵循以下通用规范：

1. 使用万能头文件 `#include <bits/stdc++.h>`包含所有标准库。
2. 使用 `using namespace std;`省略标准库前缀。

注意：千万不可以在工程里用万能头，千万不可以。万能头是出于应试的选择，目的在于不花太多时间纠结头文件。

## 1002 写出这个数

**关键词：如何去建立不同数据类型之间的映射**

这一题的关键点在于如何去把数字和汉字对应上，就例如说，怎么把“0”对应输出成“ling”，“1”对应输出成“yi”。

这个其实很简单了，数字去对应字符串，只要去建立一个对应的“字典”，你用map还是用数组都无所谓，不过在这题对应的不多的情况下还是用静态数组吧。

那其实代码实现很简单了

```cpp
string b[15] = {"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
//数组记得稍微开大一点，不是坏事，养成这个习惯可以防RE，用得到的int型数据去当下标然后一一对应。
```

当然也有用map的就例如

```cpp
unordered_map<int,string> h;
h.emplace(1,"yi");
.......
```

具体使用哪种数据结构看题目要求咯。

## 1004 成绩排名

**关键词：如何让std::sort()实现自定义排序，建议和2024机考的绝对值排序一起看**

一般而言，使用`std::sort()`时想要进行自定义排序我们是这么做的

```cpp
bool cmp(t a,t b){//t在这里表示一种数据类型，无论是语言自带的还是自己设计的结构体，我懒得打模板那个单词
   return a<b//这个就表示从小到大排序
   return a>b//这个就表示从大到小排序
}
```

就例如这一题里面的

```cpp
bool cmp(stu a,stu b){
    return a.gpa>b.gpa;
}
```

一般而言这个函数的返回类型需要是布尔类型，名字无所谓，但我喜欢定义成cmp

sort函数执行的时候，这个是默认a在前b在后面的，理解了这个就行了。

最终使用

```cpp
sort(add.begin(),add.end(),cmp);//注意这个begin和end的写法是vector才有的
```

## 1007 素数对猜想

**关键词：如何判断一个数是不是素数**

首先素数的定义应该都很清楚了，这里不赘述概念，直接贴代码

```cpp
bool isprime(int a){
    if (a<=1) return false;//这里用这样的写法是更加稳健的，谁都不知道题目会不会输入负数进来
    if (a==2) return true;
    for (int i=2;i*i<a;i++){//这里采用i*i的写法下面讲，这是一个利用数学性质降低时间复杂度。
        if (a%i==0){
            return false;
        }
    }
    return true;
}
```

第四行为什么要写`i*i<a`呢。我举个例子，如果你输入了一个数据，这个数据是27，那么27是不是可以被3整除，能被3整除说明，27/3=9，9也可以被3整除。这样子推下去其实我们只要枚举被根号27小的数据就行了。因为另一个因子都可以被前面的因子除出来。

这时候有人要问了：“主播主播，那万一在大于根号27的地方有一个数字能整除27怎么办，那不会漏答案吗？”这种情况是不可能的喵，那我问你啊，9是不是比根号27大，如果9能够整除27，那27/9的结果3是不是也整除27啊，这个因子是成对的啊，前面已经列举过了。

素数的模板是很重要的，其实还有一种素数筛的写法，不过那个对目前而言没啥意义。

## 1009 说反话

**关键词：怎么以空格为分界点分割字符串**

这题是典型的初见杀题目，很多人在第一眼看到这个题目的反应绝对是：按空格分割？这怎么搞？

然后就有肯定有人去查了AI，然后AI就告诉他用`istringstream`，就像下面这样

```cpp
......
istringstream iss(s);
    string word;
    vector<string> tokens;

    while (iss >> word) { // 自动以空格、制表符分隔
        tokens.push_back(word);
    }
......
```

那我现在就把这个函数给揉碎了讲，它的底层实现实际上就是这样子的

```cpp
getline(cin,a);//不要cin，cin遇到空格会直接断掉
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
```

这样子的一串代码如果稍加包装就可以写成一个函数，甚至只需要修改一下第五行就可以实现不同字符作为分割的分界点，可以说很好用了，而且写起来也不麻烦。

注意14-16行，一定要记得把`word`中的残余部分倒出来到数组里面，不然会漏掉最后一个字符。

如果理解了下面那个稍微”复杂“的过程，那基本上这种题就难不倒了。

补充题目：敲笨钟。

## 1017 A除以B

**关键词：整数的高精度除法**

这一题是一个1k位的数字作为被除数，1k位就代表着传统的int类型相除会直接失效，这个时候就需要一些步骤来实现除法了。

高精度除法事实上就是在做一件事情，模拟你竖式计算的过程，甚至高精度加法和高精度减法也是在做这件事情，直接上代码

```cpp
int r=0,num=0;
    string s,ans;
    for (char c:a){
        num=r*10+(c-'0');
        s+=(num/b)+'0';
        r=num%b;//这个数字是上一轮得到的余数，应该定义在全局。
    }
    int pos=0;
    while (pos<s.size()-1&&s[pos]=='0'){
        pos++;
    }//去除前导0
    ans=s.substr(pos);
```

## 1022 D进制的A+B

**关键词：进制转换**

进制转换在纸上怎么操作我不想多讲，因为这个不是这一点篇幅能讲完的。

代码上的进制转换实际上有两个很重要的点，一个是怎么获取结果，另外一个是怎么做对应（就例如转16进制）

这一题实际上就是给了一个比较通用的模板了

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

我猜有人又要问了：”主播主播，如果D超过10怎么办？“D超过10你就去做一个映射啊，这还不简单？

## 1025 翻转链表

**关键词：链表模板**

我要发表暴论了。

PAT上面所有的链表题都是披着链表皮的数组题，所以有些链表上的方法，例如哨兵节点就没什么太大用处了，这种题目全部有套路，我将会马上拆解

链表节点具有三个元素：1 链表节点地址 2 链表携带的数据 3 链表指向的下一个节点的地址

那么怎么把这三个数据串在一起，答案是类

```cpp
class node{
    public:
    int add,data,next;
};
```

（~~你问我为什么不用结构体，因为class这个单词好写啊还能因为什么~~）

一般而言类PAT的输入格式是这样的

```cpp
00100 6 4//第一个是初始节点地址，第二个是节点总个数
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
```

那么问题出现了，我们该怎么通过这一串好像有点意义不明的数字来访问一个节点呢？

答案是用map，写法如下

```cpp
unordered_map<int,node> a;//这个用于记录地址和节点之间的映射关系
```

（~~其实考虑到PAT的地址基本都是一个最多五位的数字，头铁的话也可以用数组~~）

现在我们要将这些散散的节点搓成一个连续内存的数组，那我们怎么搓呢，我们已经知道了头节点的地址，那么头结点是不是会记录它的下一个节点，我们就可以这样子一直顺藤摸瓜下去了,代码如下

```cpp
while(beginn!=-1){
        d.push_back(a[beginn]);
        beginn=a[beginn].next;
    }
```

这样子就把链表转成连续内存的数组了，后面的操作会方便很多喵

中间的处理部分依照题目而定。

处理完了之后要输出答案，但是这个时候要记得修改一下你处理完了的节点的下一个地址，这个时候map的映射又有用了，代码如下

```cpp
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
```

到这里就结束了，很简单很公式的喵。

## 1038 统计同成绩学生

**关键词：如何去选择一个合适的容器进行数量统计**

自问：知道几种计数的方法？

我现在给出几个计数法：

1 如果题目需要计数的是一个单一的指标，用一个单一的变量就可以，例如变量cnt。

2 如果题目对于查找的高效性比较放松，并且计数指标之间的关系比较简单，那么可以用静态数组。

3 如果题目对于查询的效率有较高要求，而且计数指标之间的关系复杂多样，考虑使用map。

这一题就是一个典型的`unordered_map`容器题目了，代码如下

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

unordered_map查询的速度是非常快的，O(1)的复杂度

如果还想练，那么下一题1039就是一个不错的选择。

## 1043 输出PATest

**关键词：字符串如何重复输出**

其实我把这一题收录进来的时候我在想，到底要不要收录，后来想想还是收录吧，虽然不是那么常见

这一题其实可以视为上一个部分的延申了，关键也是在于如何选择一个合适的容器来计数，以及如何交替输出

空说无用，贴代码。

```cpp
while (sum>0){//这里控制一整个循环的进行，sum代表所有符合条件的字符，一旦sum归零，那么循环就该结束
        for (char c:res){//res就是目标字符串了
            if (count[c]!=0){//count是map容器，如果这个元素还有的用，那么就输出吧。
                cout<<c;
                count[c]--;
                sum--;
            }else{//如果这个元素已经没得用了，那就直接跳过。
                continue;
            }
        }
    }
```

## 1048 数字加密

**关键词：在处理类似字符串数字相加的问题时要如何高效的避免长度带来的问题**

这一题它需要字符串之间做好对应相加，然后再用映射关系修改字符串中的字符，那么这个时候问题就出现了：如果这两个字符串并不等长，那我们该如何处理溢出的那一部分？

答案是，不用处理，只要用几个不会影响最终答案的字符去补全相对较短的那一个字符串就可以了，这样子就可以省去很多处理的麻烦

例如这一题，我们在前面补0，那么就是没有影响的，代码如下

```cpp
while (a.size() < b.size()) a = "0" + a;
while (b.size() < a.size()) b = "0" + b;
```

这个预处理方法在高精度减法也会用用处。

当然这么写的话其实不是很好，你大可以提前计算好要补的’0‘字符数量，然后牺牲一点点内存来换更快的处理。

## 1062 最简分数

**关键词：最大公约数求法GCD**

部分题目可能会涉及到最大公约数的计算，这个东西的计算是有数学上的固定算法的（我这里不去在数学角度上证明这个算法）（~~你问我为什么不证，我不会证啊~~）

GCD算法我记得是有两种写法的，一种是直接利用数字之间的交换，另外一种是利用递归。

本人喜欢递归写法，也比较推荐递归写法，代码如下

```cpp
int gcd(int a,int b){
    if (b==0) return a;
    else return gcd(b,a%b);
}
```

这个算法大名叫做欧几里得算法，如果感兴趣的话可以去自行百度一下证明和原理。

## 1065 单身狗

**关键词：如何利用合适的STL容器来快速查找是否存在有一个元素**

关于如何去查找一个元素，大多数人应该都有自己的想法啊，例如说去做一个`vis`数组什么的去标记。不过我常常喜欢内置的`find()`函数。

这一题就是很经典的，映射加查找了。我们先建立夫妻之间的映射，然后把参会人员全部放在一个集合里面，去查找这里面是否存在映射对象，就例如下面这样

```cpp
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
```

其中，内置的`find()`函数如果没有在这个集合里面查找到目标元素，那么它就会范围这个集合末尾的一个迭代器。就例如上一题里面的`people.end()`，如果查找到了就会返回那个位置的迭代器。

注意，如果你的目的只是单纯的查询，那么使用`set`就行，但如果你的目的不止于查询，还要找到这个元素的位置，那么`set`会自动排序的特点可能会对这个有所影响，请注意去选择其他的STL容器或者算法。

另外我要提一点啊，`string`也有内置的`find`函数，但是`string`用`find`函数没找到是这么写的，such as

```cpp
string s;
s.find('a')==string::npos;//这个代表没有找到a这个元素
```

## 1078 字符串的压缩与解压缩

**关键词：如何读取一个由连续重复字符读取的字符串以及如何给定一个数字和字符的组合去输出目标的连续重复字符串**

这一题一整题都在干这个事情。我现在揉碎了讲讲。

我们先讲第一点啊，如何读取一个由连续重复字符组成的字符串，并且输出以及压缩

我们可以很直观的想到一种方法，去遍历一整个字符串，然后定义两个指针`slow`与`fast`，其中`slow`控制遍历的初始位置，`fast`读取连续字符最后的位置，最后利用`substr()`函数来把这样子一个字符串拆出来。

就例如，我现在给你一个这样的字符串`aaaaabcbdddd`，如何去把里面由重复字符组成的子字符串分割出来，用我们上面的方法就行，代码如下。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s="aaaaabcbdddd";//这个字符串只是举例
    int n=s.size();
    int slow=0;
    int fast=0;
    vector<string> res;
    while (slow<n){
        int c=s[slow];//读取slow指针指向的第一个字符
        while (fast<n){//随后fast指针开始移动，如果fast指针指向的字符和c相同，那就加1，不相同直接退出
            if (s[fast]!=c||fast==n-1){//这里要做一个防止越界的操作
                break;
            }else{
                fast++;
            }
        }
        string word=s.substr(slow,fast-slow);//这里需要解释一下第二个参数为什么不加1，因为fast指向的是第一个不同元素
        res.push_back(word);
        slow=fast;//将slow直接移动到fast的位置
    }
    for (int i=0;i<res.size();i++){
        cout<<res[i]<<" ";//这个地方我懒得改了，懂就行。
    }
    return 0;
}
```

这串代码可以实现要求的功能，并且做一下封装就可以变成一个可复用的函数。

以及，这串代码的时间复杂度已经是最优，为O(n)，也就是至少需要遍历一次字符串，空间复杂度可以优化，但是意义不大。

我在这道题目里面用了队列的做法，有兴趣可以去尝试一下（~~写起来其实挺便秘的~~）

现在讲完了压缩，我们来讲一下解压。

解压事实上就是，给你一个字符串，里面有数字和字符组成，而数字就代表着它后面的那个字符重复了多少次。这个可能比压缩还简单点，因为我目测这个只需要一个指针就能解决问题。

我用字符串`5T2h4is i5s a3 te4st CA3a as10Z`举个例子，这里面的空格也算一个字符，代码如下。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s="5T2h4is i5s a3 te4st CA3a as10Z";//空格也算作一个字符
    int n=s.size();
    int pos=0;//单指针
    while (pos<n){
        char c=s[pos];
        if (isdigit(c)){
            string num;//待读取的数字，这个数字可能是两位数
            while (pos<n){
                if (!isdigit(s[pos])){
                    break;
                }else{
                    num+=s[pos];
                    pos++;
                }
            }
            int r=stoi(num);//stoi是一个把字符串数字转化为int的好函数。
            for (int i=0;i<r;i++){
                cout<<s[pos];
            }
            pos++;
        }else{
            cout<<s[pos];
            pos++;
        }
    }
    return 0;
}
```

压缩也就是这样了。

其实解压和压缩最核心的就是想到拿指针去指向每一个字符，然后注意一些坑点就行了。

其实你要说，用最笨的方法，两层循环找重复字符串也不是不行（~~行在哪了我请问？~~），但是这个时间复杂度很爆，我们尽量一次遍历就把这个问题解决掉。

~~有一个很变态的思路，就是用回溯去切割字符串，然后外部函数判断一下这个字符串是不是重复字符组成的。但是这个实现起来太小众且变态了，而且时间复杂度更爆了，头铁可以去写（我是不会去的），虽然半点实战价值都没有（）~~

## 1079 延迟的回文数

**关键词：高精度加法与回文数的判断**

需要马上注意到这个

> 输入在一行中给出一个不超过1000位的正整数。

这个意味着什么，意味着就算是开long long都要爆喵，那这个时候我提出过了，用高精度加法。

高精度的加减乘除全部都是在做一件事情，那就是模拟人类竖式计算的过程，包括溢出数字的处理之类的。

多说无益，直接贴代码

```cpp
string addition(string a,string b){
    while(a.size()<b.size()){
        a='0'+a;
    }
    while(b.size()<a.size()){
        b='0'+b;
    }//这里我前面提到过了，是为了方便后面的主程序做的预处理。
    int n=a.size();
    string ans1,ans;//ans1是初步结果，ans是返回结果，这个变量名起的有点变态。
    int t=0;//t是上一次剩下的溢出数
    for (int i=n-1;i>=0;i--){//最后一位才是个位
        int x=a[i]-'0';
        int y=b[i]-'0';//转化为int类型，注意不能用stoi
        char c=(x+y+t)%10+'0';
        t=(x+y+t)/10;//计算溢出的数字
        ans1=c+ans1;//加入答案
    }
    if (t!=0){
        ans1=char('0'+t)+ans1;//循环结束之后把溢出数加上去
    }
    bool flag=0;//去前导0逻辑
    for (int i=0;i<ans1.size();i++){
        if (flag){
            ans+=ans1[i];
        }else if(!flag&&ans1[i]!='0'){//遇到第一个非0数字，立刻结束去0
            ans+=ans1[i];
            flag=1;
        }
    }
    return ans;
}
```

高精度加法函数最后返回的应该是一个字符串类型。强烈建议这个自己动手写。

高精度加法有一个很好的点是，它只需要修改函数内部分参数，就可以实现不同进制之间的加法。

接下来讲一下回文的判断，这里我给两种方法，各有优劣，看爱用哪种

1.把字符直接翻转，然后判断和原字符是否相等。

2.利用双指针法逐字判断。

我这里给一下第一种方法的代码，这个方法要牺牲一点内存。

```cpp
bool isback_num(string a){
    string b;
    int n=a.size();
    for (int i=n-1;i>=0;i--){
        b+=a[i];
    }
    if (a==b){
        return true;
    }else{
        return false;
    }
}
```

## 1103 缘分数

**关键词：如何判断一个数字是否是一个完全平方数。**

如果基础知识扎实的人，应该知道C++里面`float`转`int`是直接把小数部分扔掉而不是四舍五入（四舍五入可以用`std::round()`）

那么如果我们对一个数字使用`sqrt`函数呢，那么这个函数就会得到一个最接近于正确答案的整数，这个时候用这个整数去平方，如果等于待测数字，那么就可以返回`true`，不等于，直接返回`false`.

直接贴代码

```cpp
bool isquare(int n){
    if (n<0) return false;
    int r=sqrt(n);
    return r*r==n;
}
```

## 1116 多2了一点

**关键词：高精度减法。**

我已经懒得再说高精度是在干什么了，直接给代码

```cpp
string subtraction(string a,string b){
    string res,ans;
    bool flag=0;
    if (!cmp(a,b)){
        swap(a,b);
        flag=1;//控制正负
    }
    while (b.size()<a.size()){
        b='0'+b;
    }
    int n=a.size();
    int p=0;
    for (int i=n-1;i>=0;i--){
        int x=(a[i]-'0')-p;
        int y=(b[i]-'0');
        if (x>=y){
            int temp=(x-y);
            res+=(char)('0'+temp);
            p=0;
        }else{
            int temp=(10+x-y);
            res+=(char)('0'+temp);
            p=1;
        }
    }
    reverse(res.begin(),res.end());
    int pos=0;
    while (pos<res.size()&&res[pos]=='0'){
        pos++;
    }//新的去0逻辑。
    if (pos==res.size()) return "0";
    else{
        ans=res.substr(pos);
        if (flag) ans='-'+ans;
    }
    return ans;
}
```

现在我总结一下我们已经有的各种高精度

加法：

```cpp
string addition(string a,string b){
    while(a.size()<b.size()){
        a='0'+a;
    }
    while(b.size()<a.size()){
        b='0'+b;
    }//这里我前面提到过了，是为了方便后面的主程序做的预处理。
    int n=a.size();
    string ans1,ans;//ans1是初步结果，ans是返回结果，这个变量名起的有点变态。
    int t=0;//t是上一次剩下的溢出数
    for (int i=n-1;i>=0;i--){//最后一位才是个位
        int x=a[i]-'0';
        int y=b[i]-'0';//转化为int类型，注意不能用stoi
        char c=(x+y+t)%10+'0';
        t=(x+y+t)/10;//计算溢出的数字
        ans1=c+ans1;//加入答案
    }
    if (t!=0){
        ans1=char('0'+t)+ans1;//循环结束之后把溢出数加上去
    }
    bool flag=0;//去前导0逻辑
    for (int i=0;i<ans1.size();i++){
        if (flag){
            ans+=ans1[i];
        }else if(!flag&&ans1[i]!='0'){//遇到第一个非0数字，立刻结束去0
            ans+=ans1[i];
            flag=1;
        }
    }
    return ans;
}
```

减法：

```cpp
string subtraction(string a,string b){
    string res,ans;
    bool flag=0;
    if (!cmp(a,b)){
        swap(a,b);
        flag=1;//控制正负
    }
    while (b.size()<a.size()){
        b='0'+b;
    }
    int n=a.size();
    int p=0;
    for (int i=n-1;i>=0;i--){
        int x=(a[i]-'0')-p;
        int y=(b[i]-'0');
        if (x>=y){
            int temp=(x-y);
            res+=(char)('0'+temp);
            p=0;
        }else{
            int temp=(10+x-y);
            res+=(char)('0'+temp);
            p=1;
        }
    }
    reverse(res.begin(),res.end());
    int pos=0;
    while (pos<res.size()&&res[pos]=='0'){
        pos++;
    }//新的去0逻辑。
    if (pos==res.size()) return "0";
    else{
        ans=res.substr(pos);
        if (flag) ans='-'+ans;
    }
    return ans;
}
```

除法：

```cpp
int r=0,num=0;
    string s,ans;
    for (char c:a){
        num=r*10+(c-'0');
        s+=(num/b)+'0';
        r=num%b;//这个数字是上一轮得到的余数，应该定义在全局。
    }
    int pos=0;
    while (pos<s.size()-1&&s[pos]=='0'){
        pos++;
    }//去除前导0
    ans=s.substr(pos);
```

乘法也是同理了。

## 1118 如需挪车请致电

关键词：手搓计算器

直接贴代码

```cpp
#include <bits/stdc++.h>
using namespace std;

string s;
int main(){
    unordered_map<string,int> n;
    n.emplace("yi",1),n.emplace("er",2),n.emplace("san",3),
    n.emplace("si",4),n.emplace("wu",5),n.emplace("liu",6),
    n.emplace("qi",7),n.emplace("ba",8),n.emplace("jiu",9),
    n.emplace("ling",0);
    vector<int> ans;
    for (int i=1;i<=11;i++){
        getline(cin,s);
        if (s.size()==1&&isdigit(s[0])){
            ans.push_back(stoi(s));
        }else if (s.size()>=4&&isalpha(s[0])){
            string t=s.substr(0,4);
            if (t=="sqrt"){
                int x=sqrt(stoi(s.substr(4)));
                ans.push_back(x);
            }else if(t=="ling"){
                ans.push_back(n[s]);
            }
        }else if(s.size()==2&&isalpha(s[0])){
            ans.push_back(n[s]);
        }else if(s.size()==3&&isalpha(s[0])){
            ans.push_back(n[s]);
        }else if(isdigit(s[0])){
            vector<int> nums;
            string m;
            string f;
            for (int i=0;i<s.size();i++){
                if (s[i]=='+'||s[i]=='-'||s[i]=='*'||s[i]=='/'||s[i]=='%'||s[i]=='^'){
                    f=s[i];
                    int y=stoi(m);
                    nums.push_back(y);
                    m.clear();
                }else{
                    m+=s[i];
                }
            }
            if (!m.empty()){
                int t=stoi(m);
                nums.push_back(t);
                m.clear();
            }
            if (f=="+"){
                ans.push_back(nums[0]+nums[1]);
            }
            if (f=="-"){
                ans.push_back(nums[0]-nums[1]);
            }
            if (f=="*"){
                ans.push_back(nums[0]*nums[1]);
            }
            if (f=="/"){
                ans.push_back(nums[0]/nums[1]);
            }
            if (f=="%"){
                if (nums[1]==0) ans.push_back(0); 
                else ans.push_back(nums[0] % nums[1]);
            }
            if (f=="^"){
                ans.push_back(pow(nums[0],nums[1]));
            }
        }
    }
    for (int i=0;i<ans.size();i++){
        cout<<ans[i];
    }
    return 0;
}
```

## 写在最后：

  这个我写了三天吧，感觉有点力竭了。

  希望有用吧。毕竟PAT乙级大多数题目的基本模板都包括了。

​                                                                                                                                                                                                          ——Schariac125

​                                                                                                                                                                      2025.11.2于福州大学旗山校区
