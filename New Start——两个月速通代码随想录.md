---
title: New Start——两个月速通代码随想录
date: 2025-10-07 16:25:00
tags: C++,算法
categories: 程序设计
---
# New Start——两个月速通代码随想录

## 写在前面

  代码随想录的训练应该紧接在PAT乙级之后，这一套题目可以很迅速的帮助复习以及查缺补漏一些算法，难度适中，题目选取是优秀，有代表性的，名副其实。

  不过这个文档纯属是我个人笔记了，不看也无所谓，直接看原作者写的就好。

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

### 不使用内置函数的完全平方数判断

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

### 不使用额外空间的移除数组重复元素法

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& a) {
        unordered_map<int,int> m;
        int n=a.size();
        for (int i=0;i<n;i++){
            m[a[i]]++;
        }
        int slowidx=0,fastidx=0;
        while (slowidx<n&&fastidx<n){
            if (m[a[fastidx]]==1){
                a[slowidx++]=a[fastidx];
            }else if(m[a[fastidx]]>1){
                m[a[fastidx]]--;
            }
            fastidx++;
        }
        return slowidx;
    }
};
```

### 不使用额外空间的移除0法

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& a) {
        int n=a.size();
        int slowidx=0,fastidx=0;
        while (slowidx<n&&fastidx<n){
            if (a[fastidx]!=0){
                a[slowidx++]=a[fastidx];
            }
            fastidx++;
        }
        while (slowidx<n){
            a[slowidx++]=0;
        }
    }
};
```

### 字符串的比较方法（lk844）

```cpp
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }

    string build(string str) {
        string ret;
        for (char ch : str) {
            if (ch != '#') {
                ret.push_back(ch);
            } else if (!ret.empty()) {
                ret.pop_back();
            }
        }
        return ret;
    }
};
```

```cpp
var backspaceCompare = function(S, T) {
    let i = S.length - 1,
        j = T.length - 1,
        skipS = 0,
        skipT = 0;
    // 大循环
    while(i >= 0 || j >= 0){
        // S 循环
        while(i >= 0){
            if(S[i] === '#'){
                skipS++;
                i--;
            }else if(skipS > 0){
                skipS--;
                i--;
            }else break;
        }
        // T 循环
        while(j >= 0){
            if(T[j] === '#'){
                skipT++;
                j--;
            }else if(skipT > 0){
                skipT--;
                j--;
            }else break;
        }
        if(S[i] !== T[j]) return false;
        i--;
        j--;
    }
    return true;
};
```

### 数组的平方

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& a) {
        int n=a.size();
        for (int i=0;i<n;i++){
            a[i]=a[i]*a[i];
        }
        sort(a.begin(),a.end());
        return a;
    }
};
```

### 长度最小的子数组

```cpp
class Solution {
public:
    int minSubArrayLen(int t, vector<int>& a) {
        int n=a.size();
        int i=0,j=0;
        int minn=n;
        int sum=a[i];
        while (i<n&&j<n){
            if (sum<t){
                j++;
                if (j<n) sum+=a[j];
            }else{
                minn=min(minn,j-i+1);
                sum-=a[i];
                i++;
            }
        }
        if (sum<t&&i==0){
            return 0;
        }else{
            return minn;
        }
    }
};
```

### 水果成篮（lk904）

```cpp
class Solution {
public:
    int totalFruit(vector<int>& f) {
        unordered_map<int,int> cnt;
        int n=f.size();
        int ans=0;
        int left=0;
        for (int right=0; right<n; right++){
            cnt[f[right]]++;
            while (cnt.size()>2){
                cnt[f[left]]--;
                if (cnt[f[left]]==0) cnt.erase(f[left]);
                left++;
            }
            ans=max(ans, right-left+1);
        }
        return ans;
    }
};
```

### 字串覆盖（lk76）

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        int n1 = s.size();
        int n2 = t.size();
        if (n1 < n2) return "";

        unordered_map<char,int> h1;
        for (auto c : t) h1[c]++;

        unordered_map<char,int> h2;
        int left = 0;
        int match = 0;
        int minlen = INT_MAX;
        int start = 0;

        for (int right = 0; right < n1; right++) {
            char c = s[right];
            if (h1.count(c)) {
                h2[c]++;
                if (h2[c] == h1[c]) match++;
            }

            while (match == (int)h1.size()) {
                if (right - left + 1 < minlen) {
                    minlen = right - left + 1;
                    start = left;
                }
                char d = s[left];
                if (h1.count(d)) {
                    h2[d]--;
                    if (h2[d] < h1[d]) match--;
                }
                left++;
            }
        }
        return minlen == INT_MAX ? "" : s.substr(start, minlen);
    }
};
```

### 螺旋矩阵（lk59）

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int top=0,bottom=n-1,left=0,right=n-1;
        int cur=1;
        vector<vector<int>> m(n,vector<int>(n));
        while (top<=bottom&&left<=right){
            for (int i=left;i<=right;i++){
                m[top][i]=cur;
                cur++;
            }
            top++;
            for (int i=top;i<=bottom;i++){
                m[i][right]=cur;
                cur++;
            }
            right--;
            if (top<=bottom){
                for (int i=right;i>=left;i--){
                    m[bottom][i]=cur;
                    cur++;
                }
                bottom--;
            }
            if (left<=right){
                for (int i=bottom;i>=top;i--){
                    m[i][left]=cur;
                    cur++;
                }
                left++;
            }
        }
        return m;
    }
};
```

## 链表理论

### 删除链表中的元素

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while (head!=NULL&&head->val==val){
            ListNode *temp=head;
            head=head->next;
            delete temp;
        }

        ListNode* cur=head;
        while (cur!=NULL&&cur->next!=NULL){
            if (cur->next->val==val){
                ListNode *temp=cur->next;
                cur->next=cur->next->next;
                delete temp;
            }else{
                cur=cur->next;
            }
        }
        return head;
    }
};
```

### 设计链表（lk707）

```cpp
class MyLinkedList {
public:
    // 定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };

    // 初始化链表
    MyLinkedList() {
        _dummyHead = new LinkedNode(0); // 这里定义的头结点 是一个虚拟头结点，而不是真正的链表头结点
        _size = 0;
    }

    // 获取到第index个节点数值，如果index是非法数值直接返回-1， 注意index是从0开始的，第0个节点就是头结点
    int get(int index) {
        if (index > (_size - 1) || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead->next;
        while(index--){ // 如果--index 就会陷入死循环
            cur = cur->next;
        }
        return cur->val;
    }

    // 在链表最前面插入一个节点，插入完成后，新插入的节点为链表的新的头结点
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }

    // 在链表最后面添加一个节点
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr){
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }

    // 在第index个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果index大于链表的长度，则返回空
    // 如果index小于0，则在头部插入节点
    void addAtIndex(int index, int val) {

        if(index > _size) return;
        if(index < 0) index = 0;        
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }

    // 删除第index个节点，如果index 大于等于链表的长度，直接return，注意index是从0开始的
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur ->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        //delete命令指示释放了tmp指针原本所指的那部分内存，
        //被delete后的指针tmp的值（地址）并非就是NULL，而是随机值。也就是被delete后，
        //如果不再加上一句tmp=nullptr,tmp会成为乱指的野指针
        //如果之后的程序不小心使用了tmp，会指向难以预想的内存空间
        tmp=nullptr;
        _size--;
    }

    // 打印链表
    void printLinkedList() {
        LinkedNode* cur = _dummyHead;
        while (cur->next != nullptr) {
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int _size;
    LinkedNode* _dummyHead;
};
```

### 翻转链表（lk206）

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *temp;
        ListNode *cur=head;
        ListNode *pre=NULL;
        while (cur!=NULL){
            temp=cur->next;
            cur->next=pre;
            pre=cur;
            cur=temp;
        }
        return pre;
    }
};
```

不得不说，这个确实是有小巧思在身上的。

### 两两交换链表中的结点（lk24）

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *dummyhead=new ListNode(0);
        dummyhead->next=head;
        ListNode *cur=dummyhead;
        while (cur->next!=NULL&&cur->next->next!=NULL){
            ListNode *temp1=cur->next;
            ListNode *temp2=cur->next->next->next;
            cur->next=cur->next->next;
            cur->next->next=temp1;
            cur->next->next->next=temp2;
            cur=cur->next->next;
        }
        return dummyhead->next;
    }
};
```

这个虚拟头结点指针操作真是惊为天人了，要换做以前我只会map存结点然后交换值。

### 删除链表的倒数第n个结点（lk19）

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy=new ListNode(0);
        dummy->next=head;
        ListNode *fast=dummy;
        ListNode *slow=dummy;
        while (n--&&fast!=NULL){
            fast=fast->next;
        }
        fast=fast->next;
        while (fast!=NULL){
            fast=fast->next;
            slow=slow->next;
        }
        slow->next=slow->next->next;
        return dummy->next;
    }
};
```

快慢指针，太经典了bro，这个思路惊为天人了。

### 链表相交

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *cur1=headA;
        ListNode *cur2=headB;
        int lenA=0,lenB=0;
        while (cur1!=NULL){
            lenA++;
            cur1=cur1->next;
        }
        while (cur2!=NULL){
            lenB++;
            cur2=cur2->next;
        }
        cur1=headA;
        cur2=headB;
        if (lenB>lenA){
            swap(lenA,lenB);
            swap(cur1,cur2);
        }
        int gap=lenA-lenB;
        while (gap--){
            cur1=cur1->next;
        }
        while (cur1!=NULL){
            if (cur1==cur2){
                return cur1;
            }
            cur1=cur1->next;
            cur2=cur2->next;
        }
        return NULL;
    }
};
```

### 环状链表（lk142）

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_map<ListNode*,int> v;
        while (head!=NULL){
            if (v.count(head)){
                return head;
            }
            v[head]++;
            head=head->next;
        }
        return NULL;
    }
};
```

这个方法真的很直观，图论里也会用到的。

## 哈希表

### 有效的字母异位词（lk242）

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size()!=t.size()){
            return 0;
        }
        unordered_map<char,int> sh;
        unordered_map<char,int> th;
        for (auto c:s){
            sh[c]++;
        }
        for (auto c:t){
            th[c]++;
        }
        for (auto c:s){
            if (t.find(c)==string::npos){
                return 0;
            }else{
                if (th[c]!=sh[c]){
                    return 0;
                }
            }
        }
        return 1;
    }
};
```

### 两个数组的交集（lk349）

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> h;
        set<int> exist;
        vector<int> ans;
        for (auto it:nums1){
            h[it]++;
        }
        for (auto it:nums2){
            if (h.count(it)&&exist.find(it)==exist.end()){
                ans.push_back(it);
                exist.insert(it);
            }
        }
        return ans;
    }
};
```

### 快乐数（lk202）

```cpp
int turnnum(int n){
    int res=0;
    while (n>0){
        int t=n%10;
        res+=t*t;
        n/=10;
    }
    return res;
}

class Solution {
public:
    bool isHappy(int n) {
        unordered_map<int,int> h;
        h.emplace(n,1);
        while (1){
            if (n==1){
                return 1;
            }
            n=turnnum(n);
            if (h.count(n)){
                return 0;
            }
            h[n]++;
        }
        return 0;
    }
};
```

一个数字一旦出现第二次，就说明开始死循环了，可以直接返回0

### 两数之和（lk1）

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int t) {
        unordered_map<int,int> h;
        int n=nums.size();
        for (int i=0;i<n;i++){
            auto ans=h.find(t-nums[i]);
            if (ans!=h.end()){
                return {ans->second,i};
            }
            h[nums[i]]=i;
        }
        return {};
    }
};
```

### 四数相加（lk454）

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> h;
        for (auto a:nums1){
            for (auto b:nums2){
                h[a+b]++;
            }
        }
        int sum=0;
        for (auto c:nums3){
            for (auto d:nums4){
                auto ans=h.find(0-(c+d));
                if (ans!=h.end()){
                    sum+=h[0-(c+d)];
                }
            }
        }
        return sum;
    }
};
```

我在想，三重循环是不是也能过？

### 三数相加（lk15）

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        int n=nums.size();
        sort(nums.begin(),nums.end());
        int l,r;
        for (int i=0;i<n;i++){
            if (nums[i]>0){
                return ans;
            }
            if (i>0&&nums[i]==nums[i-1]){
                continue;
            }
            l=i+1,r=n-1;
            while (l<r){
                if (nums[i]+nums[l]+nums[r]>0){
                    r--;
                }
                else if(nums[i]+nums[l]+nums[r]<0){
                    l++;
                }else{
                    ans.push_back({nums[i],nums[l],nums[r]});
                    while (l<r&&nums[l]==nums[l+1]) l++;
                    while (l<r&&nums[r]==nums[r-1]) r--;
                    l++;
                    r--;
                }
            }
        }
        return ans;
    }
};
```

太牛逼了双指针。

### 四数之和（lk18）

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());

        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;

                int l = j + 1, r = n - 1;
                while (l < r) {
                    long long sum = (long long)nums[i] + nums[j] + nums[l] + nums[r];
                    if (sum > target) r--;
                    else if (sum < target) l++;
                    else {
                        ans.push_back({nums[i], nums[j], nums[l], nums[r]});
                        while (l < r && nums[l] == nums[l + 1]) l++;
                        while (l < r && nums[r] == nums[r - 1]) r--;
                        l++;
                        r--;
                    }
                }
            }
        }
        return ans;
    }
};
```

和上一题双指针的思路一样，这个模板我感觉可以套到n数，只要不限制复杂度（

## 字符串理论

### 不使用额外空间的字符串翻转

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n=s.size();
        int l=0,r=n-1;
        while (l<r){
            swap(s[l],s[r]);
            l++;
            r--;
        }
        return;
    }
};
```

### 特定条件要求下的字符串翻转

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n=s.size();
        for (int i=0;i<n;i=i+2*k){
            if (i+k<n){
                reverse(s.begin()+i,s.begin()+i+k);
            }else{
                reverse(s.begin()+i,s.end());
            }
        }
        return s;
    }
};
```

快慢指针是错误的不可信的

### 反转字符串中的单词（lk151）

```cpp
class Solution {
public:
    string reverseWords(string s) {
        string word;
        vector<string> ans;
        for (auto c:s){
            if (c==' '&&!word.empty()){
                ans.push_back(word);
                word.clear();
            }else if(c!=' '){
                word+=c;
            }
        }
        if (!word.empty()){
            ans.push_back(word);
            word.clear();
        }
        int n=ans.size();
        string res;
        for (int i=n-1;i>=0;i--){
            if (i==0){
                res+=ans[i];
            }else{
                res+=ans[i];
                res+=' ';
            }
        }
        return res;
    }
};
```

搓轮子，这有什么好说的

### 重复字串（lk459）

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) != s.size();
    }
};
```

What can I say?

## 队列与栈理论

### 括号匹配

```cpp
class Solution {
public:
    bool isValid(string s) {
        int n=s.size();
        if (n%2==1) return 0;
        stack<char> sk;
        for (int i=0;i<n;i++){
            if (s[i]=='(') sk.push(')');
            else if(s[i]=='{') sk.push('}');
            else if(s[i]=='[') sk.push(']');
            else if(sk.empty()||sk.top()!=s[i]) return 0;
            else sk.pop();
        }
        return sk.empty();
    }
};
```

我的天哪代码随想录大人，曾经的顶级难题这下变成模板题了

### 去除重复字符

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        int n=s.size();
        stack<char> st;
        string ans;
        for (int i=0;i<n;i++){
            if (st.empty()){
                st.push(s[i]);
            }
            else if(st.top()!=s[i]){
                st.push(s[i]);
            }
            else if(st.top()==s[i]){
                st.pop();
            }
        }
        while (!st.empty()){
            ans+=st.top();
            st.pop();
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```

### 逆波兰表达式求值

```cpp
class Solution {
public:
    bool isNumber(const string& s) {
        if (s.empty()) return false;
        int i = 0;
        if (s[0] == '-' && s.size() > 1) i = 1;
        for (; i < s.size(); i++) {
            if (!isdigit(s[i])) return false;
        }
        return true;
    }

    int evalRPN(vector<string>& t) {
        stack<int> st;
        for (auto &s : t) {
            if (isNumber(s)) {
                st.push(stoi(s));
            } else {
                int x = st.top(); st.pop();
                int y = st.top(); st.pop();
                if (s == "+") st.push(y + x);
                else if (s == "-") st.push(y - x);
                else if (s == "*") st.push(y * x);
                else if (s == "/") st.push(y / x);
            }
        }
        return st.top();
    }
};
```

### 滑动窗口最大值

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n=nums.size();
        vector<int> ans(n);
        deque<int> maxx;
        for (int i=0;i<n;i++){
            while (!maxx.empty()&&nums[maxx.back()]<=nums[i]){
                maxx.pop_back();
            }
            while (!maxx.empty()&&maxx.front()<=i-k){
                maxx.pop_front();
            }
            maxx.push_back(i);
            ans[i]=maxx.front();
        }
        vector<int> res;
        for (int i=k-1;i<n;i++){
            res.push_back(nums[ans[i]]);
        }
        return res;
    }
};
```

有什么话跟我的单调双端队列说去吧

### 出现频率第K大的元素

```cpp
class Solution {
public:
    struct cmp {
        bool operator()(pair<int,int>& a, pair<int,int>& b) {
            return a.second > b.second; // 小根堆，频率小的优先
        }
    };
//注意这个地方的写法，小心炸缸
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> h;
        for (int x : nums) h[x]++;

        priority_queue<pair<int,int>, vector<pair<int,int>>, cmp> q;

        for (auto &p : h) {
            if (q.size() == k) {
                if (q.top().second < p.second) {
                    q.pop();
                    q.emplace(p.first, p.second);
                }
            } else {
                q.emplace(p.first, p.second);
            }
        }

        vector<int> ans;
        while (!q.empty()) {
            ans.push_back(q.top().first);
            q.pop();
        }
        return ans;
    }
};
```

我已经会写堆了（确信）

## 回溯理论

### 模板框架

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

### 组合

```cpp
vector<vector<int>> res;
vector<int> path;

void backtract(int start,int n,int k){
    if (path.size()==k){
        res.push_back(path);
        return;
    }
    for (int i=start;i<=n;i++){
        path.push_back(i);
        backtract(i+1,n,k);
        path.pop_back();
    }
}

class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        res.clear();
        path.clear();
        backtract(1,n,k);
        return res;
    }
};
```

很典型的回溯算法应用

### 组合变式

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int start,int n,int k){
    if (path.size()==k){
        int sum=0;
        for (int i=0;i<k;i++){
            sum+=path[i];
        }
        if (sum==n){
            ans.push_back(path);
            return;
        }
    }
    for (int i=start;i<=9;i++){
        path.push_back(i);
        backtract(i+1,n,k);
        path.pop_back();
    }
}
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        ans.clear();
        path.clear();
        backtract(1,n,k);
        return ans;
    }
};
```

### 电话号码的字符组合：

```cpp
string a[10]={"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
vector<string> ans;
string path;
void backtract(string s,int start,int k){
    if (path.size()==k){
        ans.push_back(path);
        return;
    }
    int m=s[start]-'0';
    string b=a[m];
    int n=b.size();
    for (int i=0;i<n;i++){
        path+=b[i];
        backtract(s,start+1,k);
        path.pop_back();
    }
}
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        ans.clear();
        path.clear();
        if (digits.empty()) return {};
        backtract(digits,0,digits.size());
        return ans;
    }
};
```

练手题，我喜欢这个。

### 组合总数

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int sum,int t,int start,vector<int> &a){
    if (sum>t){
        return;
    }
    if (sum==t){
        ans.push_back(path);
        return;
    }
    for (int i=start;i<a.size();i++){
        sum+=a[i];
        path.push_back(a[i]);
        backtract(sum,t,i,a);
        sum-=a[i];
        path.pop_back();
    }
}
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        ans.clear();
        path.clear();
        backtract(0,target,0,candidates);
        return ans;
    }
};
```

