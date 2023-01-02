# Kth Row of Pascal's Triangle 
https://www.interviewbit.com/problems/kth-row-of-pascals-triangle/

```
vector<int> Solution::getRow(int n) {
    vector <int> ans;
    int c = 1;
    for (int i = 1; i <= n+1; i++) {
        ans.push_back(c);
        c = c*(n+1-i) / i;
    }
    return ans;
}

```

# Rotate Matrix
https://www.interviewbit.com/problems/rotate-matrix/

```
void Solution::rotate(vector<vector<int> > &matrix) {
   reverse(matrix.begin(), matrix.end());
    for(int i = 0; i<matrix.size(); i++) {
        for(int j = i + 1; j<matrix[i].size(); j++) 
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

# Max Sum Contiguous Subarray
https://www.interviewbit.com/problems/max-sum-contiguous-subarray/

```
int Solution::maxSubArray(const vector<int> &A) {
    int max_so_far = A[0], max_end_here = A[0], i;
    for (i = 1; i < A.size(); ++i) {
        max_end_here = std::max(A[i],max_end_here+A[i]);
        max_so_far = std::max(max_end_here, max_so_far);
    }
    return max_so_far;
}
```

# Find Duplicate in Array
https://www.interviewbit.com/problems/find-duplicate-in-array/

```
int Solution::repeatedNumber(const vector<int> &V) {
    assert(V.size() >= 1 && V.size() <= 1e6);
    for( auto it: V ) assert(it >= 1 && it <= V.size()); 
    if (V.size() <= 1) return -1;
    int valueRange = V.size() - 1; // 1 to N when the size is N+1.
    int range = sqrt(valueRange);
    if (range * range < valueRange) range++;
    int count[range + 1];
    memset(count, 0, sizeof(count));

    for (int i = 0; i < V.size(); i++) {
    count[(V[i] - 1) / range]++;
    }

    int repeatingRange = -1;
    int numRanges = ((valueRange - 1) / range) + 1;
    for (int i = 0; i < numRanges && repeatingRange == -1; i++) {
        if (i < numRanges - 1 || valueRange % range == 0) {
            if (count[i] > range) repeatingRange = i;
        } else {
            if (count[i] > valueRange % range) repeatingRange = i;
        }
    }
    if (repeatingRange == -1) return -1;
    memset(count, 0, sizeof(count));
    for (int i = 0; i < V.size(); i++) {
        if ((V[i] - 1) / range == repeatingRange) count[(V[i] - 1) % range]++;
    }
    for (int i = 0; i < range; i++) {
        if (count[i] > 1) {
            return repeatingRange * range + i + 1;
        }
    }
    return -1;
}
```
#### Another Solution
```
int Solution::repeatedNumber(const vector<int> &A) {
    
    int i,xor1=0,xor2=1;
    for(i=0;i<A.size();i++)
    {
        xor1^=A[i];
    }
    int n=A.size();
    for(i=2;i<=(n-1);i++)
    {
        xor2^=i;
    }
    
    int repeat;
    repeat=xor1^xor2;
    
    return repeat;
    
}
```

# Merge Intervals
https://www.interviewbit.com/problems/merge-intervals/

```
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
vector<Interval> Solution::insert(vector<Interval> &intervals, Interval newInterval) { 
    vector<Interval> res;
    for (Interval test : intervals) {
        if (newInterval.start > test.end) res.push_back(test);
        else if(test.start > newInterval.end) {
            res.push_back(newInterval);
            newInterval = test;
        } else if (newInterval.start <= test.end || newInterval.end >= test.start) {
            newInterval = Interval(min(test.start, newInterval.start), max(test.end, newInterval.end));
        }
    } 
    res.push_back(newInterval);
    return res;
}
```
#### Another Solution
```
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
 bool cmp(Interval a, Interval b)
 {
     return a.start<=b.start;
 }
