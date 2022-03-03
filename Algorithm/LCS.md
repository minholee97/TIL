# LCS

> Longest Common Substring vs Longest Common Subsequence

- 두 수열(문자열)중 가장 길고 공통된 부분을 찾는 알고리즘
- Substring는 문자들이 연속되어야 하는 경우에 사용하고 Subsequence는 문자들이 연속되지 않아도 되는 경우에 사용함.

### **1**. LCS (Longest Common Substring) 최장 공통 부분 문자열
- 두 문자열이 공통적으로 가진 부분 문자열 중 가장 긴 부분 문자열
```
// 비교할 두 문자열 앞에 마진(길이 1) 추가

for (int i = 0; i < length1; i++) {
    for (int j = 0; j < length2; j++) {
        if (i == 0 || j == 0)
            LCS[i][j] = 0;
        else if (str1[i] == str2[j]) 
            LCS[i][j] = LCS[i-1][j-1] + 1;
        else
            LCS[i][j] = 0;
    }
}
```


### **2**. LCS (Longest Common Subsequence) 최장 공통 부분 수열
- 두 수열이 공통적으로 가진 부분 수열 중 가장 긴 부분 수열
```
// 비교할 두 문자열 앞에 마진(길이 1) 추가

for (int i = 0; i < length1; i++) {
    for (int j = 0; j < length2; j++) {
        if (i == 0 || j == 0)
            LCS[i][j] = 0;
        else if (str1[i] == str2[j])
            LCS[i][j] = LCS[i-1][j-1] + 1;
        else
            LCS[i][j] = Math.max(LCS[i-1][j], LCS[i][j-1]);
    }
}
```