可以选取重复数字直接把双指针法废掉了（那我只能回溯了。

### 组合总数pro max

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int sum,int start,int t,vector<int> &a,vector<bool> &b){
    if (sum>t){
        return;
    }
    if (sum==t){
        ans.push_back(path);
        return;
    }
    for (int i=start;i<a.size();i++){
        if (i>0&&a[i]==a[i-1]&&!b[i-1]){
            continue;
        }else{
            sum+=a[i];
            path.push_back(a[i]);
            b[i]=true;
            backtract(sum,i+1,t,a,b);
            sum-=a[i];
            path.pop_back();
            b[i]=false;
        }
    }
}
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        ans.clear();
        path.clear();
        vector<bool> used(candidates.size(),false);
        sort(candidates.begin(),candidates.end());
        backtract(0,0,target,candidates,used);
        return ans;
    }
};
```

这个最难的地方在于如何把同层重复这个问题干掉

我们都知道，回溯算法本质上是在一棵树上跑，这个题由于要去除掉重复组合，就要在同一层干掉同层用过的数字。这个可能有点难理解，事实上就是一种去重。

当你回溯到一个根结点时，下面有两个子结点，如果这两个子结点相同，那么你只需要选取其中的一个就行了，另一个跳过。怎么判断一个数字是否该跳过，这个要在根结点阶段就进行，如果下面两个结点都是相同的，而且都没用过，那么就要跳过后面那个结点。

问题是，如果已经有一个用过了呢？那就不是同层重复了，那就是一个合理组合，要算答案的喵。

### 回文串字符分割

```cpp
vector<vector<string>> ans;
vector<string> path;
bool isback(string &s,int i,int j){
    while (i<j){
        if (s[i]!=s[j]){
            return 0;
        }
        i++;
        j--;
    }
    return 1;
}
void backtract(string &s,int start){
    if (start>=s.size()){
        ans.push_back(path);
        return;
    }
    for (int i=start;i<s.size();i++){
        if (isback(s,start,i)){
            string str=s.substr(start,i-start+1);
            path.push_back(str);
        }else{
            continue;
        }
        backtract(s,i+1);
        path.pop_back();
    }
}
class Solution {
public:
    vector<vector<string>> partition(string s) {
        ans.clear();
        path.clear();
        backtract(s,0);
        return ans;
    }
};
```

### IP地址复原

```cpp
vector<vector<string>> ans;
vector<string> path;

void backtract(int start, int cur, string &s) {
    if (cur == 4) {
        if (start == s.size()) ans.push_back(path);
        return;
    }
    for (int i = start; i < s.size(); i++) {
        string b = s.substr(start, i - start + 1);
        if (b.size() > 3) break;
        if ((b.size() == 1 || b[0] != '0') && stoi(b) <= 255) {
            path.push_back(b);
            backtract(i + 1, cur + 1, s);
            path.pop_back();
        } else break;
    }
}

class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        ans.clear();
        path.clear();
        backtract(0, 0, s);
        vector<string> res;
        for (auto &v : ans) {
            string a;
            for (int j = 0; j < 4; j++) {
                a += v[j];
                if (j != 3) a += '.';
            }
            res.push_back(a);
        }
        return res;
    }
};
```

与上面那题回文串字符串分割如出一辙。

### 子集

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int start,vector<int> &a){
    ans.push_back(path);
    if (start>=a.size()){
        return;
    }
    for (int i=start;i<a.size();i++){
        path.push_back(a[i]);
        backtract(i+1,a);
        path.pop_back();
    }
}
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        ans.clear();
        path.clear();
        backtract(0,nums);
        return ans;
    }
};
```

### 子集变式

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int start,vector<int> &a,vector<bool> &used){
    ans.push_back(path);
    if (start>=a.size()){
        return;
    }
    for (int i=start;i<a.size();i++){
        if (i>0&&a[i]==a[i-1]&&!used[i-1]){
            continue;
        }
        path.push_back(a[i]);
        used[i]=true;
        backtract(i+1,a,used);
        path.pop_back();
        used[i]=false;
    }
}
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        ans.clear();
        path.clear();
        vector<bool> used(nums.size(),false);
        sort(nums.begin(),nums.end());
        backtract(0,nums,used);
        return ans;
    }
};
```

### 非递减子序列

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(int start,vector<int> &a){
    if (path.size()>=2){
        ans.push_back(path);
    }
    if (start>=a.size()){
        return;
    }
    int used[210];
    memset(used,0,sizeof(used));
    for (int i=start;i<a.size();i++){
        if ((!path.empty()&&a[i]<path.back())||used[a[i]+100]){
            continue;
        }
        path.push_back(a[i]);
        used[a[i]+100]=1;
        backtract(i+1,a);
        path.pop_back();
    }
}
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        ans.clear();
        path.clear();
        backtract(0,nums);
        return ans;
    }
};
```

这个去重逻辑和旧版的实现的是同一件事，都是在做同层去重，只不过由于这个无法排序，只能换一个思路了。

> 非常棒的问题 👍
> “**什么时候需要同层去重**”是掌握回溯算法的关键之一。
> 很多人背了模板却写不好题，根本原因就是没理解**“为什么、什么时候需要同层去重”**。
> 下面我们从直觉、场景和代码三个角度讲透 👇
>
> ------
>
> ## 🧩 一、先明确：什么是“同层去重”？
>
> 在回溯树中：
>
> - “**同层**”指的是：当前递归函数的一次 for 循环范围；
> - “**去重**”指的是：在这一层循环中，不让相同数值重复选取，避免产生重复分支。
>
> ### 🌰 举例
>
> 以 `nums = [1, 2, 2]` 为例。
>
> #### 没有去重：
>
> 回溯树的前两层可能会生成重复分支：
>
> ```
>         []
>       / | \
>     1   2   2   ← 两个 2 是重复选择（同层）
> ```
>
> 于是生成两个相同子集 `[2]`。
>
> #### 有同层去重：
>
> 我们在本层循环时记录已经选过的值：
>
> ```
> used = {2}
> ```
>
> 第二个 2 就被跳过了。
>
> ------
>
> ## 🧠 二、什么时候要同层去重？
>
> ### ✅ 1. 当输入数据中存在**重复元素**，且顺序不影响结果时
>
> 典型例子：
>
> | 题目                          | 场景         | 是否要同层去重 |
> | ----------------------------- | ------------ | -------------- |
> | LC39 组合总和 (no duplicates) | 无重复元素   | ❌              |
> | LC40 组合总和 II              | 元素可能重复 | ✅              |
> | LC78 子集 I                   | 无重复元素   | ❌              |
> | LC90 子集 II                  | 有重复元素   | ✅              |
>
> 这些题目中，**相同的数出现在同一层时**，代表“同一种选择方式重复出现”，要去掉。
>
> ------
>
> ### ✅ 2. 当题目要求结果“去重”，但输入中存在相同数时
>
> **核心判断**：
>
> > 如果题目保证输入中“有重复”，但要求输出“无重复”，
> > 那么就必须进行**同层去重**。
>
> 同层去重 = 避免**同一个递归深度**重复使用相同数。
>
> ------
>
> ### ✅ 3. 当排序无法使用（需保持原顺序）时，用哈希同层去重
>
> 比如 LC491「递增子序列」：
>
> - 数组不能排序；
> - 相同值在同层出现时要跳过；
> - 所以要用 `used[]` / `unordered_set` 记录本层用过的数。
>
> ------
>
> ## ⚙️ 三、什么时候不需要同层去重？
>
> | 场景                                           | 是否需要 |
> | ---------------------------------------------- | -------- |
> | 输入数据本身不重复（题目保证）                 | ❌        |
> | 输出允许重复（例如排列问题）                   | ❌        |
> | 递归深度决定唯一结果（比如组合数 k）           | ❌        |
> | 不同分支必须保留所有情况（例如带位置的全排列） | ❌        |
>
> 🌰 举例：
>
> - LC46 全排列：即使有重复值，也通常是用 **used 全局标记数组** 去除“同一个元素多次用”，不是“同层去重”。
> - LC77 组合：数字从 1..n，本身不重复，无需同层去重。
>
> ------
>
> ## 📘 四、一句话口诀
>
> > **同层去重 = 为了去掉同一递归深度中“值相同”的分支**
> > 通常出现在「组合类」「子集类」「非排序但有重复元素」的问题中。
>
> ------
>
> ## ✅ 五、小结对比
>
> | 场景                   | 是否排序 | 去重策略        | 用法示例                       |
> | ---------------------- | -------- | --------------- | ------------------------------ |
> | 数组可排序，顺序不重要 | ✅        | 相邻跳过        | LC90 子集 II、LC40 组合总和 II |
> | 数组不可排序（需保序） | ❌        | 哈希标记 used[] | LC491 递增子序列               |
> | 数组无重复             | —        | 不去重          | LC78 子集 I、LC39 组合总和     |
>
> ------
>
> 是否希望我帮你画一张「同层去重 vs 跨层允许重复」的回溯树图？
>  那张图能一眼看懂为什么只在“同层”去重而“跨层”不去重。

### 全排列

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(vector<int> &a,vector<bool> &used){
    if (path.size()==a.size()){
        ans.push_back(path);
    }
    for (int i=0;i<a.size();i++){
        if (!used[i]){
            path.push_back(a[i]);
            used[i]=true;
            backtract(a,used);
            path.pop_back();
            used[i]=false;
        }
    }
}
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        ans.clear();
        path.clear();
        vector<bool> used(nums.size(),false);
        backtract(nums,used);
        return ans;
    }
};
```

组合，子集问题需要传入start，排列不用。

### 全排列（可重复版）

```cpp
vector<vector<int>> ans;
vector<int> path;
void backtract(vector<int> &a,vector<bool> &used){
    if (path.size()==a.size()){
        ans.push_back(path);
        return;
    }
    for (int i=0;i<a.size();i++){
        if (i>0&&a[i]==a[i-1]&&!used[i-1]){
            continue;
        }
        if (used[i]==false){
            path.push_back(a[i]);
            used[i]=true;
            backtract(a,used);
            path.pop_back();
            used[i]=false;
        }
    }
}
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        ans.clear();
        path.clear();
        sort(nums.begin(),nums.end());
        vector<bool> used(nums.size(),false);
        backtract(nums,used);
        return ans;
    }
};
```

加一个去重逻辑。

### 重新安排行程（lk332）

```cpp
unordered_map<string,map<string,int>> target;
bool backtract(int num,vector<string> &a){
    if (a.size()==num+1){
        return true;
    }
    for (pair<const string,int> &t: target[a[a.size()-1]]){
        if (t.second>0){
            a.push_back(t.first);
            t.second--;
            if (backtract(num,a)) return true;
            a.pop_back();
            t.second++;
        }
    }
    return false;
}

class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        target.clear();
        vector<string> ans;
        for (const vector<string> &v:tickets){
            target[v[0]][v[1]]++;
        }
        ans.push_back("JFK");
        backtract(tickets.size(),ans);
        return ans;
    }
};
```

这个感觉像图论的题目，事实上和我以前写过的那种找一条连续合法路径的题目很像，都是用回溯的方法连续探索一条路到死，然后在循环内部搞嵌套。

这个其实就是图论了，下面的过程和建图并没有什么差别。

### N皇后

```cpp
vector<vector<string>> ans;
bool isValid(int row, int col, vector<string>& chessboard, int n) {
    // 检查列
    for (int i = 0; i < row; i++) { // 这是一个剪枝
        if (chessboard[i][col] == 'Q') {
            return false;
        }
    }
    // 检查 45度角是否有皇后
    for (int i = row - 1, j = col - 1; i >=0 && j >= 0; i--, j--) {
        if (chessboard[i][j] == 'Q') {
            return false;
        }
    }
    // 检查 135度角是否有皇后
    for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (chessboard[i][j] == 'Q') {
            return false;
        }
    }
    return true;
}
void backtract(int n,int row,vector<string> &m){
    if (row==n){
        ans.push_back(m);
        return;
    }
    for (int col=0;col<n;col++){
        if (isValid(row,col,m,n)){
            m[row][col]='Q';
            backtract(n,row+1,m);
            m[row][col]='.';
        }
    }
}
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        ans.clear();
        vector<string> m(n,string(n,'.'));
        backtract(n,0,m);
        return ans;
    }
};
```

### 解数独

```cpp
bool isValid(int row, int col, char val, vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) { // 判断行里是否重复
        if (board[row][i] == val) {
            return false;
        }
    }
    for (int j = 0; j < 9; j++) { // 判断列里是否重复
        if (board[j][col] == val) {
            return false;
        }
    }
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == val ) {
                return false;
            }
        }
    }
    return true;
}
bool backtract(vector<vector<char>> &board){
    for (int i=0;i<board.size();i++){
        for (int j=0;j<board[0].size();j++){
            if (board[i][j]=='.'){
                for (char c='1';c<='9';c++){
                    if (isValid(i,j,c,board)){
                        board[i][j]=c;
                        if (backtract(board)) return true;
                        board[i][j]='.';
                    }
                }
                return false;
            }
        }
    }
    return true;
}
class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtract(board);
    }
};
```

无论是“重新安排行程”还是“N皇后”“解数独”，这几道题目很明显都在做一件事情，就是持续探索一种可能，直到不行为止，再回溯。

## 贪心理论

### 分发饼干

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int ans=0;
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int one=0,two=0;
        while (one<g.size()&&two<s.size()){
            if (g[one]<=s[two]){
                ans++;
                one++;
                two++;
            }else if(g[one]>s[two]){
                two++;
            }
        }
        return ans;
    }
};
```

### 摆动序列

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n=nums.size();
        if (n<=1) return n;
        int prediff=nums[1]-nums[0];
        int ans;
        if (prediff==0){
            ans=1;
        }else{
            ans=2;
        }
        for (int i=2;i<n;i++){
            int diff=nums[i]-nums[i-1];
            if ((diff>0&&prediff<=0)||(diff<0&&prediff>=0)){
                ans++;
                prediff=diff;
            }
        }
        return ans;
    }
};
```

### 最大子数组和（24机考原题）

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        int ans=-100000;
        int res=0;
        for (int i=0;i<n;i++){
            res+=nums[i];
            if (res>ans){
                ans=res;
            }
            if (res<0) res=0;
        }
        return ans;
    }
};
```

大代老师神了，这个思路比动态规划好想多了。

简单来说，如果这个数加到负数了，那就直接重开。

### 买卖股票的最佳时间

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        int ans=0;
        for (int i=1;i<n;i++){
            ans+=max(prices[i]-prices[i-1],0);
        }
        return ans;
    }
};
```

我确实没想到了。

这个东西由于时间无关，所以就是越早卖越好，只要相邻出现正利益，立刻买入无需多言。

### 跳跃游戏

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n=nums.size();
        if (n==1) return 1;
        int cover=0;
        for (int i=0;i<=cover;i++){
            cover=max(i+nums[i],cover);
            if (cover>=n-1) return 1;
        }
        return 0;
    }
};
```