vector<Interval> Solution::insert(vector<Interval> &A, Interval B) {
   
     A.push_back(B);
     sort(A.begin(),A.end(),cmp);
    int j=0,i=0,k;
    for(i=0;i<A.size();i++)
    {
        int mn = A[i].start;
        int mx = A[i].end;
        k=i+1;
        for(;k<A.size();k++)
        {
            if(A[k].start<=mx)
            {
                mx = max(mx,A[k].end);
                mn = min(A[i].start,mn);
            }
            else
            {
                k--;
                break;
            }
        }
        
        i = k;
        
        A[j].start = mn;
        A[j].end = mx;
        j++;
    }
    
    A.erase(A.begin()+j,A.end());
    return A;
}
```

# Spiral Order Matrix I
https://www.interviewbit.com/problems/spiral-order-matrix-i/

```
vector<int> Solution::spiralOrder(const vector<vector<int> > &A) {
	vector<int> result;
	int r=A.size(), c=A[0].size();
	int r_beg=0,r_end=r,c_beg=0,c_end=c;
    while(r_beg<r_end && c_beg<c_end){
        for(int i=c_beg;i<c_end;i++)
            result.push_back(A[r_beg][i]);
        for(int i=r_beg+1;i<r_end;i++)
            result.push_back(A[i][c_end-1]);
        for(int i=c_end-2;i>=c_beg && (r_end-1-r_beg)>0;i--)
            result.push_back(A[r_end-1][i]);
        for(int i=r_end-2;i>r_beg && (c_end-1-c_beg)>0;i--)
            result.push_back(A[i][c_beg]);
        r_beg++;r_end--;
        c_beg++;c_end--;
    }
	return result;
}
```
#### Another Solution
```
vector<int> Solution::spiralOrder(const vector<vector<int> > &A) {
    int dir = 0;
    vector<int> B;
    int m = A.size();
    int n = A[0].size();
    int t=0, l=0, b = m-1, r=n-1;
    while(t<=b && l<=r){
        if(dir==0){
            for(int i=l; i<=r; i++){
                B.push_back(A[t][i]);
            }
            t++;
            dir=1;
        }
        else if(dir==1){
            for(int i=t; i<=b; i++){
                B.push_back(A[i][r]);
            }
            r--;
            dir=2;
        }
        else if(dir==2){
            for(int i=r; i>=l; i--){
                B.push_back(A[b][i]);
            }
            b--;
            dir=3;
        }
        else if(dir==3){
            for(int i=b; i>=t; i--){
                B.push_back(A[i][l]);
            }
            l++;
            dir=0;
        }
        else break;
    }
    return B;
}
```

# Repeat and Missing Number Array
https://www.interviewbit.com/problems/repeat-and-missing-number-array/

```
vector<int> Solution::repeatedNumber(const vector<int> &A) {
    vector<int> ret(2);
    long long sumOfA = 0, sumOfA2 = 0;
    long long temp;
    int retA, retB;
    int n = A.size();
    for (int i = 0; i < n; i++) {
        temp = A[i];
        sumOfA += temp;
        sumOfA2 += temp*temp;
        temp = i + 1;
        sumOfA -= temp;
        sumOfA2 -= temp*temp;
    }
    sumOfA2 = sumOfA2/sumOfA;
    retA = (int)((sumOfA + sumOfA2)/2);
    retB = (int)(sumOfA2-retA);
    ret[0] = retA;
    ret[1] = retB;
    return ret;
}
```

#### Another Solution
```
vector<int> Solution::repeatedNumber(const vector<int> &A) {
    int a, b;
    vector<bool> temp(A.size(), false);
    for(int i=0;i<A.size();i++){
        if(temp[A[i]]) a = A[i];
        temp[A[i]] = true;
    }

    for(int i=1; i<=A.size();i++){
        if(temp[i]==false) b = i;
    }

    vector<int> ans;
    ans.push_back(a); ans.push_back(b); 
    return ans;
}
```

# Merge Overlapping Intervals
https://www.interviewbit.com/problems/merge-overlapping-intervals/

```
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
bool compare(Interval A,Interval B) 
{
    return A.start < B.start;
}
vector<Interval> Solution::merge(vector<Interval> &A) {
int n=A.size(); 
sort(A.begin(),A.end(),compare); 
//for(int i=0;i<A.size();i++) 
    //cout<<A.start<<" ";
vector<Interval> ans;
Interval b=Interval(0,0); 
for(int i=0;i<A.size();i++) 
{
    if(i==0) 
    {
        b.start=A[i].start;
        b.end=A[i].end;
    } 
    else 
    {
        if(A[i].start <= b.end) 
            b.end=max(b.end,A[i].end);  // change here for overlapping with same value 
        else 
        {
            ans.push_back(b);
            b=Interval(A[i].start,A[i].end);
        }  

    }
} 
ans.push_back(b);
return ans;


}

