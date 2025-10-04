# New Start——两个月速通代码随想录

## 数组：

### 二分查找

```cpp
class Solution {
public:
    int search(vector<int>& a, int t) {
        int n=a.size();
        set<int> s;
        for (auto it:a){
            s.insert(it);
        }
        if (s.find(t)==s.end()){
            return -1;
        }else{
            int l=0,r=n-1;
            while (l<=r){
                int mid=(l+r)/2;
                if (a[mid]==t){
                    return mid;
                }else if(a[mid]>t){
                    r=mid-1;
                }else{
                    l=mid+1;
                }
            }
        }
        return -1;
    }
};
```

### 搜索插入位置

```cpp
class Solution {
public:
    int searchInsert(vector<int>& a, int t) {
        int n=a.size();
        set<int> s;
        for (auto it:a){
            s.insert(it);
        }
        if (s.find(t)==s.end()){
            if (t<a[0]){
                return 0;
            }
            if (t>a[n-1]){
                return n;
            }
            for (int i=1;i<=n-1;i++){
                if (a[i]>t&&a[i-1]<t){
                    return i;
                }
            }
        }else{
            int l=0,r=n-1;
            while (l<=r){
                int mid=(l+r)/2;
                if (a[mid]==t){
                    return mid;
                }
                if (a[mid]>t){
                    r=mid-1;
                }
                if (a[mid]<t){
                    l=mid+1;
                }
            }
        }
        return 0;
    }
};
```

感觉有点像拿内存换时间的感觉？

### 在排序数组中查找元素的第一个和最后一个位置

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& a, int t) {
        int n=a.size();
        set<int> s;
        for (auto it:a){
            s.insert(it);
        }
        if (s.empty()||s.find(t)==s.end()){
            return {-1,-1};
        }else{
            int ansl=0,ansr=0;
            int l=0,r=n-1;
            while (l<=r){
                int mid=(l+r)/2;
                if (a[mid]>t){
                    r=mid-1;
                }else{
                    l=mid+1;
                    ansr=l;
                }
            }
            l=0,r=n-1;
            while (l<=r){
                int mid=(l+r)/2;
                if (a[mid]>=t){
                    r=mid-1;
                    ansl=r;
                }else{
                    l=mid+1;
                }
            }
            return {ansl+1,ansr-1};
        }
        return {-1,-1};
    }
};
```

### 不使用内置函数的算术平方根查找

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l=1,r=x;
        int ans=0;
        while (l<=r){
            int mid=l+(r-l)/2;
            if ((long long)mid*mid<=x){
                ans=mid;
                l=mid+1;
            }else{
                r=mid-1;
            }
        }
        return ans;
    }
};
```

### 不适用内置函数的完全平方数判断

```cpp
class Solution {
public:
    bool isPerfectSquare(int x) {
        int l=1,r=x;
        int ans=0;
        while (l<=r){
            int mid=l+(r-l)/2;
            if ((long long)mid*mid<=x){
                ans=mid;
                l=mid+1;
            }else{
                r=mid-1;
            }
        }
        if ((long long)ans*ans==x){
            return 1;
        }else{
            return 0;
        }
    }
};
```

其实这个我感觉比起上一题而言只是多了一个if和else的判断。

### 不使用额外空间的移除数组元素法

```cpp
class Solution {
public:
    int removeElement(vector<int>& a, int v) {
        int n=a.size();
        int fastidx=0,slowidx=0;
        while (fastidx<n&&slowidx<n){
            if (a[fastidx]!=v){
                a[slowidx++]=a[fastidx];
            }
            fastidx++;
        }
        return slowidx;
    }
};
```

快慢指针法，当快指针遇到了要移除的元素直接跳过，不用移除的元素移到慢指针的下一位。