### 跳跃游戏变式 

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n=nums.size();
        if (n==1) return 0;
        int ans=0;
        int ptr=0;
        int end=1;
        while (end<n){
            int maxx=0;
            for (int i=ptr;i<end;i++){
                maxx=max(i+nums[i],maxx);
            }
            ptr=end;
            end=maxx+1;
            ans++;
        }
        return ans;
    }
};
```

你知道这个事情有多脑残吗，三个月前我交的题解，三个月后的我自己没看懂。

简单来说，ptr和end围成的区间就是每次可以选择的区间，懂了吗。

> 哈哈，这种感觉太真实了 😄——很多人回看自己几个月前写的贪心代码，尤其是这种“区间推进式”的写法，都会觉得：
>
> > “我当时是怎么想的？为什么 `end=1` ？”
>
> 别急，我们一步步还原你三个月前的思路。
>
> ------
>
> ## 🧩 先看关键初始化部分
>
> ```cpp
> int ans = 0;
> int ptr = 0;
> int end = 1;   // 👈 你现在不理解的地方
> ```
>
> 我们要理解 `end=1`，得先明白 **`ptr`、`end` 代表什么**。
>
> ------
>
> ## 🧭 变量语义（你写的风格）
>
> | 变量  | 含义                                                      |
> | ----- | --------------------------------------------------------- |
> | `ptr` | 当前层的**起始下标**                                      |
> | `end` | 当前层的**结束下标的下一个位置**（右开区间 `[ptr, end)`） |
> | `ans` | 已跳的次数                                                |
>
> 换句话说，每一轮 `while` 的含义是：
>
> > “我们现在在区间 `[ptr, end)` 内，尝试跳一次，看能到多远。”
>
> ------
>
> ## 🧠 为啥 `end=1`？
>
> 因为：
>
> - 初始时，**我们一开始就在下标 0**；
> - 那么当前层的范围应该是 **只包含第 0 个点**；
> - 用右开区间 `[ptr, end)` 表示区间时，如果 `ptr=0`，只想包含下标 `0`，那 `end` 应该是 `1`。
>
> 也就是说：
>
> > 初始化 `[ptr, end) = [0, 1)`
> >  表示「我当前在第 0 层，只能从位置 0 起跳」。
>
> ------
>
> ## 🧩 如果你把 `end=0` 或 `end=n`
>
> 我们看看会怎样：
>
> ### ❌ `end = 0`
>
> - 那么初始区间 `[0, 0)` 是空的；
> - 进 `while(end<n)` → true；
> - 但 `for (int i=ptr; i<end; i++)` 不会执行；
> - `maxx` 一直是 0；
> - 进入死循环。
>
> → 程序卡死。
>
> ------
>
> ### ❌ `end = n`
>
> - 那就是一开始认为“整段都在当前层”；
> - 跳跃逻辑全乱套；
> - 直接跳出 `while`，`ans=0`；
> - 输出错误。
>
> ------
>
> ### ✅ `end = 1`
>
> 这刚好表示「当前层只含下标 0」，完全符合初始状态。
>
> ------
>
> ## 🔄 一轮循环后
>
> 举例：`nums = [2,3,1,1,4]`
>
> | 循环                              | `[ptr, end)` | 含义              |
> | --------------------------------- | ------------ | ----------------- |
> | 初始                              | `[0, 1)`     | 当前只在第 0 号点 |
> | 遍历 i=0 → i+nums[i]=2 → `maxx=2` |              |                   |
> | 更新 `ptr=end=1`, `end=maxx+1=3`  |              |                   |
> | 表示下一层 `[1, 3)` = {1,2}       |              |                   |
>
> > 即从 0 能跳到范围 [1,2]，
> >  下一层就从这些点再继续跳。
>
> ------
>
> ## ✅ 一句话总结
>
> 你的 `end=1` 是为了：
>
> > 用 `[ptr, end)` 表示“当前层能到达的所有点”，
> >  初始时只在第 0 个点，所以要设置为 `[0, 1)`。
>
> ------
>
> ## 💬 小比喻
>
> 想象你在玩跳格子游戏：
>
> - `ptr` 是当前一层的**起点格子**；
> - `end` 是这层的**终点格子+1**；
> - 每层循环表示**你跳一次，能进入下一层可达格子范围**；
> - 初始化时，你只站在第 0 个格子上，所以 `[0,1)`。
>
> ------
>
> 要不要我帮你改写成“更自然语义”的版本（例如 `start` / `end` 不用右开区间）？
>  那样写出来看起来更直观一点（不需要 `+1`）。

### K次取反后最大化的数组和

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        while (k--){
            nums[0]=-nums[0];
            sort(nums.begin(),nums.end());
        }
        int ans=0;
        for (auto it:nums){
            ans+=it;
        }
        return ans;
    }
};
```

### 加油站

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        //-2 -2 -2 3 3
        //-1 -1 1
        vector<int> oil;
        int n=gas.size();
        int sum=0;
        int minn=100000;
        for (int i=0;i<n;i++){
            oil.push_back(gas[i]-cost[i]);
        }
        for (int i=0;i<n;i++){
            sum+=oil[i];
            if (sum<minn){
                minn=sum;
            }
        }
        if (sum<0) return -1;
        if (minn>=0) return 0;
        for (int i=n-1;i>=0;i--){
            minn+=oil[i];
            if (minn>=0){
                return i;
            }
        }
        return -1;
    }
};
```

### 柠檬水找零

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int n=bills.size();
        int h[11];
        memset(h,0,sizeof(h));
        for (int i=0;i<n;i++){
            if (bills[i]==5){
                h[5]++;
            }
            else if (bills[i]==10){
                if (h[5]==0){
                    return 0;
                }else{
                    h[5]--;
                    h[10]++;
                }
            }
            else if (bills[i]==20){
                if (h[10]>=1&&h[5]>=1){
                    h[10]--;
                    h[5]--;
                }else if (h[5]>=3){
                    h[5]-=3;
                }else{
                    return 0;
                }
            }
        }
        return 1;
    }
};
```

这里有三种金额，5，10，20。问题是要找零

我们可以发现的是，5泛用性较高对策性较强属于超大杯，10泛用性一般对策性较强属于大杯，而20泛用性极低对策性极低属于中杯。

每次消耗先消耗泛用性低的，留在手里没用，然后再去考虑泛用性高的。

### 分发糖果

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n=ratings.size();
        vector<int> ans(n,1);
        for (int i=1;i<=n-1;i++){
            if (ratings[i]>ratings[i-1]){
                ans[i]=ans[i-1]+1;
            }
        }
        for (int i=n-2;i>=0;i--){
            if (ratings[i]>ratings[i+1]){
                ans[i]=max(ans[i],ans[i+1]+1);
            }
        }
        int sum=0;
        for (int i=0;i<n;i++){
            sum+=ans[i];
        }
        return sum;
    }
};
```

分发糖果事实上是一类题，与这一题类似的还有PAT B1119

```cpp
//PAT B1119答案
#include <bits/stdc++.h>
using namespace std;

int n;
int main(){
    cin>>n;
    vector<int> panda(n);
    for (int i=0;i<n;i++){
        cin>>panda[i];
    }
    vector<int> ans(n,200);
    for (int i=1;i<n;i++){
        if (panda[i]>panda[i-1]){
            ans[i]=ans[i-1]+100;
        }
        else if(panda[i]==panda[i-1]){
            ans[i]=ans[i-1];
        }
    }
    for (int i=n-2;i>=0;i--){
        if (panda[i]>panda[i+1]){
            ans[i]=max(ans[i],ans[i+1]+100);
        }
        else if(panda[i]==panda[i+1]){
            ans[i]=max(ans[i],ans[i+1]);
        }
    }
    int sum=0;
    for (int i=0;i<n;i++){
        sum+=ans[i];
    }
    cout<<sum<<endl;
    return 0;
}
```

都是扫描两次。

### 根据身高重建队列

```cpp
bool cmp(vector<int> &a,vector<int> &b){
    if (a[0]==b[0]) return a[1]<b[1];
    return a[0]>b[0];
}

class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> que;
        for (int i=0;i<people.size();i++){
            int pos=people[i][1];
            que.insert(que.begin()+pos,people[i]);
        }
        return que;
    }
};
```

这个思路有点难想了。

### 用最少的箭引爆气球

```cpp
bool cmp(vector<int> &a,vector<int> &b){
    if (a[0]==b[0]) return a[1]<b[1];
    return a[0]>b[0];
}

class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> que;
        for (int i=0;i<people.size();i++){
            int pos=people[i][1];
            que.insert(que.begin()+pos,people[i]);
        }
        return que;
    }
};
```

事实上，这个是贪心里最有迹可循的题目，也就是重合线段模型了。

### 无重叠区间

```cpp
bool cmp(vector<int> &a,vector<int> &b){
    return a[1]<b[1];
}

class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n=intervals.size();
        sort(intervals.begin(),intervals.end(),cmp);
        int minn=intervals[0][1];
        int ans=1;
        for (int i=1;i<n;i++){
            if (intervals[i][0]>=minn){
                ans++;
                minn=intervals[i][1];
            }else{
                continue;
            }
        }
        return n-ans;
    }
};
```

### 划分字母区间

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n=s.size();
        vector<int> ans;
        unordered_map<char,int> h;
        for (int i=0;i<n;i++){
            h[s[i]]=i;
        }
        int idx=0;
        int pos=0;
        for (int i=0;i<n;i++){
            if (h[s[i]]>idx){
                idx=h[s[i]];
            }
            if (i==idx){
                ans.push_back(idx-pos+1);
                idx=h[s[i+1]];
                pos=i+1;
            }
        }
        return ans;
    }
};
```

何尝不是一种线段模型的表示形式，总的而言就是先统计出来每个字符最后出现的位置，然后再定义一个最大值为0，一旦遇到那个字母最后出现位置大于最大值，那么就更新最大值，一旦最大值和i相等了，那么直接保存一次答案，然后把初始指针移动到下一位，继续遍历。

```cpp
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] < b[0];
    }
    // 记录每个字母出现的区间
    vector<vector<int>> countLabels(string s) {
        vector<vector<int>> hash(26, vector<int>(2, INT_MIN));
        vector<vector<int>> hash_filter;
        for (int i = 0; i < s.size(); ++i) {
            if (hash[s[i] - 'a'][0] == INT_MIN) {
                hash[s[i] - 'a'][0] = i;
            }
            hash[s[i] - 'a'][1] = i;
        }
        // 去除字符串中未出现的字母所占用区间
        for (int i = 0; i < hash.size(); ++i) {
            if (hash[i][0] != INT_MIN) {
                hash_filter.push_back(hash[i]);
            }
        }
        return hash_filter;
    }
    vector<int> partitionLabels(string s) {
        vector<int> res;
        // 这一步得到的 hash 即为无重叠区间题意中的输入样例格式：区间列表
        // 只不过现在我们要求的是区间分割点
        vector<vector<int>> hash = countLabels(s);
        // 按照左边界从小到大排序
        sort(hash.begin(), hash.end(), cmp);
        // 记录最大右边界
        int rightBoard = hash[0][1];
        int leftBoard = 0;
        for (int i = 1; i < hash.size(); ++i) {
            // 由于字符串一定能分割，因此,
            // 一旦下一区间左边界大于当前右边界，即可认为出现分割点
            if (hash[i][0] > rightBoard) {
                res.push_back(rightBoard - leftBoard + 1);
                leftBoard = hash[i][0];
            }
            rightBoard = max(rightBoard, hash[i][1]);
        }
        // 最右端
        res.push_back(rightBoard - leftBoard + 1);
        return res;
    }
};
```

### 合并区间

```cpp
bool cmp(vector<int> &a,vector<int> &b){
    return a[0]<b[0];
}
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int n=intervals.size();
        vector<vector<int>> ans;
        sort(intervals.begin(),intervals.end());
        int minn=intervals[0][0];
        int maxx=intervals[0][1];
        for (int i=0;i<n;i++){
            if (intervals[i][0]>maxx){
                vector<int> path;
                path.push_back(minn);
                path.push_back(maxx);
                ans.push_back(path);
                minn=intervals[i][0];
                maxx=intervals[i][1];
            }else{
                maxx=max(intervals[i][1],maxx);
            }
        }
        vector<int> path;
        path.push_back(minn);
        path.push_back(maxx);
        ans.push_back(path);
        return ans;
    }
};
```

这个也是线段合并模板题，经过讲解我现在理解更深了。

### 单调递增的数字

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string str=to_string(n);
        int flag=str.size();
        for (int i=str.size()-1;i>0;i--){
            if (str[i-1]>str[i]){
                flag=i;
                str[i-1]--;
            }
        }
        for (int i=flag;i<str.size();i++){
            str[i]='9';
        }
        return stoi(str);
    }
};
```

> 好——下面我把 **贪心解（Greedy）** 的思路讲得尽可能清楚、逐步且可直接拿去写代码。目标是：理解为什么操作是正确的、每一步在做什么、常见边界如何处理，并给出简洁的实现与例子演示。
>
> # 要解决的问题（快速回顾）
>
> 给定正整数 `n`，找出最大的整数 `x`（`x ≤ n`），使得 `x` 的每一位从左到右**非递减**（即单调递增/非降：每一位 ≤ 下一位）。
>
> 示例：`n=332` → 答案 `299`。
>
> ------
>
> # 核心观察（关键不难但很重要）
>
> 若一个数的某一位 `i-1` 大于后一位 `i`（即 `s[i-1] > s[i]`），那么任何以当前前缀（包括 `s[i-1]`）开始并保持后面不变的数都**不是单调非降**。
>  要在不超过 `n` 的前提下得到尽可能大的单调数，你需要 **把违规点之前的某一位减 1**，然后把那个位置右边所有位都改成 `9`（因为改成 9 能在保证 ≤ n 的前提下把数尽量变大）。
>
> 举例直觉：`332`
>
> - 发现 `3 > 2`（位置 1 比位置 2 大），所以不能直接用 `332`。
> - 把 `s[1]`（第二位）减 1 → 得到 `3 2 ?`（现在前缀是 `32`）
> - 把后面全部改 `9` → `3 2 9`，但 `329` 仍然不是单调非降（因为 `3 > 2`），所以你应当继续向左看，把 `s[0]`（第一位）减 1 → `2 ? ?`，然后把所有右边改 `9` → `299`，这是合法且最大的。
>
> ------
>
> # 具体算法（一步步执行）
>
> 1. 把 `n` 转成字符串 `s`（比如 `"332"`）。
> 2. 从右往左扫描索引 `i = len(s)-1` 到 `1`：
>    - 如果发现 `s[i-1] > s[i]`：
>      - 把 `s[i-1]--`（把左位减 1），
>      - 记录一个位置 `mark = i`（表示从 `i` 开始到末尾要全部改成 `9`）。
> 3. 扫描结束后，把从 `mark` 到末尾的所有字符都置为 `'9'`。
> 4. 将字符串转回整数返回（注意可能有前导 `0`，如 `1000 -> 0999 -> 999`，转换为整数会自动处理）。
>
> **实现细节**：初始化 `mark = s.size()`（表示如果从未发现违规就无需改成 9）。每次发现 `s[i-1] > s[i]`，执行 `s[i-1]--` 并 `mark = i`（注意不要立刻把右边改成 9 - 先记录位置，继续向左扫描，可能需要多次左减）。
>
> ------
>
> # 为什么这样是正确的（证明直观版）
>
> - 当 `s[i-1] > s[i]` 时，任何保持前缀不变的数都不可能合法（不单调）。要让前缀变得合法，唯一可行的操作是把前缀中某一位减小（向左找第一个能减的位置），因为减小某一左侧位可以使得后面做任意更大填充而仍 ≤ `n`。
> - 为了在不超过 `n` 的前提下最大化结果，把该位减 1 后，右边所有位都应设为最大可能（即 `9`）。
> - 向左继续检查是必要的：减了左边某位之后，可能出现新的 `s[j-1] > s[j]`（因为你减小了 `s[j]`），所以需要继续向左处理直到没有违反。上述“先记录`mark`继续向左”正是实现这个逻辑的高效方法。
> - 最终形成的数是小于等于原数且单调非降的最大数，因为对任何更大的比它还小的候选数都会在某处违反条件或超过 `n`。
>
> ------
>
> # 时间与空间复杂度
>
> - 时间：O(d)，d = 数字的位数（将近 `log10(n)`），非常快。
> - 空间：O(d)（字符串存储），可认为 O(1) 额外空间。
>
> ------
>
> # 代码（C++，简洁易懂）
>
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> 
> int monotoneIncreasingDigits(int n) {
>     string s = to_string(n);
>     int mark = s.size();
>     for (int i = s.size() - 1; i > 0; --i) {
>         if (s[i-1] > s[i]) {
>             s[i-1]--;
>             mark = i;
>         }
>     }
>     for (int i = mark; i < (int)s.size(); ++i) s[i] = '9';
>     return stoi(s);
> }
> ```
>
> （Python 版本也很短，可按需提供。）
>
> ------
>
> # 例子逐步演示（两例）
>
> **例 1：n = 332**
>  s = "332", mark = 3
>  i=2: s[1]=3, s[2]=2 → 3>2 ✓ → s[1]-- => s="322", mark=2
>  i=1: s[0]=3, s[1]=2 → 3>2 ✓ → s[0]-- => s="222", mark=1
>  end loop -> 把从 mark=1 到末尾全设为 '9' -> s = "299" -> 返回 299。
>
> **例 2：n = 1234**
>  s="1234"，从右往左检查没有 s[i-1] > s[i] 的情况，mark 保持为 4（s.size），不用变 9，直接返回 1234。
>
> ------
>
> # 常见边界 / 注意点
>
> - `n` 本身就是单调非降时，直接返回 `n`（算法会自动做到）。
> - 会出现类似 `1000` 的情形：
>    s = "1000"
>    i=3: 0 !> 0
>    i=2: 0 !> 0
>    i=1: 1 > 0 → s[0]-- => '0', mark=1 → 最后从 1 开始全部设 9 => "0999" -> stoi -> 999，正确。
> - 字符串减法 `s[i-1]--` 对字符安全（'0' 到 '9' 范围里减 1 都有效）；若出现 `'0'--` 结果是 `'/'`（ASCII），但不会发生因为当 `s[i-1] > s[i]` 且 `s[i]` 最小为 '0' 时 `s[i-1]` 至少是 '1'，故减 1 后仍是数字字符。
>
> ------
>
> # 直观理解（一句话）
>
> 把从右到左第一个“下降”的点往左找到能调整的位，把它减 1，然后把右边全部填 9；重复直到整个数不再下降 —— 这样得到的不超过原数的单调非降数是**最大的**。
>
> ------
>
> 如果你想，我可以再：
>
> - 给出 Python 实现；或
> - 把“为什么要继续向左”的情形用一个小动画式的多步骤例子（比如 `n=3329`）画出来。
>
> 你想看哪一个？

我其实还去想了回溯解，但是回溯解法用脑子稍微想想就知道会超时了，这边只是做个参考

```cpp
#include <bits/stdc++.h>
using namespace std;

long long n;
string n_str;
long long best = 0;

void backtrack(string cur, int last_digit) {
    // 把当前字符串转成数字看看是不是一个候选解
    if (!cur.empty()) {
        long long val = stoll(cur);
        if (val <= n) best = max(best, val);
        else return; // 如果已经超过 n，就没必要继续往下构造了
    }

    // 如果长度超过 n 的位数，也没必要继续
    if (cur.size() >= n_str.size()) return;

    // 尝试添加新的一位
    for (int d = last_digit; d <= 9; d++) {
        backtrack(cur + (char)('0' + d), d);
    }
}

int main() {
    cin >> n;
    n_str = to_string(n);

    backtrack("", 0);
    cout << best << endl;
    return 0;
}
```

## 动态规划理论

### 斐波那契数列

```cpp
class Solution {
public:
    int fib(int n) {
        int f[35];
        f[0]=0,f[1]=1;
        for (int i=2;i<=30;i++){
            f[i]=f[i-1]+f[i-2];
        }
        return f[n];
    }
};
```

### 爬楼梯