```

# Set Matrix Zeros
https://www.interviewbit.com/problems/set-matrix-zeros/

```
void Solution::setZeroes(vector<vector<int> > &matrix) {
    int rownum = matrix.size();
    if (rownum == 0)  return;
    int colnum = matrix[0].size();
    if (colnum == 0)  return;

    bool hasZeroFirstRow = false, hasZeroFirstColumn = false;

    // Does first row have zero?
    for (int j = 0; j < colnum; ++j) {
        if (matrix[0][j] == 0) {
            hasZeroFirstRow = true;
            break;
        }
    }

    // Does first column have zero?
    for (int i = 0; i < rownum; ++i) {
        if (matrix[i][0] == 0) {
            hasZeroFirstColumn = true;
            break;
        }
    }

    // find zeroes and store the info in first row and column
    for (int i = 1; i < rownum; ++i) {
        for (int j = 1; j < colnum; ++j) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    // set zeroes except the first row and column
    for (int i = 1; i < rownum; ++i) {
        for (int j = 1; j < colnum; ++j) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0)  matrix[i][j] = 0;
        }
    }

    // set zeroes for first row and column if needed
    if (hasZeroFirstRow) {
        for (int j = 0; j < colnum; ++j) {
            matrix[0][j] = 0;
        }
    }
    if (hasZeroFirstColumn) {
        for (int i = 0; i < rownum; ++i) {
            matrix[i][0] = 0;
        }
    }
}
```

#### Another Solution
```
void Solution::setZeroes(vector<vector<int> > &A) {
   int k=0;
   int r=0;
   for(int i=0;i<A.size();i++){
       if(A[i][0]==0){
           k=1;
           break;
       }
   }
   for(int j=0;j<A[0].size();j++){
       if(A[0][j]==0){
           r=1;
           break;
       }
   }
   
   for(int i=1;i<A.size();i++){
       for(int j=1;j<A[0].size();j++){
           if(A[i][j]==0)
           {
               A[i][0]=0;
               break;
           }
       }
   }
   for(int j=1;j<A[0].size();j++){
       for(int i=1;i<A.size();i++){
           if(A[i][j]==0){
               A[0][j]=0;
               break;
           }
       }
   }
   for(int i=1;i<A.size();i++){
       for(int j=1;j<A[0].size();j++){
           if(A[0][j]==0||A[i][0]==0){
               A[i][j]=0;
           }
       }
   }
   if(k==1){
       for(int i=0;i<A.size();i++){
            A[i][0]=0;
        }
    }
    if(r==1){
        for(int j=0;j<A[0].size();j++){
            A[0][j]=0;
        }
    }
}

```
# Spiral Order Matrix II
https://www.interviewbit.com/problems/spiral-order-matrix-ii/

```
vector<vector<int> > Solution::generateMatrix(int A) {
    vector< vector<int> > arr(A, vector<int> (A, 0));
    int start,end,i;
    long long int x;
    x=1;
    for(start=0,end=A-1;start<=end;start++,end--)
    {
        for(i=start;i<=end;i++) { arr[start][i]=x; x++;}
        for(i=start+1;i<=end;i++) { arr[i][end]=x; x++;}
        for(i=end-1;i>=start;i--) { arr[end][i]=x; x++;}
        for(i=end-1;i>=start+1;i--) { arr[i][start]=x; x++;}
    }
    return arr;
}
```

# Largest Number
https://www.interviewbit.com/problems/largest-number/

```
bool compareNum(string a, string b) {
    return a + b > b + a;
}
string Solution::largestNumber(const vector < int > & A) {
    string result;
    vector < string > str;
    for (int i = 0; i < A.size(); i++) {
        str.push_back(to_string(A[i]));
    }
    sort(str.begin(), str.end(), compareNum);
    for (int i = 0; i < str.size(); i++) {
        result += str[i];
    }

    int pos = 0;
    while (result[pos] == '0' && pos + 1 < result.size()) pos++;
    return result.substr(pos);
}

```

# First Missing Integer
https://www.interviewbit.com/problems/first-missing-integer/

```
int Solution::firstMissingPositive(vector<int> &A) {
    int n = A.size();
    for (int i = 0; i < n; i++) {
        if (A[i] > 0 && A[i] <= n) {
            int pos = A[i] - 1;
            if (A[pos] != A[i]) {
                swap(A[pos], A[i]);
                i--;
            }
        }
    }
    for (int i = 0; i < n; i++) {
        if (A[i] != i + 1) return (i + 1);
    }
    return n + 1;
}
```