```cpp
class Solution {
public:
    int climbStairs(int n) {
        //dp[i]的意思是爬上第i层楼梯有几种方法
        int dp[46];
        dp[1]=1;
        dp[2]=2;
        for (int i=3;i<=45;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
};
```

### 用最小的花费爬楼梯

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector<int> dp(n+1);
        dp[0]=0;
        dp[1]=0;
        for (int i=2;i<=n;i++){
            dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
};
```

### 不同路径

#### 题解一：深度优先搜索

```cpp
class Solution {
private:
    int dfs(int i, int j, int m, int n) {
        if (i > m || j > n) return 0; // 越界了
        if (i == m && j == n) return 1; // 找到一种方法，相当于找到了叶子节点
        return dfs(i + 1, j, m, n) + dfs(i, j + 1, m, n);
    }
public:
    int uniquePaths(int m, int n) {
        return dfs(1, 1, m, n);
    }
};
```

这个写法是比较直观的思路，因为这个题事实上就是一个图，但是问题是会超时

#### 题解二：动态规划（正解）

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int>(n));
        dp[0][0]=1;
        for (int i=1;i<n;i++){
            dp[0][i]=1;
        }
        for (int i=1;i<m;i++){
            dp[i][0]=1;
        }
        for (int i=1;i<m;i++){
            for (int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

和洛谷上那题马的遍历可能是完全一样的题目，这一题可能还比较简单没有加障碍物，也没有限制移动规则。

### 不同路径变式

#### 动态规划解法：

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m=obstacleGrid.size();
        int n=obstacleGrid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
        if (obstacleGrid[m-1][n-1]==1||obstacleGrid[0][0]==1){
            return 0;
        }
        dp[0][0]=1;
        for (int i=1;i<m;i++){
            if (obstacleGrid[i][0]==1) break;
            else dp[i][0]=1;
        }
        for (int i=1;i<n;i++){
            if (obstacleGrid[0][i]==1) break;
            else dp[0][i]=1;
        }
        for (int i=1;i<m;i++){
            for (int j=1;j<n;j++){
                if (obstacleGrid[i][j]==1) continue;
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

如果在最外围出现了障碍物，那么和障碍物同一条直线上且之后的都无法到达，都是0

#### 深度优先搜索+记忆化

```cpp
class Solution {
public:
    int m, n;
    vector<vector<int>> memo;
    int dfs(vector<vector<int>>& grid, int i, int j) {
        if (i >= m || j >= n || grid[i][j] == 1) return 0;
        if (i == m - 1 && j == n - 1) return 1;
        if (memo[i][j] != -1) return memo[i][j];
        return memo[i][j] = dfs(grid, i + 1, j) + dfs(grid, i, j + 1);
    }

    int uniquePathsWithObstacles(vector<vector<int>>& grid) {
        m = grid.size(), n = grid[0].size();
        memo.assign(m, vector<int>(n, -1));
        return dfs(grid, 0, 0);
    }
};
```

### 整数拆分

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1);
        dp[2]=1;
        for (int i=3;i<=n;i++){
            for (int j=1;j<=i/2;j++){
                dp[i]=max(dp[i],max(j*dp[i-j],j*(i-j)));
            }
        }
        return dp[n];
    }
};
```

我先吐槽，你再给我十辈子我都想不出来这种思路。

### 0/1背包理论

![](https://s3.bmp.ovh/imgs/2025/10/16/bbedccb90d144180.png)

#### 例题：携带研究材料（01背包模板题）

题目描述

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的空间，并且具有不同的价值。 

小明的行李空间为 N，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料只能选择一次，并且只有选与不选两种选择，不能进行切割。

输入描述

第一行包含两个正整数，第一个整数 M 代表研究材料的种类，第二个正整数 N，代表小明的行李空间。

第二行包含 M 个正整数，代表每种研究材料的所占空间。 

第三行包含 M 个正整数，代表每种研究材料的价值。

输出描述

输出一个整数，代表小明能够携带的研究材料的最大价值。

输入示例

```
6 1
2 2 3 1 5 2
2 3 1 5 4 3
```

输出示例

提示信息

小明能够携带 6 种研究材料，但是行李空间只有 1，而占用空间为 1 的研究材料价值为 5，所以最终答案输出 5。 

数据范围：  
1 <= N <= 5000  
1 <= M <= 5000  
研究材料占用空间和价值都小于等于 1000

题解：

```cpp
#include <bits/stdc++.h>
using namespace std;
int m,n;
int main(){
    cin>>m>>n;
    vector<int> value(m);
    vector<int> weight(m);
    for (int i=0;i<m;i++){
        cin>>weight[i];
    }
    for (int i=0;i<m;i++){
        cin>>value[i];
    }
    vector<vector<int>> dp(m,vector<int>(n+1,0));
    for (int i=weight[0];i<n+1;i++){
        dp[0][i]=value[0];
    }
    for (int i=1;i<m;i++){
        for (int j=0;j<n+1;j++){
            if (j<weight[i]){
                dp[i][j]=dp[i-1][j];
            }else{
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i]);
            }
        }
    }
    cout<<dp[m-1][n];
    return 0;
}
```

即**dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少**。

**要时刻记着这个dp数组的含义，下面的一些步骤都围绕这dp数组的含义进行的**，如果哪里看懵了，就来回顾一下i代表什么，j又代表什么

#### 分割等和子集

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n=nums.size();
        int sum=0;
        vector<int> dp(10001,0);
        for (int i=0;i<n;i++){
            sum+=nums[i];
        }
        if (sum%2==1) return 0;
        int t=sum/2;
        for (int i=0;i<n;i++){
            for (int j=t;j>=nums[i];j--){
                dp[j]=max(dp[j],dp[j-nums[i]]+nums[i]);
            }
        }
        if (dp[t]==t) return 1;
        return 0;
    }
};
```

这个真的很扯你知道吗，不看我这辈子想不到要dp。

#### 最后一块石头的重量

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int n=stones.size();
        int sum=0;
        for (int i=0;i<n;i++){
            sum+=stones[i];
        }
        vector<int> dp(1510,0);
        for (int i=0;i<n;i++){
            for (int j=sum/2;j>=stones[i];j--){
                dp[j]=max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return sum-2*dp[sum/2];
    }
};
```

这个题目和洛谷的那个分作业有点像，都是01背包求最小值的，应该就是一个模板了

一维dp从后往前面遍历的目的其实是为了防止同一个物品被多次选择

更详细的看下面

dp[j]为 容量为j的背包所背的最大价值。等于就是把原本的二维dp的其中一个维度都拷贝到一个一维数组上了

#### 目标和

```cpp
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int neg = diff / 2;
        int[] dp = new int[neg + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int j = neg; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        return dp[neg];
    }
}
```

![](https://s3.bmp.ovh/imgs/2025/10/17/8ad0d9723cb46504.png)

由于 dp 的每一行的计算只和上一行有关，因此可以使用滚动数组的方式，去掉 dp 的第一个维度，将空间复杂度优化到 O(neg)。

实现时，内层循环需采用倒序遍历的方式，这种方式保证转移来的是 dp[i−1][] 中的元素值。

#### 1和0

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1,vector<int>(n+1)); 
        for (string s:strs){
            int zero=0,one=0;
            for (char c:s){
                if (c=='0') zero++;
                else one++;
            }
            for (int i=m;i>=zero;i--){
                for (int j=n;j>=one;j--){
                    dp[i][j]=max(dp[i][j],dp[i-zero][j-one]+1);
                }
            }
        }
        return dp[m][n];
    }
};
```

### 完全背包理论

![](https://s3.bmp.ovh/imgs/2025/10/18/1fa5f5df155c4a42.png)

完全背包由于每种物品的状态不止有两种，所以递推公式与01背包存在着一些不同。

当然，在初始化上也存在着不同，由于完全背包中可以选择的物品存在着无限多个，因此它的初始化应该是

```cpp
for (int i = 1; i < weight.size(); i++) {  // 当然这一步，如果把dp数组预先初始化为0了，这一步就可以省略，但很多同学应该没有想清楚这一点。
    dp[i][0] = 0;
}

// 正序遍历，如果能放下就一直装物品0
for (int j = weight[0]; j <= bagWeight; j++)
    dp[0][j] = dp[0][j - weight[0]] + value[0];
```

#### 例题（模板）

![](https://s3.bmp.ovh/imgs/2025/10/18/b66f26eeeb1481f7.png)

题解

```c++
#include <bits/stdc++.h>
using namespace std;
int n,v;
int main(){
    cin>>n>>v;
    vector<int> weight(n);
    vector<int> value(n);
    for (int i=0;i<n;i++){
        cin>>weight[i]>>value[i];
    }
    vector<vector<int>> dp(n,vector<int>(v+1,0));
    for (int i=0;i<n;i++){
        dp[i][0]=0;
    }
    for (int i=weight[0];i<=v;i++){
        dp[0][i]=dp[0][i-weight[0]]+value[0];
    }
    for (int i=1;i<n;i++){
        for (int j=0;j<=v;j++){
            if (j<weight[i]){
                dp[i][j]=dp[i-1][j];
            }else{
                dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i]]+value[i]);
            }
        }
    }
    cout<<dp[n-1][v]<<endl;
    return 0;
}
```

这个就是完完全全的完全背包模板题了，没有一点绕弯子的那种

#### 零钱兑换

![](https://s3.bmp.ovh/imgs/2025/10/18/e4780fb6e99f8d37.png)

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int bagSize = amount;

        vector<vector<uint64_t>> dp(coins.size(), vector<uint64_t>(bagSize + 1, 0));

        // 初始化最上行
        for (int j = 0; j <= bagSize; j++) {
            if (j % coins[0] == 0) dp[0][j] = 1;
        }
        // 初始化最左列
        for (int i = 0; i < coins.size(); i++) {
            dp[i][0] = 1;
        }
        // 以下遍历顺序行列可以颠倒
        for (int i = 1; i < coins.size(); i++) { // 行，遍历物品
            for (int j = 0; j <= bagSize; j++) { // 列，遍历背包
                if (coins[i] > j) dp[i][j] = dp[i - 1][j]; 
                else dp[i][j] = dp[i - 1][j] +  dp[i][j - coins[i]];
            }
        }
        return dp[coins.size() - 1][bagSize];
    }
};
```

一维dp写法

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<uint64_t> dp(amount + 1, 0); // 防止相加数据超int
        dp[0] = 1; // 只有一种方式达到0
        for (int i = 0; i < coins.size(); i++) { // 遍历物品
            for (int j = coins[i]; j <= amount; j++) { // 遍历背包
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount]; // 返回组合数
    }
};
```

#### 组合总和（变式2）

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 0; i <= target; i++) { // 遍历背包
            for (int j = 0; j < nums.size(); j++) { // 遍历物品
                if (i - nums[j] >= 0 && dp[i] <= INT_MAX - dp[i - nums[j]]) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```

![](https://s3.bmp.ovh/imgs/2025/10/18/dad5b012e2e89963.png)

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

***简单来说就是：顺序有关就要先遍历背包，以防出问题，无关就先遍历物品。***

#### 01背包与完全背包在写法上的差别

> 非常好的问题👏，这个是背包问题的核心区分点之一。
>  我们来系统地讲清楚 **01 背包** 和 **完全背包** 在「一维 DP」时的**递推公式差别**。
>
> ------
>
> ## 🧩 一、共同点（大框架相同）
>
> 无论是 01 背包还是 完全背包，它们都要解决这样的问题：
>
> > 在容量为 `V` 的背包中，放若干个物品，使得价值最大。
>
> 每个物品 `i` 有：
>
> - 重量（或体积）`w[i]`
> - 价值 `v[i]`
>
> 定义一维数组：
>
> ```
> dp[j] = 容量为 j 时的最大价值
> ```
>
> ------
>
> ## 🧱 二、01 背包公式（每件只能选一次）
>
> ### 状态转移方程：
>
> [
>  dp[j] = \max(dp[j], dp[j - w[i]] + v[i])
>  ]
>
> ### 🔁 遍历顺序（**从右往左**）：
>
> ```cpp
> for (int i = 0; i < n; i++) {         // 枚举物品
>     for (int j = V; j >= w[i]; j--) { // 从大到小
>         dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
>     }
> }
> ```
>
> #### 💡 为什么倒序？
>
> 防止同一个物品被重复使用。
>  如果从左到右更新，那么在处理第 i 个物品时，
>  `dp[j - w[i]]` 可能已经被第 i 个物品更新过，就会出现「重复选」的错误。
>
> ------
>
> ## 🌀 三、完全背包公式（每件可以无限次选）
>
> ### 状态转移方程：
>
> [
>  dp[j] = \max(dp[j], dp[j - w[i]] + v[i])
>  ]
>
> 👉 **看起来公式一样**，但遍历顺序不同！
>
> ### 🔁 遍历顺序（**从左往右**）：
>
> ```cpp
> for (int i = 0; i < n; i++) {         // 枚举物品
>     for (int j = w[i]; j <= V; j++) { // 从小到大
>         dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
>     }
> }
> ```
>
> #### 💡 为什么正序？
>
> 因为完全背包允许同一个物品被多次使用。
>  当你从左往右更新时，`dp[j - w[i]]` 是**本轮**更新后的值，
>  代表“可以继续选当前物品 i”。
>
> ------
>
> ## 🎯 四、对比总结表
>
> | 项目           | 01 背包                                              | 完全背包                                           |
> | -------------- | ---------------------------------------------------- | -------------------------------------------------- |
> | 是否可重复     | ❌ 每个物品只能选一次                                 | ✅ 可重复选                                         |
> | 状态方程       | `dp[j] = max(dp[j], dp[j - w[i]] + v[i])`            | 同上                                               |
> | 遍历顺序       | `for j = V → w[i]`（倒序）                           | `for j = w[i] → V`（正序）                         |
> | 原因           | 防止重复使用物品                                     | 允许重复使用物品                                   |
> | 二维版对应转移 | `dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])` | `dp[i][j] = max(dp[i-1][j], dp[i][j-w[i]] + v[i])` |
>
> ------
>
> ## 🧠 五、直观理解图
>
> 假设物品重 2，价值 3，容量 6。
>
> | 过程             | 说明                                                  |
> | ---------------- | ----------------------------------------------------- |
> | **01 背包倒序**  | 计算 dp[6] 时不会用到刚更新的 dp[4]（防止重复选）     |
> | **完全背包正序** | 计算 dp[6] 时会用到刚更新的 dp[4]（表示可以再选一次） |
>
> ------
>
> ## 🧩 六、举个例子对比
>
> 假设：
>
> ```
> 重量 w = [1, 3, 4]
> 价值 v = [15, 20, 30]
> 容量 V = 4
> ```
>
> ### 01 背包结果：
>
> - 最优选法：选 4 重量物品 → 价值 30
>
> ### 完全背包结果：
>
> - 最优选法：选 4 个 1 重量物品 → 价值 60
>
> ------
>
> ## ✅ 总结一句话记忆
>
> | 名称         | 一维遍历方向 | 思想记忆   |
> | ------------ | ------------ | ---------- |
> | **01 背包**  | 从右往左     | 防止重复选 |
> | **完全背包** | 从左往右     | 允许重复选 |
>
> ------
>
> 如果你想，我可以帮你画出一个具体 dp 表格（例如容量=5, w=[2,3], v=[3,4]），
>  并演示在正序 vs 倒序时 dp 数组是如何变化的。
>  要我画那个表吗？

#### 爬楼梯

![](https://s3.bmp.ovh/imgs/2025/10/18/90c9fdbfb3f9e1a8.png)

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
int main(){
    cin>>n>>m;
    vector<int> dp(n+1,0);
    dp[0]=1;
    for (int i=1;i<=n;i++){
        for (int j=1;j<=m;j++){
            if (i>=j){
                dp[i]+=dp[i-j];
            }
        }
    }
    cout<<dp[n]<<endl;
}
```

#### 零钱兑换（变）

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int n=coins.size();
        vector<int> dp(amount+1,INT_MAX);
        dp[0]=0;
        for (int i=0;i<n;i++){
            for (int j=coins[i];j<=amount;j++){
                if (dp[j-coins[i]]!=INT_MAX){
                    dp[j]=min(dp[j],dp[j-coins[i]]+1);
                }
            }
        }
        if (dp[amount]==INT_MAX) return -1;
        return dp[amount];
    }
};
```

#### 完全平方数

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> a;
        for (int i=1;i*i<=n;i++){
            a.push_back(i*i);
        }
        int m=a.size();
        vector<int> dp(n+1,INT_MAX);
        dp[0]=0;
        for (int i=0;i<m;i++){
            for (int j=a[i];j<=n;j++){
                if (dp[j-a[i]]!=INT_MAX){
                    dp[j]=min(dp[j],dp[j-a[i]]+1);
                }
            }
        }
        return dp[n];
    }
};
```

#### 单词拆分

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for (int i = 1; i <= s.size(); i++) {   // 遍历背包
            for (int j = 0; j < i; j++) {       // 遍历物品
                string word = s.substr(j, i - j); //substr(起始位置，截取的个数)
                if (wordSet.find(word) != wordSet.end() && dp[j]) {
                    dp[i] = true;
                }
            }
        }
        return dp[s.size()];
    }
};
```

### 多重背包理论

![](https://s3.bmp.ovh/imgs/2025/10/18/8a345f9182f1b009.png)

```c++
#include<iostream>
#include<vector>
using namespace std;
int main() {
    int bagWeight,n;
    cin >> bagWeight >> n;
    vector<int> weight(n, 0);
    vector<int> value(n, 0);
    vector<int> nums(n, 0);
    for (int i = 0; i < n; i++) cin >> weight[i];
    for (int i = 0; i < n; i++) cin >> value[i];
    for (int i = 0; i < n; i++) cin >> nums[i];

    vector<int> dp(bagWeight + 1, 0);

    for(int i = 0; i < n; i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            // 以上为01背包，然后加一个遍历个数
            for (int k = 1; k <= nums[i] && (j - k * weight[i]) >= 0; k++) { // 遍历个数
                dp[j] = max(dp[j], dp[j - k * weight[i]] + k * value[i]);
            }
        }
    }

    cout << dp[bagWeight] << endl;
}
```

多重背包事实上就是一种01背包的变体，只要用上面的方法将每个物品全部展开就行，然后就可以当01背包处理了。

### 背包理论总结篇

![](https://s3.bmp.ovh/imgs/2025/10/18/db35ad9e1bd76fcc.png)

![](https://s3.bmp.ovh/imgs/2025/10/18/1fed0382f1592c33.png)

![](https://s3.bmp.ovh/imgs/2025/10/18/40b9a39892d761ef.png)

### 相邻约束DP

#### 打家劫舍

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if (n==1) return nums[0];
        vector<int> dp(n,0);
        dp[0]=nums[0];
        dp[1]=max(nums[0],nums[1]);
        for (int i=2;i<n;i++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[n-1];
    }
};
```

这个题型叫做相邻约束DP，就是相邻数不能选。

#### 打家劫舍（变式二）

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if (n==1) return nums[0];
        int res1=robans(nums,1,n-1);
        int res2=robans(nums,0,n-2);
        return max(res1,res2);
    }
    int robans(vector<int> &nums,int start,int end){
        int n=nums.size();
        if (start==end) return nums[start];
        vector<int> dp(n,0);
        dp[start]=nums[start];
        dp[start+1]=max(nums[start],nums[start+1]);
        for (int i=start+2;i<=end;i++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[end];
    }
};
```

事实上也就只有两种情况，只偷头/只偷尾，至于只偷中间，这个情况是不可能的，不会获得最佳答案。

### 状态机DP

#### 买卖股票的最佳时机

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if (len == 0) return 0;
        vector<vector<int>> dp(len, vector<int>(2));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[len - 1][1];
    }
};//DP写法

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        int res=0;
        int minn=100010;
        for (int i=0;i<n;i++){
            minn=min(minn,prices[i]);
            res=max(res,prices[i]-minn);
        }
        return res;
    }
};//贪心写法
```

我觉得这题贪心是最优解有没有懂的（

如果第i天持有股票即`dp[i][0]`， 那么可以由两个状态推出来

- 第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：`dp[i - 1][0]`
- 第i天买入股票，所得现金就是买入今天的股票后所得现金即：-prices[i]

那么dp[i][0]应该选所得现金最大的，所以`dp[i][0] = max(dp[i - 1][0], -prices[i]);`

如果第i天不持有股票即`dp[i][1]`， 也可以由两个状态推出来

- 第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：`dp[i - 1][1]`
- 第i天卖出股票，所得现金就是按照今天股票价格卖出后所得现金即：`prices[i] + dp[i - 1][0]`

同样dp[i][1]取最大的，`dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);`

这样递推公式我们就分析完了

#### 买卖股票的最佳时机（变式1）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        vector<vector<int>> dp(n,vector(2,0));
        dp[0][0]=-prices[0];
        dp[0][1]=0;
        for (int i=1;i<n;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][1]-prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]+prices[i]);
        }
        return dp[n-1][1];
    }
};
```

利用二维dp定义了两种状态，0为持股，1为不持股，利用上述的递推公式就可以写出来

但是，不如贪心。

#### 买卖股票的最佳时机（变式2）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        vector<vector<int>> dp(n,vector<int>(5,0));
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        dp[0][2]=0;
        dp[0][3]=-prices[0];
        dp[0][4]=0;
        for (int i=1;i<n;i++){
            dp[i][0]=dp[i-1][0];
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i]);
            dp[i][2]=max(dp[i-1][2],dp[i-1][1]+prices[i]);
            dp[i][3]=max(dp[i-1][3],dp[i-1][2]-prices[i]);
            dp[i][4]=max(dp[i-1][4],dp[i-1][3]+prices[i]);
        }
        return dp[n-1][4];
    }
};
```

和上题做法差不多。

#### 买卖股票的最佳时机（最终变式）

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n=prices.size();
        vector<vector<int>> dp(n,vector<int>(2*k+1,0));
        dp[0][0]=0;
        for (int i=1;i<=2*k;i++){
            if (i%2==1){
                dp[0][i]=-prices[0];
            }else{
                dp[0][i]=0;
            }
        }
        for (int i=1;i<n;i++){
            for (int j=0;j<=2*k;j++){
                if (j==0){
                    dp[i][j]=dp[i-1][j];
                }
                else if (j%2==1){
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-1]-prices[i]);
                }else{
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-1]+prices[i]);
                }
            }
        }
        return dp[n-1][2*k];
    }
};
```

最公式

#### 买卖股票的最佳时机（含冷静期）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0) return 0;
        vector<vector<int>> dp(n, vector<int>(4, 0));
        dp[0][0] -= prices[0]; // 持股票
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], max(dp[i - 1][3] - prices[i], dp[i - 1][1] - prices[i]));
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3]);
            dp[i][2] = dp[i - 1][0] + prices[i];
            dp[i][3] = dp[i - 1][2];
        }
        return max(dp[n - 1][3], max(dp[n - 1][1], dp[n - 1][2]));
    }
};
```

状态机DP的套路很明显了

第一步：列举所有状态并且定义所有状态

第二步：明确状态转移的方式

第三步：确定初始状态

第四步：开始面向答案编程（ 

#### 买卖股票的最佳时机（含手续费）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n=prices.size();
        vector<vector<int>> dp(n,vector<int>(2,0));
        dp[0][0]=-prices[0];
        for (int i=1;i<n;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][1]-prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]+prices[i]-fee);
        }
        return dp[n-1][1];
    }
};
```

### 区间序列类DP

#### 最长递增子序列

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size();
        vector<int> dp(n,1);
        int maxx=1;
        for (int i=1;i<n;i++){
            for (int j=0;j<i;j++){
                if (nums[i]>nums[j]){
                    dp[i]=max(dp[i],dp[j]+1);
                }
            }
            maxx=max(dp[i],maxx);
        }
        return maxx;
    }
};
```

这题需要双层循环的原因是这个要求可以不连续

#### 最长连续递增序列

```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n=nums.size();
        vector<int> dp(n,1);
        int ans=1;
        for (int i=1;i<n;i++){
            if (nums[i]>nums[i-1]){
                dp[i]=dp[i-1]+1;
            }
            ans=max(ans,dp[i]);
        }
        return ans;
    }
};
```

这个只要单层的原因是一定要求连续

#### 最长重复子数组

```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int n=nums1.size();
        int m=nums2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        int ans=0;
        for (int i=1;i<=n;i++){
            for (int j=1;j<=m;j++){
                if (nums1[i-1]==nums2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }
                ans=max(ans,dp[i][j]);
            }
        }
        return ans;
    }
};
```

为什么不用处理不相等的情况，那是因为这是子数组，不是子序列，如果遇到不相等的情况直接过了就行

#### 最长公共子序列

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n=text1.size();
        int m=text2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        for (int i=1;i<=n;i++){
            for (int j=1;j<=m;j++){
                if (text1[i-1]==text2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[n][m];
    }
};//dp[i][j]：长度为[0, i - 1]的字符串text1与长度为[0, j - 1]的字符串text2的最长公共子序列为dp[i][j]
```

#### 不相交的钱

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int n=nums1.size();
        int m=nums2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        for (int i=1;i<=n;i++){
            for (int j=1;j<=m;j++){
                if (nums1[i-1]==nums2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[n][m];
    }
};
```

这个实际上建模一下就能发现是上面那一题的公式了

### 编辑距离类DP

> 编辑距离类 DP 是 **字符串动态规划里的一大类经典问题**，主要用于衡量两个字符串之间的差异，或者从一个字符串变成另一个字符串所需的最少操作次数。这个概念在算法竞赛、自然语言处理、文本比对中都非常常见。下面我给你整理一个 **完整概念 + 模板 + 核心思想**，让你一看就懂。
>
> ------
>
> # ✅ **1. 编辑距离的基本概念**
>
> - 给定两个字符串 `A`（长度 n）和 `B`（长度 m）
> - 允许三种操作把 `A` 变成 `B`：
>   1. **插入**一个字符
>   2. **删除**一个字符
>   3. **替换**一个字符
> - **目标**：最少操作次数 → 这就是“编辑距离”（Levenshtein distance）
>
> ------
>
> # ✅ **2. DP 状态定义**
>
> ```text
> dp[i][j] = 将 A[1..i] 变成 B[1..j] 的最少操作次数
> ```
>
> ------
>
> # ✅ **3. 状态转移方程**
>
> 1. 如果 A[i] == B[j] → 不需要操作：
>
> ```
> dp[i][j] = dp[i-1][j-1]
> ```
>
> 1. 否则可以：
>
> - 插入 B[j] → `dp[i][j-1] + 1`
> - 删除 A[i] → `dp[i-1][j] + 1`
> - 替换 A[i] → `dp[i-1][j-1] + 1`
>
> 综合：
>
> ```text
> dp[i][j] = min(
>     dp[i-1][j] + 1,    // 删除
>     dp[i][j-1] + 1,    // 插入
>     dp[i-1][j-1] + (A[i]==B[j]?0:1) // 替换或相等
> )
> ```
>
> ------
>
> # ✅ **4. 边界条件**
>
> ```cpp
> dp[0][0] = 0;       // 空字符串变空字符串 = 0
> dp[i][0] = i;       // A[1..i] → "" 需要删除 i 次
> dp[0][j] = j;       // "" → B[1..j] 需要插入 j 次
> ```
>
> ------
>
> # ✅ **5. C++ 典型模板**
>
> ```cpp
> int n = A.size(), m = B.size();
> vector<vector<int>> dp(n+1, vector<int>(m+1, 0));
> 
> // 初始化边界
> for (int i = 0; i <= n; i++) dp[i][0] = i;
> for (int j = 0; j <= m; j++) dp[0][j] = j;
> 
> // DP 计算
> for (int i = 1; i <= n; i++) {
>     for (int j = 1; j <= m; j++) {
>         if (A[i-1] == B[j-1]) dp[i][j] = dp[i-1][j-1];
>         else dp[i][j] = min({dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]+1});
>     }
> }
> 
> // 编辑距离
> int edit_distance = dp[n][m];
> ```
>
> ------
>
> # ✅ **6. 常见变种**
>
> 1. **只允许插入/删除** → 常用于 LCS（最长公共子序列）
> 2. **带权操作** → 可以给删除、插入、替换不同代价
> 3. **打印编辑步骤** → 通过回溯 dp 矩阵
> 4. **空间优化** → 只保留上一行 dp → O(min(n,m)) 空间
>
> ------
>
> # 🔹 **7. 小技巧/记忆口诀**
>
> - dp[i][j] → A 前 i 个字符变成 B 前 j 个字符
> - 三种操作：**删/增/替换** → min 三个状态
> - 相等不操作 → `dp[i][j] = dp[i-1][j-1]`
> - 初始化 `dp[0][j] = j, dp[i][0] = i`

#### 判断子序列

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int n=s.size();
        int m=t.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        for (int i=1;i<=n;i++){
            for (int j=1;j<=m;j++){
                if (s[i-1]==t[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=dp[i][j-1];//记得注意和原本那个最长子序列的差别。
                }
            }
        }
        if (dp[n][m]==n) return 1;
        return 0;
    }
};
```

感觉已经像一个模板了

#### 两个字符串的增删操作

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n=word1.size();
        int m=word2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1));
        for (int i=0;i<=n;i++){
            dp[i][0]=i;
        }
        for (int j=0;j<=m;j++){
            dp[0][j]=j;
        }
        for (int i=1;i<=n;i++){
            for (int j=1;j<=m;j++){
                if (word1[i-1]==word2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    dp[i][j]=min(dp[i-1][j]+1,dp[i][j-1]+1);
                }
            }
        }
        return dp[n][m];
    }
};//本质上是字符串匹配问题的变种
```

事实上就是用双重的循环去比较字符串，分为相同和不相同两种情况分别对进行动态规划。

#### 真 编辑距离

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n=word1.size();
        int m=word2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        for (int i=1;i<=n;i++){
            dp[i][0]=i;
        }
        for (int i=1;i<=m;i++){
            dp[0][i]=i;
        }
        for (int i=1;i<=n;i++){
            for (int j=1;i<=m;j++){
                if (word1[i-1]==word2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    dp[i][j]=min({dp[i-1][j],dp[i][j-1],dp[i-1][j-1]})+1;
                }
            }
        }
        return dp[n][m];
    }
};
```

第一步：面向答案，开始进行dp的定义

第二步：初始化

第三步：考虑相等和不相等的情况。

第四步：面向答案编程（

### 回文类

#### 回文字串

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n=s.size();
        vector<vector<bool>> dp(n,vector<bool>(n,0));
        int ans=0;
        for (int i=n-1;i>=0;i--){
            for (int j=i;j<n;j++){
                if (s[i]==s[j]){
                    if (j-i<=1){
                        ans++;
                        dp[i][j]=1;
                    }else if(dp[i+1][j-1]){
                        ans++;
                        dp[i][j]=1;
                    }
                }else{
                    dp[i][j]=0;
                }
            }
        }
        return ans;
    }
};
```

#### 最长回文子序列

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for (int i = 0; i < s.size(); i++) dp[i][i] = 1;
        for (int i = s.size() - 1; i >= 0; i--) {
            for (int j = i + 1; j < s.size(); j++) {
                if (s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size() - 1];
    }
};
```

### 总结

![](https://s3.bmp.ovh/imgs/2025/11/21/b39db20358fb987c.png)

## 单调栈

### 每日温度

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n=temperatures.size();
        stack<int> st;
        vector<int> ans(n);
        for (int i=n-1;i>=0;i--){
            while (!st.empty()&&temperatures[st.top()]<=temperatures[i]){
                st.pop();
            }
            ans[i]=st.empty()?0:st.top()-i;
            st.push(i);
        }
        return ans;
    }
};
```

### 下一个更大的元素

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();
        vector<int> ans(n, -1);
        unordered_map<int, int> nextGreater;
        stack<int> st;  // 单调递减栈（存 nums2 的值）

        // 从右往左遍历 nums2
        for (int i = m - 1; i >= 0; i--) {
            int x = nums2[i];
            // 栈顶 <= 当前元素，说明栈顶不是它的下一个更大值
            while (!st.empty() && st.top() <= x) {
                st.pop();
            }
            // 栈顶即为右边第一个比 x 大的元素
            nextGreater[x] = st.empty() ? -1 : st.top();
            st.push(x);
        }

        // 查询 nums1
        for (int i = 0; i < n; i++) {
            ans[i] = nextGreater[nums1[i]];
        }

        return ans;
    }
};

```

这个模板依旧没差，这个讲的跟清楚，单调栈其实就是记录已经遍历过的元素。

### 下一个更大的元素（变式一）

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        stack<int> st;

        // 遍历两倍长度，实现循环数组效果
        for (int i = 2 * n - 1; i >= 0; i--) {
            int idx = i % n;  // 当前真实下标（循环取模）

            // 弹出所有比当前值小或相等的元素
            while (!st.empty() && nums[st.top()] <= nums[idx]) {
                st.pop();
            }

            // 栈顶即为下一个更大元素
            if (!st.empty()) {
                ans[idx] = nums[st.top()];
            }

            // 当前元素入栈
            st.push(idx);
        }

        return ans;
    }
};
```

### 接雨水

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        stack<int> st;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            while (!st.empty() && height[i] > height[st.top()]) {
                int mid = st.top(); 
                st.pop();
                if (st.empty()) break; // 没左挡板
                int left = st.top();
                int width = i - left - 1;
                int h = min(height[left], height[i]) - height[mid];
                ans += width * h;
            }
            st.push(i);
        }
        return ans;
    }
};
```

### 柱形图中最大的矩形

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& h) {
        int n=h.size();
        vector<int> arr1(n+2);//左边
        stack<int> st1;
        for (int i=0;i<n;i++){
            while (!st1.empty()&&h[st1.top()]>=h[i]){
                st1.pop();
            }
            arr1[i]=st1.empty()?-1:st1.top();
            st1.push(i);
        }
        vector<int> arr2(n+2);//右边
        stack<int> st2;
        for (int i=n-1;i>=0;i--){
            while (!st2.empty()&&h[st2.top()]>=h[i]){
                st2.pop();
            }
            arr2[i]=st2.empty()?n:st2.top();
            st2.push(i);
        }
        int ans=0;
        for (int i=0;i<n;i++){
            ans=max(ans,(arr2[i]-arr1[i]-1)*h[i]);
        }
        return ans;
    }
};

//版本二
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> st;
        heights.insert(heights.begin(), 0); // 数组头部加入元素0
        heights.push_back(0); // 数组尾部加入元素0
        st.push(0);
        int result = 0;
        for (int i = 1; i < heights.size(); i++) {
            while (heights[i] < heights[st.top()]) {
                int mid = st.top();
                st.pop();
                int w = i - st.top() - 1;
                int h = heights[mid];
                result = max(result, w * h);
            }
            st.push(i);
        }
        return result;
    }
};
```

版本二和接雨水很类似，感觉可以背一下板子。

## 图论

由于主播已经学过DFS和BFS了，因此不多赘述这个基本概念。

### 可达路径

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
vector<vector<int>> ans;
vector<int> path;
void dfs(int start,vector<vector<int>> &mp){
    if (start==n){
        ans.push_back(path);
        return;
    }
    for (int i=1;i<=n;i++){
        if (mp[start][i]==1){
            path.push_back(i);
            dfs(i,mp);
            path.pop_back();
        }
    }
}
int main(){
    cin>>n>>m;
    vector<vector<int>> mp(n+1,vector<int>(n+1));
    for (int i=0;i<m;i++){
        int x,y;
        cin>>x>>y;
        mp[x][y]=1;
    }
    path.push_back(1);
    dfs(1,mp);
    if (ans.size()==0){
        cout<<"-1"<<endl;
        return 0;
    }
    for (int i=0;i<ans.size();i++){
        for (int j=0;j<ans[i].size();j++){
            if (j==ans[i].size()-1){
                cout<<ans[i][j]<<endl;
            }else{
                cout<<ans[i][j]<<" ";
            }
        }
    }
    return 0;
}
```

很典型的DFS模板题

### 岛屿类

#### 岛屿统计

```cpp
//DFS
#include <bits/stdc++.h>
using namespace std;
int xx[4]={0,0,-1,1};
int yy[4]={1,-1,0,0};
int mp[55][55];
int n,m;
int vis[55][55];
void dfs(int x,int y){
    vis[x][y]=1;//这边要开局就标记
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dx>=n||dy<0||dy>=m||vis[dx][dy]||mp[dx][dy]==0){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
int main(){
    cin>>n>>m;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    int ans=0;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (mp[i][j]==1&&!vis[i][j]){
                ans++;
                dfs(i,j);
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}

//BFS
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void bfs(const vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> que;
    que.push({x, y});
    visited[x][y] = true; // 只要加入队列，立刻标记
    while(!que.empty()) {
        pair<int ,int> cur = que.front(); que.pop();
        int curx = cur.first;
        int cury = cur.second;
        for (int i = 0; i < 4; i++) {
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
                que.push({nextx, nexty});
                visited[nextx][nexty] = true; // 只要加入队列立刻标记
            }
        }
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n, vector<bool>(m, false));

    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == 1) {
                result++; // 遇到没访问过的陆地，+1
                bfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
            }
        }
    }


    cout << result << endl;
}
```

很模板很公式

#### 最大岛屿

```cpp
#include <bits/stdc++.h>
using namespace std;
int xx[4]={0,0,-1,1};
int yy[4]={1,-1,0,0};
int mp[55][55];
int n,m;
int vis[55][55];
int cnt;
void dfs(int x,int y){
    vis[x][y]=1;
    cnt++;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dx>=n||dy<0||dy>=m||vis[dx][dy]||mp[dx][dy]==0){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
int main(){
    cin>>n>>m;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    int ans=0;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (mp[i][j]==1&&!vis[i][j]){
                cnt=0;
                dfs(i,j);
                ans=max(ans,cnt);
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

dfs的作用是打标记，一旦遇到一个合理的节点，立刻启动下一层递归，因此我们可以在递归开始就收集答案并且计数。

#### 岛屿面积

```cpp
#include <bits/stdc++.h>
using namespace std;
int xx[4]={0,0,-1,1};
int yy[4]={1,-1,0,0};
int mp[55][55];
int n,m;
int vis[55][55];
int cnt;
int sum;
void dfs(int x,int y){
    vis[x][y]=1;
    sum++;
    if  (x==0||x==n-1||y==0||y==m-1){
        cnt=0;
    }
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dx>=n||dy<0||dy>=m||vis[dx][dy]||mp[dx][dy]==0){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
int main(){
    cin>>n>>m;
    int ans=0;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (mp[i][j]==1&&!vis[i][j]){
                cnt=1;
                sum=0;
                dfs(i,j);
                if (cnt==1){
                    ans+=sum;
                }
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

#### 沉没孤岛

（注意，这题不是海岸线上涨）

```cpp
#include <bits/stdc++.h>
using namespace std;
int xx[4]={0,0,-1,1};
int yy[4]={1,-1,0,0};
int mp[55][55];
int n,m;
int vis[55][55];
void dfs(int x,int y){
    vis[x][y]=1;
    mp[x][y]=2;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dx>=n||dy<0||dy>=m||vis[dx][dy]||!mp[dx][dy]){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
int main(){
    cin>>n>>m;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    for (int i=0;i<n;i++){
        if (mp[i][0]==1) dfs(i,0);
        if (mp[i][m-1]==1) dfs(i,m-1);
    }
    for (int i=0;i<m;i++){
        if (mp[0][i]==1) dfs(0,i);
        if (mp[n-1][i]==1) dfs(n-1,i);
    }
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (mp[i][j]==2){
                mp[i][j]=1;
            }else if(mp[i][j]==1){
                mp[i][j]=0;
            }
        }
    }
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cout<<mp[i][j]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

思路其实挺逆向的。

#### 篮球杯淹没岛屿

```cpp
#include <bits/stdc++.h>
using namespace std;
int xx[4]={0,0,-1,1};
int yy[4]={1,-1,0,0};
char mp[1050][1050];
int vis[1050][1050];
int n;
class node{
    public:
    int x,y;
};
vector<node> b;
bool soak(int x,int y){
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=n){
            continue;
        }else{
            if (mp[dx][dy]=='.'){
                return false;
            }
        }
    }
    return true;
}
void dfs(int x,int y){
    vis[x][y]=1;
    if (!soak(x,y)){
        b.push_back((node){x,y});
    }
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=n||vis[dx][dy]||mp[dx][dy]=='.'){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
void dfs1(int x,int y){
    vis[x][y]=1;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=n||vis[dx][dy]||mp[dx][dy]=='.'){
            continue;
        }else{
            dfs1(dx,dy);
        }
    }
}
int main(){
    cin>>n;
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++){
            cin>>mp[i][j];
        }
    }
    int sum1=0,sum2=0;
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++){
            if (mp[i][j]=='#'&&!vis[i][j]){
                sum1++;
                dfs1(i,j);
            }
        }
    }
    memset(vis,0,sizeof(vis));
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++){
            if (mp[i][j]=='#'&&!vis[i][j]){
                dfs(i,j);
            }
        }
    }
    for (auto it:b){
        mp[it.x][it.y]='-';
    }//注意这里别直接修改成海，不然的话就会被认为是分裂出了新的岛屿
    memset(vis,0,sizeof(vis));
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++){
            if (mp[i][j]=='#'&&!vis[i][j]){
                sum2++;
                dfs1(i,j);
            }
        }
    }
    cout<<sum1-sum2<<endl;
    return 0;
}
```

***你要记住，这种题目如果内存不是限制的很死，不要在原图上修改和打标记，真的很容易错。***

***在原图上的标记会有非常非常非常多麻烦的连锁反应，水平不够千万千万千万不要这么干，是特别坏的习惯。***

#### 高山流水

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
int mp[110][110];
int xx[4]={0,0,1,-1},yy[4]={1,-1,0,0};
class node{
    public:
    int x,y;
};
void dfs(int x,int y,vector<vector<int>> &vis){
    vis[x][y]=1;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=m||vis[dx][dy]||mp[x][y]>mp[dx][dy]){
            continue;
        }else{
            dfs(dx,dy,vis);
        }
    }
}
int main(){
    cin>>n>>m;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    vector<vector<int>> vis1(n,vector<int>(m,0));
    vector<vector<int>> vis2(n,vector<int>(m,0));
    for (int i=0;i<n;i++){
        dfs(i,0,vis1);
        dfs(i,m-1,vis2);
    }
    for (int i=0;i<m;i++){
        dfs(0,i,vis1);
        dfs(n-1,i,vis2);
    }
    vector<node> ans;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (vis1[i][j]==1&&vis2[i][j]==1){
                ans.push_back((node){i,j});
            }
        }
    }
    for (auto it:ans){
        cout<<it.x<<" "<<it.y<<endl;
    }
    return 0;
}//Can run is ok
```

#### 引水入城（2024机考压轴题）

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,m;
int mp[1010][1010];
int vis[1010][1010];
int xx[4]={0,0,1,-1},yy[4]={1,-1,0,0};
int l[1010][1010],r[1010][1010];//这个用来记录可以覆盖到的左右区间端点，dp用的
void dfs(int x,int y){
    vis[x][y]=1;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=m||mp[x][y]<=mp[dx][dy]){
            continue;
        }
        if (!vis[dx][dy]){
            dfs(dx,dy);
        }
        l[x][y]=min(l[x][y],l[dx][dy]);
        r[x][y]=max(r[x][y],r[dx][dy]);
    }
}
int main(){
    cin>>n>>m;
    memset(l,200000,sizeof(l));
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    for (int i=0;i<m;i++){
        l[n-1][i]=i;
        r[n-1][i]=i;
    }
    for (int i=0;i<m;i++){
        if (!vis[0][i]){
            dfs(0,i);
        }
    }
    bool flag=1;
    int ans=0;
    for (int i=0;i<m;i++){
        if (vis[n-1][i]==0){
            flag=0;
            ans++;
        }
    }
    if (!flag){
        cout<<0<<endl;
        cout<<ans<<endl;
        return 0;
    }
    ans=0;
    int left=0,right=r[0][0];
    while (left<=m-1){
        for (int i=0;i<=m-1;i++){
            if (l[0][i]<=left){
                right=max(right,r[0][i]);
            }
        }
        left=right+1;
        ans++;
    }
    cout<<1<<endl;
    cout<<ans<<endl;
    return 0;
}
```

省选，我曾经都不敢直面的题目。

这题的思路并不难想，弯弯绕绕的不多，大方向是图论，在一些细节处理上存在有小方向的问题。我等下细致的讲一下吧。

第一次见到这道题的时候，说实话我是望而却步的。它看起来既涉及图论又有动态规划和贪心的影子，感觉思路很复杂，不知道从哪下手。

后来真正静下心去分析，才发现它的大方向确实是图论问题，但核心思路并不难，只是综合性很强：

- 用 DFS 建立从高处流向低处的关系，
- 结合动态规划记录每个点能覆盖到的区间，
- 最后再用贪心完成最少区间覆盖。

如果真的要把这一题A出来，真的要对这些前置知识有很深刻的理解（所以还不赶紧接着刷算法）

DFS可以处理水流问题我们都是知道的，我们可以尝试去枚举每一个最上面的点，然后利用动态规划给出这个点能到的最左和最右区间（我个人感觉这个不动态规划也是可以的，理论可行后面讲），最后就是一个很简单的贪心区间统计问题了。

我现在讲讲不用DP的思路啊，纯我自己想的，洛谷好像没这种思路。因为不用DP我们每次探索都是一次独立的过程，这个时候我们就需要一个局部的vis数组和一个全局的vis数组，全局的vis数组用来判断最底层是不是都可以被覆盖到，然后我们怎么更新最左侧和最右侧，只要我们在到最后一排的时候更新就行了，多说无益，直接上代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
int mp[1010][1010];
int xx[4] = {0, 0, 1, -1}, yy[4] = {1, -1, 0, 0};
int l[1010], r[1010];
bool global_vis[1010][1010]; // 全局vis数组，用于检查最后一行是否全部被灌溉

void dfs(int x, int y, int start, vector<vector<bool>>& local_vis) {
    local_vis[x][y] = true;
    global_vis[x][y] = true; // 同时更新全局vis数组
    
    if (x == n-1) {
        l[start] = min(y, l[start]);
        r[start] = max(y, r[start]);
    }
    
    for (int i = 0; i < 4; i++) {
        int dx = x + xx[i];
        int dy = y + yy[i];
        if (dx < 0 || dy < 0 || dx >= n || dy >= m || mp[x][y] <= mp[dx][dy]) {
            continue;
        }
        if (!local_vis[dx][dy]) {
            dfs(dx, dy, start, local_vis);
        }
    }
}

int main() {
    cin >> n >> m;
    memset(l, 0x3f, sizeof(l)); // 使用标准的大数初始化
    memset(r, -1, sizeof(r));   // 初始化r数组
    memset(global_vis, 0, sizeof(global_vis)); // 初始化全局vis数组
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> mp[i][j];
        }
    }
    
    // 为每个起点创建独立的局部vis数组
    for (int i = 0; i < m; i++) {
        vector<vector<bool>> local_vis(n, vector<bool>(m, false));
        if (!global_vis[0][i]) { // 如果起点已经被全局标记，可以跳过
            dfs(0, i, i, local_vis);
        }
    }
    
    // 检查底部是否全部覆盖
    bool flag = true;
    int uncovered = 0;
    for (int i = 0; i < m; i++) {
        if (!global_vis[n-1][i]) {
            flag = false;
            uncovered++;
        }
    }
    
    if (!flag) {
        cout << 0 << endl;
        cout << uncovered << endl;
        return 0;
    }
    
    // 贪心覆盖区间
    int ans = 0;
    int left = 0;
    while (left < m) {
        int right = -1;
        for (int i = 0; i < m; i++) {
            if (l[i] <= left) {
                right = max(right, r[i]);
            }
        }
        left = right + 1;
        ans++;
    }
    cout << 1 << endl;
    cout << ans << endl;
    return 0;
}//我自己写的思路，让AI补了注释和改了改变量名
```

#### 建造最大工岛

```cpp
#include <bits/stdc++.h>
using namespace std;

int mp[55][55];
int us[55][55];
int n,m;
int vis[55][55];
int xx[4]={0,0,-1,1},yy[4]={1,-1,0,0};
int cnt;
void dfs(int x,int y,int color){
    vis[x][y]=1;
    us[x][y]=color;
    cnt++;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>=n||dy>=m||vis[dx][dy]||mp[dx][dy]==0){
            continue;
        }else{
            dfs(dx,dy,color);
        }
    }
}
int main(){
    cin>>n>>m;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            cin>>mp[i][j];
        }
    }
    unordered_map<int,int> h;
    int idx=2;
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (!vis[i][j]&&mp[i][j]==1){
                cnt=0;
                dfs(i,j,idx);
                h[idx]=cnt;
                idx++;
            }
        }
    }
    int ans=1;
    for (int i=2;i<idx;i++){
        ans=max(ans,h[i]);
    }
    for (int i=0;i<n;i++){
        for (int j=0;j<m;j++){
            if (mp[i][j]==0){
                int sum=1;
                set<int> s;
                for (int k=0;k<4;k++){
                    int dx=i+xx[k];
                    int dy=j+yy[k];
                    if (us[dx][dy]!=0&&s.find(us[dx][dy])==s.end()){
                        sum+=h[us[dx][dy]];
                        s.insert(us[dx][dy]);
                    }
                }
                ans=max(ans,sum);
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

#### 海岸线统计

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
int mp[55][55];
int vis[55][55];
int xx[4]={0,0,-1,1},yy[4]={1,-1,0,0};
class node{
    public:
    int x,y;
};
vector<node> b;
int soak(int x,int y){
    int sum=0;
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>n+1||dy>m+1){
            continue;
        }else{
            if (mp[dx][dy]==0){
                sum++;
            }
        }
    }
    return sum;
}
bool sea(int x,int y){
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<0||dy<0||dx>n+1||dy>m+1){
            continue;
        }else{
            if (mp[dx][dy]==0){
                return 1;
            }
        }
    }
    return 0;
}
void dfs(int x,int y){
    vis[x][y]=1;
    if (sea(x,y)){
        b.push_back((node){x,y});
    }
    for (int i=0;i<4;i++){
        int dx=x+xx[i];
        int dy=y+yy[i];
        if (dx<1||dy<1||dx>n||dy>m||vis[dx][dy]||mp[dx][dy]==0){
            continue;
        }else{
            dfs(dx,dy);
        }
    }
}
int main(){
    cin>>n>>m;
    int us[55][55];
    for (int i=1;i<=n;i++){
        for (int j=1;j<=m;j++){
            cin>>mp[i][j];
            us[i][j]=mp[i][j];
        }
    }
    for (int i=1;i<=n;i++){
        for (int j=1;j<=m;j++){
            if (mp[i][j]==1&&!vis[i][j]){
                dfs(i,j);
            }
        }
    }
    for (auto it:b){
        us[it.x][it.y]=2;
    }
    // for (int i=0;i<n;i++){
    //     for (int j=0;j<m;j++){
    //         cout<<us[i][j];
    //     }
    //     cout<<endl;
    // }
    int ans=0;
    for (int i=1;i<=n;i++){
        for (int j=1;j<=m;j++){
            if (us[i][j]==2){
                ans+=soak(i,j);
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

处理完图把图输出出来先看一眼是一个好习惯喵

这时候有人就要问了，主播主播，如果有内海怎么办喵，有内海你就从图边缘开始DFS，遇到陆地停止，然后把外海染色就行了。

### 字符串迁移（最短路问题）

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
string tbe,tend;
class node{
    public:
    string a;
    int step;
};
int main(){
    cin>>n;
    cin>>tbe>>tend;
    unordered_set<string> lis;
    unordered_set<string> vis;
    for (int i=0;i<n;i++){
        string b;
        cin>>b;
        lis.insert(b);
    }
    queue<node> bfs;
    bfs.push((node){tbe,1});
    while (!bfs.empty()){
        string word=bfs.front().a;
        int cnt=bfs.front().step;
        bfs.pop();
        for (int i=0;i<word.size();i++){
            string neww=word;
            for (int j=0;j<26;j++){
                neww[i]='a'+j;
                if (neww==tend){
                    cout<<cnt+1<<endl;
                    return 0;
                }
                if (lis.find(neww)!=lis.end()&&vis.find(neww)==vis.end()){
                    vis.insert(neww);
                    bfs.push((node){neww,cnt+1});
                }
            }
        }
    }
    cout<<"-1"<<endl;
    return 0;
}
```

DFS我自己试过了，解决的思路和这个差不多，但是DFS由于天生就是那种一条路走到黑的，不适合求这种最短路问题，最后也是美美超时。

DFS和BFS的区别在于，一个是方向导向的，一个是节点导向的，DFS会向一个方向一条路走到黑，而BFS更像是广撒网，把这个节点连接的全部走完了再说。

你可以理解为，DFS过程就是一条龙一直往一个方向走到黑为止，而BFS的过程就是水面的涟漪，是扩散式的，求最短路为什么BFS更好用，就是因为你可以理解为BFS可以直接往外面扩散，第一次扩散到答案的地方就是最短路了。

### 有向图的完全联通

```cpp
//DFS版
#include <bits/stdc++.h>
using namespace std;

int n,k;
int vis[110];
void dfs(int start,vector<vector<int>> &mp){
    vis[start]=1;
    vector<int> p=mp[start];
    int n=p.size();
    for (int i=0;i<n;i++){
        if (vis[p[i]]==1){
            continue;
        }else{
            dfs(p[i],mp);
        }
    }
}
int main(){
    cin>>n>>k;
    vector<vector<int>> mp(n);
    for (int i=0;i<k;i++){
        int a,b;
        cin>>a>>b;
        a--;
        b--;
        mp[a].push_back(b);
    }
    dfs(0,mp);
    bool check=1;
    for (int i=0;i<n;i++){
        if (vis[i]==0){
            //cout<<vis[i]<<" ";
            check=0;
        }
    }
    //cout<<endl;
    if (!check){
        cout<<"-1"<<endl;
    }else{
        cout<<"1"<<endl;
    }
    return 0;
}
```

```cpp
//BFS版
#include <bits/stdc++.h>
using namespace std;

int n,k;
int vis[110];

int main(){
    cin>>n>>k;
    vector<vector<int>> mp(n);
    for (int i=0;i<k;i++){
        int a,b;
        cin>>a>>b;
        a--;
        b--;
        mp[a].push_back(b);
    }
    queue<int> q;
    q.push(0);
    while (!q.empty()){
        int node=q.front();
        q.pop();
        vis[node]=1;
        vector<int> p=mp[node];
        for (int i=0;i<p.size();i++){
            if (!vis[p[i]]){
                q.push(p[i]);
            }
        }
    }
    bool check=1;
    for (int i=0;i<n;i++){
        if (!vis[i]){
            check=0;
        }
    }
    if (check){
        cout<<1<<endl;
    }else{
        cout<<"-1"<<endl;
    }
    return 0;
}
```

其实挺简单的喵。

### 并查集理论基础

#### 模板

```cpp
int n = 1005; // n根据题目中节点数量而定，一般比节点数量大一点就好
vector<int> father = vector<int> (n, 0); // C++里的一种数组结构

// 并查集初始化
void init() {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}
// 并查集里寻根的过程
int find(int u) {
    return u == father[u] ? u : father[u] = find(father[u]); // 路径压缩
}

// 判断 u 和 v是否找到同一个根
bool isSame(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

// 将v->u 这条边加入并查集
void join(int u, int v) {
    u = find(u); // 寻找u的根
    v = find(v); // 寻找v的根
    if (u == v) return ; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    father[v] = u;
}
```

#### 寻找存在的路线

这一题很明显有两种解法，一种是传统DFS/BFS，另外一种是并查集写法，这里不写传统法。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
int father[110];

int findfather(int u){
    if (u==father[u]) return u;
    else return father[u]=findfather(father[u]);
}
void join(int a,int b){
    a=findfather(a);
    b=findfather(b);
    if (a==b) return;
    else{
        father[b]=a;
    }
}
bool isin(int a,int b){
    a=findfather(a);
    b=findfather(b);
    if (a==b) return 1;
    else return 0;
}
int main(){
    cin>>n>>m;
    for (int i=1;i<=n;i++){
        father[i]=i;
    }
    for (int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        join(a,b);
    }
    int s,d;
    cin>>s>>d;
    if (isin(s,d)){
        cout<<1<<endl;
    }else{
        cout<<0<<endl;
    }
    return 0;
}
```

#### 多余连接

```cpp
#include <bits/stdc++.h>
using namespace std;
int father[1010];
int find(int u){
    if (u==father[u]){
        return u;
    }else{
        return father[u]=find(father[u]);
    }
}
bool issame(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return 1;
    }else{
        return 0;
    }
}
void join(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return;
    }else{
        father[b]=a;
    }
}
int n,s,t;
int main(){
    cin>>n;
    for (int i=1;i<=n;i++){
        father[i]=i;
    }
    for (int i=0;i<n;i++){
        cin>>s>>t;
        if (!issame(s,t)){
            join(s,t);
        }else{
            cout<<s<<" "<<t<<endl;
            return 0;
        }
    }
}
```

思路：不在同一个集合的加入，已经在同一个集合的直接输出。

#### 多余连接（有向图）

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,s,t;
int father[1010];
int in[1010];
void init(){
    for (int i=1;i<=n;i++){
        father[i]=i;
    }
}
int find(int u){
    if (u==father[u]){
        return u;
    }else{
        return father[u]=find(father[u]);
    }
}
bool issame(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return 1;
    }else{
        return 0;
    }
}
void join(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return;
    }else{
        father[b]=a;
    }
}
bool removetree(vector<vector<int>> &edge,int a){
    init();
    for (int i=0;i<n;i++){
        if (i==a){
            continue;
        }else{
            if (issame(edge[i][0],edge[i][1])){
                return 0;
            }
            join(edge[i][0],edge[i][1]);
        }
    }
    return 1;
}
void removedge(vector<vector<int>> &edge){
    init();
    for (int i=0;i<n;i++){
        if (issame(edge[i][0],edge[i][1])){
            cout<<edge[i][0]<<" "<<edge[i][1];
            return;
        }else{
            join(edge[i][0],edge[i][1]);
        }
    }
}
int main(){
    cin>>n;
    vector<vector<int>> edge;
    for (int i=0;i<n;i++){
        cin>>s>>t;
        in[t]++;
        edge.push_back({s,t});
    }
    vector<int> vec;
    for (int i=n-1;i>=0;i--){
        if (in[edge[i][1]]==2){
            vec.push_back(i);
        }
    }
    if (vec.size()>0){
        for (int i=0;i<vec.size();i++){
            if (removetree(edge,vec[i])){
                cout<<edge[vec[i]][0]<<" "<<edge[vec[i]][1];
                return 0;
            }
        }
    }
    removedge(edge);
    return 0;
}
```

思路看网站吧，我说不清楚。

顺便补充一下如何找到有向图和无向图里的环

> 好的，这里是两个 C++ 演示，分别用于无向图和有向图。
>
> 两个例子都使用邻接表来存储图，并都依赖于 **深度优先搜索 (DFS)**。核心区别在于它们如何检测“回边”（back edge）。
>
> ------
>
> 
>
> ## 🌳 无向图 (Undirected Graph)
>
> 
>
> 在无向图中，我们使用一个 `visited` 数组和一个 `parent` 变量。当我们访问一个节点 `u` 时，我们遍历它的邻居 `v`：
>
> - 如果 `v` 是 `u` 的父节点，我们忽略它。
> - 如果 `v` **不是父节点**，但**已经被访问过** (`visited[v] == true`)，我们就找到了一个环。
>
> 我们使用一个额外的 `path` 向量来追踪当前的 DFS 路径，以便在找到环时能立即输出成环节点。
>
> C++
>
> ```cpp
> #include <iostream>
> #include <vector>
> #include <algorithm> // 用于 std::find
> 
> using namespace std;
> 
> // 邻接表
> vector<vector<int>> adj;
> // 访问标记
> vector<bool> visited;
> 
> /**
>  * @brief 打印环路
>  * @param path 当前的DFS路径
>  * @param startNode 环的起始节点 (即那个 "已被访问" 的节点)
>  */
> void printCycle(const vector<int>& path, int startNode) {
>     cout << "找到环: ";
>     // 从 path 中找到 startNode 第一次出现的位置
>     auto it = find(path.begin(), path.end(), startNode);
>     
>     // 打印从 startNode 到 path 末尾的所有节点
>     for (auto i = it; i != path.end(); ++i) {
>         cout << *i << " -> ";
>     }
>     // 最后再打印一次 startNode，形成闭环
>     cout << startNode << endl;
> }
> 
> /**
>  * @brief 无向图的DFS
>  * @param u 当前节点
>  * @param parent_node u 的父节点
>  * @param path 到达 u 的路径
>  * @return bool 是否找到了环
>  */
> bool dfs_undirected(int u, int parent_node, vector<int>& path) {
>     visited[u] = true;
>     path.push_back(u); // 将当前节点加入路径
> 
>     for (int v : adj[u]) {
>         // 忽略父节点
>         if (v == parent_node) {
>             continue;
>         }
> 
>         // 情况一：v 已被访问，且不是父节点
>         if (visited[v]) {
>             // 找到了环！v 是环的起点，u 是环的终点
>             path.push_back(v); // 把环的起点也加进来，方便打印
>             printCycle(path, v);
>             path.pop_back(); // 恢复 path 状态（非必须，因为马上要返回 true）
>             return true; // 找到环，立即返回
>         }
> 
>         // 情况二：v 未被访问
>         if (dfs_undirected(v, u, path)) {
>             return true; // 如果子调用找到了环，也立即返回
>         }
>     }
> 
>     path.pop_back(); // 回溯：将当前节点移出路径
>     return false;
> }
> 
> int main() {
>     int n = 6; // 6个节点 (0 到 5)
>     adj.resize(n);
>     visited.resize(n, false);
> 
>     // 构造一个带环的图
>     //     0
>     //    / \
>     //   1 - 2
>     //   |   |
>     //   3 - 4
>     //   |
>     //   5
>     adj[0].push_back(1);
>     adj[0].push_back(2);
>     
>     adj[1].push_back(0);
>     adj[1].push_back(2);
>     adj[1].push_back(3);
> 
>     adj[2].push_back(0);
>     adj[2].push_back(1);
>     adj[2].push_back(4);
> 
>     adj[3].push_back(1);
>     adj[3].push_back(4);
>     adj[3].push_back(5);
> 
>     adj[4].push_back(2);
>     adj[4].push_back(3);
> 
>     adj[5].push_back(3);
> 
>     cout << "--- 正在查找无向图中的环 ---" << endl;
>     vector<int> path;
>     // 遍历所有节点以处理非连通图
>     for (int i = 0; i < n; ++i) {
>         if (!visited[i]) {
>             // 我们只需要找到第一个环
>             if (dfs_undirected(i, -1, path)) {
>                 // 示例可能找到多个环，这里只演示找到的第一个
>                 // 如 0 -> 1 -> 2 -> 0
>                 // 或 1 -> 3 -> 4 -> 2 -> 1
>             }
>         }
>     }
> 
>     return 0;
> }
> ```
>
> ------
>
> 
>
> ## ✈️ 有向图 (Directed Graph)
>
> 
>
> 在有向图中，我们不能只用 `visited` 数组。我们需要一个数组来追踪**三种状态**：
>
> - `0`: 未访问 (unvisited)
> - `1`: **访问中** (visiting / on current recursion stack)
> - `2`: 已访问 (visited / processing finished)
>
> 当我们从 `u` 访问 `v` 时：
>
> - 如果 `v` 的状态是 `1` (访问中)，说明 `v` 是 `u` 的祖先，我们找到了一个环。
> - 如果 `v` 的状态是 `2` (已访问)，说明 `v` 属于另一个已处理完的分支，这不是环。
>
> C++
>
> ```cpp
> #include <iostream>
> #include <vector>
> #include <algorithm> // 用于 std::find
> 
> using namespace std;
> 
> // 邻接表
> vector<vector<int>> adj_directed;
> // 访问状态: 0=未访问, 1=访问中, 2=已访问
> vector<int> visited_directed;
> 
> /**
>  * @brief 打印环路 (与无向图版本相同)
>  */
> void printCycle_directed(const vector<int>& path, int startNode) {
>     cout << "找到环: ";
>     auto it = find(path.begin(), path.end(), startNode);
>     for (auto i = it; i != path.end(); ++i) {
>         cout << *i << " -> ";
>     }
>     cout << startNode << endl;
> }
> 
> /**
>  * @brief 有向图的DFS
>  * @param u 当前节点
>  * @param path 到达 u 的路径
>  * @return bool 是否找到了环
>  */
> bool dfs_directed(int u, vector<int>& path) {
>     visited_directed[u] = 1; // 标记为 "访问中"
>     path.push_back(u);       // 加入路径
> 
>     for (int v : adj_directed[u]) {
>         // 情况一：v 正在访问中 (状态为 1)
>         if (visited_directed[v] == 1) {
>             // 找到了环！v 是环的起点
>             path.push_back(v);
>             printCycle_directed(path, v);
>             path.pop_back();
>             return true;
>         }
> 
>         // 情况二：v 未访问 (状态为 0)
>         if (visited_directed[v] == 0) {
>             if (dfs_directed(v, path)) {
>                 return true; // 子调用找到了环
>             }
>         }
>         
>         // 情况三：v 已访问 (状态为 2)
>         // 这是安全的，v 属于已处理完的分支，不是环。
>     }
> 
>     visited_directed[u] = 2; // 标记为 "已访问完毕"
>     path.pop_back();         // 回溯：移出路径
>     return false;
> }
> 
> 
> int main() {
>     int n = 5; // 5个节点 (0 到 4)
>     adj_directed.resize(n);
>     visited_directed.resize(n, 0); // 初始状态 0
> 
>     // 构造一个带环的图
>     // 0 -> 1 -> 2
>     //      ^    |
>     //      |    v
>     //      4 <- 3
>     adj_directed[0].push_back(1);
>     adj_directed[1].push_back(2);
>     adj_directed[2].push_back(3);
>     adj_directed[3].push_back(4);
>     adj_directed[4].push_back(1); // 这条边 (4 -> 1) 构成了环 1-2-3-4-1
> 
>     cout << "--- 正在查找有向图中的环 ---" << endl;
>     vector<int> path;
>     for (int i = 0; i < n; ++i) {
>         if (visited_directed[i] == 0) {
>             if (dfs_directed(i, path)) {
>                 // 示例中会找到 1 -> 2 -> 3 -> 4 -> 1
>             }
>         }
>     }
>     
>     return 0;
> }
> ```

### 最短路算法

#### prim算法（存点）

```cpp
#include <bits/stdc++.h>
using namespace std;

int v,e,v1,v2,val;
int main(){
    cin>>v>>e;
    vector<vector<int>> grid(v+1,vector<int>(v+1,10010));
    for (int i=0;i<e;i++){
        cin>>v1>>v2>>val;
        grid[v1][v2]=val;
        grid[v2][v1]=val;
    }
    vector<int> mindist(v+1,10010);
    vector<bool> in(v+1,false);
    for (int i=1;i<v;i++){
        int cur=-1;
        int minn=10011;
        for (int j=1;j<=v;j++){
            if (!in[j]&&mindist[j]<minn){
                minn=mindist[j];
                cur=j;
            }
        }
        in[cur]=true;
        for (int j=1;j<=v;j++){
            if (!in[j]&&grid[cur][j]<mindist[j]){
                mindist[j]=grid[cur][j];
            }
        }
    }
    int ans=0;
    for (int i=2;i<=v;i++){
        ans+=mindist[i];
    }
    cout<<ans<<endl;
    return 0;
}
```

#### Kruskal算法（存边）

```cpp
#include <bits/stdc++.h>
using namespace std;
int v,e,v1,v2,val;
int father[1010];
void init(){
    for (int i=1;i<=v;i++){
        father[i]=i;
    }
    return;
}
int find(int a){
    if (a==father[a]){
        return a;
    }else{
        return father[a]=find(father[a]);
    }
}
bool issame(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return 1;
    }else{
        return 0;
    }
}
void join(int a,int b){
    a=find(a);
    b=find(b);
    if (a==b){
        return;
    }else{
        father[b]=a;
    }
}
class node{
    public:
    int fr,to,d;
};
bool cmp(node a,node b){
    return a.d<b.d;
}
int main(){
    cin>>v>>e;
    init();
    vector<node> mp;
    for (int i=0;i<e;i++){
        cin>>v1>>v2>>val;
        mp.push_back((node){v1,v2,val});
    }
    sort(mp.begin(),mp.end(),cmp);
    int ans=0;
    for (auto it:mp){
        if (!issame(it.fr,it.to)){
            ans+=it.d;
            join(it.fr,it.to);
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

#### 拓扑排序

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
int in[100010];
int main(){
    cin>>n>>m;
    vector<vector<int>> g(n+1);
    for (int i=0;i<m;i++){
        int u,v;
        cin>>u>>v;
        g[u].push_back(v);
        in[v]++;
    }
    queue<int> q;
    for (int i=0;i<n;i++){
        if (in[i]==0){
            q.push(i);
        }
    }
    vector<int> res;
    while (!q.empty()){
        int p=q.front();
        q.pop();
        res.push_back(p);
        for (int i=0;i<g[p].size();i++){
            in[g[p][i]]--;
            if (in[g[p][i]]==0){
                q.push(g[p][i]);
            }
        }
    }
    if (res.size()==n){
        for (int i=0;i<res.size();i++){
            if (i==res.size()-1){
                cout<<res[i];
            }else{
                cout<<res[i]<<" ";
            }
        }
    }else{
        cout<<"-1"<<endl;
    }
    return 0;
}
```

> 判断图中有没有环，比找到环并输出它要**更简单**。你不需要 `path` 栈来追踪路径，只需要在DFS中返回一个布尔值（`true` 或 `false`）即可。
>
> 这里介绍两种图最主流的判断方法。
>
> ------
>
> 
>
> ## 🌳 无向图 (Undirected Graph)
>
> 
>
> 对于无向图，我们有两种常用方法：
>
> 
>
> ### 方法一：DFS (深度优先搜索)
>
> 
>
> 这是最直观的方法，逻辑与上一题类似，但更精简：
>
> 1. 你需要一个 `visited` 数组和一个在递归时传递的 `parent`（父节点）参数。
> 2. `bool dfs(int u, int parent)`:
>    - 标记 `visited[u] = true`。
>    - 遍历 `u` 的所有邻居 `v`：
>      - **情况一：`v == parent`**
>        - 这是来时的路，忽略。
>      - **情况二：`visited[v] == true`** (且 `v != parent`)
>        - **找到环！** 你遇到了一个已被访问的、非父节点的节点。立即返回 `true`。
>      - **情况三：`visited[v] == false`**
>        - 递归 `dfs(v, u)` (此时 `u` 成为 `v` 的父节点)。
>        - 如果递归调用返回 `true`，说明在子树中找到了环，立即返回 `true`。
>    - 如果所有邻居都遍历完，没有发现环，返回 `false`。
>
> > **主函数**：你需要遍历所有节点。如果一个节点 `i` 尚未访问 (`!visited[i]`)，就从它开始 `dfs(i, -1)`。只要**任意一次** `dfs` 调用返回 `true`，整个图就存在环。
>
> 
>
> ### 方法二：并查集 (Union-Find)
>
> 
>
> 这是一个非常高效的专门用于无向图判环的方法：
>
> 1. 初始化并查集，每个节点都是一个单独的集合。
> 2. 遍历图中的**每一条边** `(u, v)`：
>    - 调用 `find(u)` 找到 `u` 的根节点 `rootU`。
>    - 调用 `find(v)` 找到 `v` 的根节点 `rootV`。
>    - **判断**：
>      - 如果 `rootU == rootV`，说明 `u` 和 `v` **已经**在同一个连通分量（集合）里了。现在这条新边 `(u, v)` 再次连接了它们，**构成了一个环**。立即返回 `true`。
>      - 如果 `rootU != rootV`，说明 `u` 和 `v` 分属两个不同集合。调用 `union(u, v)` 合并它们。
> 3. 如果遍历完所有边都没有返回 `true`，则图中无环，返回 `false`。
>
> ------
>
> 
>
> ## ✈️ 有向图 (Directed Graph)
>
> 
>
> 对于有向图，我们也有两种常用方法：
>
> 
>
> ### 方法一：DFS (三态标记法)
>
> 
>
> 这是最经典的方法，你需要一个数组来标记节点的三种状态：
>
> - `0`: **未访问** (unvisited)
> - `1`: **访问中** (visiting / on recursion stack)
> - `2`: **已访问** (visited / processing finished)
>
> 1. `bool dfs(int u)`:
>    - 标记 `visited[u] = 1` (访问中)。
>    - 遍历 `u` 的所有邻居 `v`（`u -> v`）：
>      - **情况一：`visited[v] == 1`**
>        - **找到环！** 你访问到了一个也正在"访问中"的节点（即 `v` 是 `u` 的祖先）。立即返回 `true`。
>      - **情况二：`visited[v] == 0`**
>        - 递归 `dfs(v)`。
>        - 如果递归调用返回 `true`，立即返回 `true`。
>      - **情况三：`visited[v] == 2`**
>        - `v` 节点已经处理完毕，属于其他分支，不是环。忽略。
>    - **回溯**：标记 `visited[u] = 2` (已访问)。
>    - 返回 `false`。
>
> > **主函数**：同样需要遍历所有节点。如果一个节点 `i` 的状态是 `0` (未访问)，就从它开始 `dfs(i)`。
>
> 
>
> ### 方法二：拓扑排序 (Kahn's Algorithm)
>
> 
>
> 这个方法基于**入度 (in-degree)**。有向无环图 (DAG) 必定可以进行拓扑排序，反之，如果**无法完成**拓扑排序，则说明图中**有环**。
>
> 1. **计算入度**：计算所有节点的入度（有多少条边指向它）。
> 2. **初始化队列**：将所有**入度为 0** 的节点放入一个队列 `q`。
> 3. **计数**：初始化一个计数器 `count = 0`。
> 4. **循环**：
>    - 当 `q` 不为空时：
>      - 出队一个节点 `u`。
>      - `count++`。
>      - 遍历 `u` 的所有邻居 `v`（`u -> v`）：
>        - 将 `v` 的入度减 1 (`indegree[v]--`)。
>        - 如果 `indegree[v]` 变为 0，将 `v` 入队。
> 5. **判断**：
>    - 循环结束后，比较 `count` 和图的总节点数 `N`。
>    - 如果 `count == N`，说明所有节点都成功入队并出队，拓扑排序完成，**无环**。
>    - 如果 `count < N`，说明有些节点（在环上）的入度永远无法变为 0，无法入队，**有环**。

#### dijkstra算法

```cpp
//未优化版
#include <bits/stdc++.h>
using namespace std;
int n,m;
int v1,v2,val;
int main(){
    cin>>n>>m;
    vector<vector<int>> grid(n+1,vector<int>(n+1,INT_MAX));
    vector<int> mindist(n+1,INT_MAX);
    vector<bool> vis(n+1,false);
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid[v1][v2]=val;
    }
    mindist[1]=0;
    for (int i=1;i<n;i++){
        int cur=-1;
        int minn=INT_MAX;
        for (int j=1;j<=n;j++){
            if (!vis[j]&&mindist[j]<minn){
                cur=j;
                minn=mindist[j];
            }
        }
        vis[cur]=true;
        for (int j=1;j<=n;j++){
            if (!vis[j]&&grid[cur][j]!=INT_MAX&&mindist[cur]+grid[cur][j]<mindist[j]){
                mindist[j]=grid[cur][j]+mindist[cur];
            }
        }
    }
    if (mindist[n]==INT_MAX){
        cout<<"-1"<<endl;
    }else{
        cout<<mindist[n]<<endl;
    }
    return 0;
}
```

思路和prim算法是很相似的，不过一个是处理有向图一个是处理无向图，而且这个算法的mindist是到起源的点。

```cpp
//堆优化版
#include <bits/stdc++.h>
using namespace std;
class cmp{
    public:
    bool operator()(const pair<int,int> &a,const pair<int,int> &b){
        return a.second>b.second;
    }
};
class node{
    public:
    int to;
    int v;
};
int n,m;
int v1,v2,val;
int main(){
    cin>>n>>m;
    vector<list<node>> grid(n+1);
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid[v1].push_back((node){v2,val});
    }
    int start=1;
    int end=n;
    vector<int> mindist(n+1,INT_MAX);
    vector<bool> vis(n+1,false);
    priority_queue<pair<int,int>,vector<pair<int,int>>,cmp> q;
    q.push(pair<int,int>(start,0));
    mindist[start]=0;
    while (!q.empty()){
        pair<int,int> cur=q.top();
        q.pop();
        if (vis[cur.first]) continue;
        vis[cur.first]=1;
        for (auto it:grid[cur.first]){
            if (!vis[it.to]&&mindist[cur.first]+it.v<mindist[it.to]){
                mindist[it.to]=mindist[cur.first]+it.v;
                q.push(pair<int,int>(it.to,mindist[it.to]));
            }
        }
    }
    if (mindist[end]==INT_MAX){
        cout<<"-1"<<endl;
    }else{
        cout<<mindist[end]<<endl;
    }
    return 0;
}
```

用堆其实就是省去了循环选最小边的那个过程，不过定义起来有点复杂麻烦，堆就这样。

好处是大大降低了时间复杂度喵。

这个只能用邻接表，用邻接矩阵就相当于没有优化了。

事实上就是让边带两个信息，一个是起始点，一个是后面接的边。

#### Bellman_ford算法

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,v1,v2,val;
int main(){
    cin>>n>>m;
    vector<vector<int>> grid;
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid.push_back({v1,v2,val});
    }
    int start=1;
    int end=n;
    vector<int> mindist(n+1,INT_MAX);
    vector<bool> vis(n+1,false);
    mindist[start]=0;
    for (int i=1;i<n;i++){
        for (vector<int> &it:grid){
            int from=it[0];
            int to=it[1];
            int len=it[2];
            if (mindist[from]!=INT_MAX&&mindist[to]>mindist[from]+len){
                mindist[to]=mindist[from]+len;
            }
        }
    }
    if (mindist[end]==INT_MAX){
        cout<<"unconnected"<<endl;
    }else{
        cout<<mindist[end]<<endl;
    }
    return 0;
}
```

##### 加入队列优化（SPFA）

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,v1,v2,val;
class node{
    public:
    int to;
    int len;
};
int main(){
    cin>>n>>m;
    vector<list<node>> grid(n+1);
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid[v1].push_back((node){v2,val});
    }
    vector<int> mindist(n+1,INT_MAX);
    vector<bool> vis(n+1,false);
    int start=1;
    int end=n;
    queue<int> q;
    q.push(start);
    vis[start]=true;
    mindist[start]=0;
    while (!q.empty()){
        int cur=q.front();
        q.pop();
        vis[cur]=false;
        for (auto it:grid[cur]){
            if (mindist[it.to]>mindist[cur]+it.len){
                mindist[it.to]=mindist[cur]+it.len;
                if (!vis[it.to]){
                    q.push(it.to);
                    vis[it.to]=true;
                }
            }
        }
    }
    if (mindist[end]==INT_MAX){
        cout<<"unconnected"<<endl;
    }else{
        cout<<mindist[end]<<endl;
    }
    return 0;
}
```

##### 用于判断有无负权回路（简单好写）

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,v1,v2,val;
int main(){
    cin>>n>>m;
    vector<vector<int>> grid;
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid.push_back({v1,v2,val});
    }
    int start=1;
    int end=n;
    vector<int> mindist(n+1,INT_MAX);
    vector<bool> vis(n+1,false);
    mindist[start]=0;
    int flag=0;
    for (int i=1;i<n;i++){
        for (vector<int> &it:grid){
            int from=it[0];
            int to=it[1];
            int len=it[2];
            if (mindist[from]!=INT_MAX&&mindist[to]>mindist[from]+len){
                mindist[to]=mindist[from]+len;
            }
        }
    }
    for (vector<int> &it:grid){
        int from=it[0];
        int to=it[1];
        int len=it[2];
        if (mindist[from]!=INT_MAX&&mindist[to]>mindist[from]+len){
            flag=1;
        }
    }
    if (flag) cout<<"circle"<< endl;
    else if(mindist[end]==INT_MAX){
        cout<<"unconnected"<<endl;
    }else{
        cout<<mindist[end]<<endl;
    }
    return 0;
}
```

我们知道，对于每一条边去进行一次松弛，假设一共有n个节点，那么只要松弛n-1次就能得到正确答案，而且n-1次之后，如果没有负权回路的话，再怎么松弛结果都不会再改变了。就可以利用这样子的一个特性去判断是否有负权回路。只要我们再松弛一次，观察数组的变化我们就可以知道是否有负权回路了。

##### SPFA带负环检测（事实上，这个在极端数据下发挥会更好）

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int to, w;
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T;
    cin >> T; // 多组测试
    while (T--) {
        int n, m;
        cin >> n >> m;

        vector<vector<Edge>> g(n + 1);
        for (int i = 0; i < m; i++) {
            int u, v, w;
            cin >> u >> v >> w;
            g[u].push_back({v, w});
            if (w >= 0) { 
                // 如果边权非负，加反向边，这样可检测无向负环
                g[v].push_back({u, w});
            }
        }

        // 初始化
        vector<int> dist(n + 1, 0);
        vector<int> cnt(n + 1, 0);
        vector<bool> inq(n + 1, false);
        queue<int> q;

        // 所有点都入队（关键点：防止非连通图漏检测负环）
        for (int i = 1; i <= n; i++) {
            q.push(i);
            inq[i] = true;
        }

        bool hasCycle = false;

        // SPFA 主体
        while (!q.empty() && !hasCycle) {
            int u = q.front(); q.pop();
            inq[u] = false;

            for (auto &e : g[u]) {
                int v = e.to, w = e.w;
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    cnt[v] = cnt[u] + 1;
                    if (cnt[v] >= n) { // 松弛超过 n-1 次 → 负环
                        hasCycle = true;
                        break;
                    }
                    if (!inq[v]) {
                        q.push(v);
                        inq[v] = true;
                    }
                }
            }
        }

        if (hasCycle) cout << "YES\n";
        else cout << "NO\n";
    }

    return 0;
}

```

##### 有限最短路（其实是带有负环的模板）

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <climits>
using namespace std;

int main() {
    int src, dst,k ,p1, p2, val ,m , n;
    
    cin >> n >> m;

    vector<vector<int>> grid;

    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        grid.push_back({p1, p2, val});
    }

    cin >> src >> dst >> k;

    vector<int> minDist(n + 1 , INT_MAX);
    minDist[src] = 0;
    vector<int> minDist_copy(n + 1); // 用来记录上一次遍历的结果
    for (int i = 1; i <= k + 1; i++) {
        minDist_copy = minDist; // 获取上一次计算的结果
        for (vector<int> &side : grid) {
            int from = side[0];
            int to = side[1];
            int price = side[2];
            // 注意使用 minDist_copy 来计算 minDist 
            if (minDist_copy[from] != INT_MAX && minDist[to] > minDist_copy[from] + price) {  
                minDist[to] = minDist_copy[from] + price;
            }
        }
    }
    if (minDist[dst] == INT_MAX) cout << "unreachable" << endl; // 不能到达终点
    else cout << minDist[dst] << endl; // 到达终点最短路径
}
```

> 如果做有负权回路的判断，都要在每一次松弛开始前记录上一次松弛的结果

> 非常棒的问题 ✅ 你已经踩到了 **Bellman-Ford 算法里一个非常细节但关键的点**。
>  你的直觉完全正确，只是要再明确一点 **“什么时候需要上一次的副本 minDist_copy”** 和 **“为什么”**。
>
> ------
>
> ## 🧩 我们先分清楚两个场景：
>
> | 场景                                      | 图中是否有负权       | 是否允许经过同一个点多次   | 算法                                       | 是否需要 `minDist_copy` |
> | ----------------------------------------- | -------------------- | -------------------------- | ------------------------------------------ | ----------------------- |
> | 普通最短路（如 Dijkstra）                 | 无负权               | 否                         | Dijkstra                                   | ❌ 不需要                |
> | 有负权边但无负环                          | 允许有限次松弛       | Bellman-Ford               | ✅ 需要                                     |                         |
> | 有负环但不要求检测（例如“最多走 k 条边”） | 有负权回路但限制边数 | 限制版 Bellman-Ford（k次） | ✅ 必须要                                   |                         |
> | 要检测负环                                | 有负环               | Bellman-Ford 检测模式      | ❌ 不需要 `copy`（因为松弛 n 次即可发现环） |                         |
>
> ------
>
> ## 🧩 为什么要 `minDist_copy`
>
> 假设我们在一次循环里直接这样写：
>
> ```cpp
> if (minDist[from] != INT_MAX && minDist[to] > minDist[from] + w)
>     minDist[to] = minDist[from] + w;
> ```
>
> 这意味着在 **同一轮松弛中更新的值会立即影响本轮后续的松弛**。
>
> 这在一般 Bellman-Ford（松弛 n−1 轮）中**是没问题的**，因为每轮都尝试遍历所有边，最后仍能收敛到正确最短路。
>
> 但在以下两种情况下，它会出错：
>
> ------
>
> ### ❌ 情况1：限制最多走 k 条边（比如“最多转 k 次机票”）
>
> 你这个程序其实就是这个问题的变体：
>
> > “给定最多能走 k 条边，求从 src 到 dst 的最短路径。”
>
> 在这种题目里：
>
> - 第 i 轮松弛表示：**用 i 条边以内可以到达的最短路**；
> - 所以第 i 轮的结果，**只能用第 i−1 轮的结果来推**。
>
> 👉 如果不加 `minDist_copy`，你在第 i 轮中更新的结果可能立即影响第 i 轮后面的边，相当于用了 **超过 i 条边** 的结果，
>  从而违反了“至多 k 条边”的限制。
>
> 因此在这种 **“有层数/边数限制”** 的 Bellman-Ford 里，
>  `minDist_copy` 是 **必须的**。
>
> ------
>
> ### ✅ 情况2：普通 Bellman-Ford（无 k 限制）
>
> 如果只是要求单源最短路（无边数限制），
>  Bellman-Ford 每轮遍历所有边 n−1 次，理论上不需要 `copy`，
>  因为允许更新立即生效不会改变最终正确性。
>
> ------
>
> ### ⚠️ 情况3：有负权回路但“题目保证不成环”
>
> 如果题目没负环，但你为了保险使用 `copy`，完全没问题，
>  只是略微慢一点（多一次数组拷贝）。
>
> ------
>
> ## 🔍 总结要点
>
> | 用途                             | 是否需要 `minDist_copy` | 说明                             |
> | -------------------------------- | ----------------------- | -------------------------------- |
> | 普通 Bellman-Ford 最短路         | ❌                       | 立即更新也行，n−1 轮后收敛       |
> | 检测负环                         | ❌                       | 立即更新没问题                   |
> | 限制边数 k（例如机票题）         | ✅                       | 必须要，用上一轮结果防止越界松弛 |
> | 防止被负环影响（题目保证有限步） | ✅                       | 可以防止误松弛进无限环           |
>
> ------
>
> ## ✅ 你的代码分析
>
> 你的版本其实是正确实现了**“最多经过 k 条边的最短路”**（类似 LeetCode 787 “K Stops Flight” 那类问题）：
>
> ```cpp
> minDist_copy = minDist;
> if (minDist_copy[from] != INT_MAX && minDist[to] > minDist_copy[from] + price)
>     minDist[to] = minDist_copy[from] + price;
> ```
>
> ✔️ 正确，逻辑上严格遵守“只使用前 k−1 层信息更新第 k 层”。
>
> 所以：
>
> - 如果题目要求**限制边数**，你的写法是 ✅ 必须的；
> - 如果是普通单源最短路（无 k 限制），就可以去掉 `minDist_copy`；
> - 如果图中存在负环，而你只跑固定 k 轮（不是检测负环），那就靠 `minDist_copy` 保证算法不会被负环无限松弛拖崩。
>
> ------
>
> ## ✅ 一句话总结：
>
> > 💡 在单源有限最短路问题中（比如“最多经过 k 条边”），
> >  每轮都要用上一次松弛的 `minDist_copy` 来保证状态层数不被串扰；
> >  否则算法会意外用到当前轮的新结果，相当于使用了更多边，结果错误。

#### Floyd算法

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,v1,v2,val;
int main(){
    cin>>n>>m;
    vector<vector<int>> grid(n+1,vector<int>(n+1,10010));
    for (int i=0;i<m;i++){
        cin>>v1>>v2>>val;
        grid[v1][v2]=val;
        grid[v2][v1]=val;
    }
    for (int k=1;k<=n;k++){
        for (int i=1;i<=n;i++){
            for (int j=1;j<=n;j++){
                grid[i][j]=min(grid[i][j],grid[i][k]+grid[k][j]);
            }
        }
    }
    int z,start,end;
    cin>>z;
    while(z--){
        cin>>start>>end;
        if (grid[start][end]!=10010){
            cout<<grid[start][end]<<endl;
        }else{
            cout<<"-1"<<endl;
        }
    }
    return 0;
}
```

这种算法是利用了动态规划的思想去处理多源最短路的问题，时间复杂度稍微有一点高。

### 总结篇

我将会在下面分为几种情况来选择不同的算法

***题目说明是无向图：***

**稠密图：**prim，基于点，mindist表示的是距离最小生成树的最近距离

**稀疏图：**Kruskal，基于边，利用贪心算法和并查集每次选取最短的边，边的两端如果不在同一个集合，那么就加入。

***题目说明是有向图且单源：***

**题目中不存在负边：**使用Dijkstra算法，其中的mindist表示的是点到源点的距离，与prim在写法上非常相似，唯一的不同只有mindist的差别（？）堆优化版需要转化思路存边。

**题目中存在负边且存在有负权回路：**使用最原始的B_f算法，对边进行松弛，或者使用带上了负环检测的SPFA。

**题目中存在负边且无负权回路：**使用SPFA，最高效。

**题目是问有限最短路：**用B_f算法的变体，记得每次循环都要拷贝mindist

***题目说明多源：***

使用Floyd算法。

## 行向深渊：旅途的终点？

2025.10.02——2025.10.29

在跳过有些题目和章节的情况下，我耗时一个月，刷完了代码随想录。

这份算法笔记我可以说是非常厉害，由浅入深，透彻，清晰，把我很多最开始没搞懂的算法全部揉碎了讲。

我可以肯定，我绝对认识比三个月前清晰很多很多。

接下来是二刷，我会以自己写blog的形式来进行复习和总结，不过在这之前我需要抓住10月的尾巴，去整理一下一些其他东西

这不是终点

——by Schariac125

2025.10.29 写于福州大学旗山校区

