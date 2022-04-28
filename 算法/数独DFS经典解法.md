```cpp
经典解法一维
#include <iostream> 
using namespace std;
       
bool sign = false;   
int num[9][9];
      
void Output()
{  
    for (int i = 0; i < 9; i++){
        for (int j = 0; j < 8; j++)
            cout << num[i][j] << " ";
        cout << num[i][8];
        cout << endl;
    }
}
  
/* 判断key填入n格时是否满足条件,n代表第几个格子 */
bool Check(int n, int key)
{
    /* 判断n所在横列是否合法 */
    for (int i = 0; i < 9; i++){
        /* j为n竖坐标 */
        int j = n / 9;
        if (num[j][i] == key)
            return false;
    }
     
    /* 判断n所在竖列是否合法 */
    for (int i = 0; i < 9; i++){
        /* j为n横坐标 */
        int j = n % 9;
        if (num[i][j] == key)
            return false;
    }
     
    int y = n / 9 / 3 * 3;
    int x = n % 9 / 3 * 3;    
    /* 判断n所在的小九宫格是否合法 */
    for (int i = y; i < y + 3; i++) 
        for (int j = x; j < x + 3; j++)
            if (num[i][j] == key)
                return false;
 
    return true;
}
  
/* 深搜 */
int DFS(int n)
{
    /* 所有的都符合，退出搜索,n代表格子数，共81个格子，0~80 */
    if (n > 80){
        sign = true;
        return 0;
    }
 
    if (num[n / 9][n % 9] != 0)
        DFS(n + 1); 
    else{
        /* 否则对当前位一次填入1~9进行测试 */
        for (int i = 1; i <= 9; i++){
            if (Check(n, i)){
                num[n / 9][n % 9] = i;
                /* 继续搜索，后续位也填1~9测试，直到最后一位，通过的话置true */
                DFS(n + 1);
                /* 返回时如果构造成功，则直接退出 */
                if (sign) 
                    return 0;
                /* 如果构造不成功，还原当前位 */
                num[n/9][n%9] = 0;
            }
                   
        }
    }
    return 0;
}
       
int main()
{ 
    for (int i = 0; i < 9; i++){
        for (int j = 0; j < 9; j++)
            cin >> num[i][j];
    }
    DFS(0);     //从第0格开始填
    Output();
}
=================================================================================================================================
我的解法二维
#include <iostream>
#include <string>
#include <cmath>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;
int dq[9][9] = { 0 };
bool check(int i,int j,int val) {
    for (int k = 0; k < 9;k++) {
        if (dq[k][j] == val)return 0;
    }
    for (int k = 0; k < 9; k++) {
        if (dq[i][k] == val)return 0;
    }
    for (int p = i/3*3; p < i / 3 * 3+3; p++) {
        for (int q = j / 3 * 3; q < j / 3 * 3+3; q++) {
            if(dq[p][q]==val)return 0;
        }
    }
    return 1;
}
int flag = 0;
int fun(int i, int j) {
    if (i > 8 || j > 8) {
        flag = 1;
        return 0;
    }
    if (dq[i][j] != 0) {
        if (i < 8)fun(i + 1, j);
        else fun(0, j + 1);
    }
    else {
        for (int k = 1; k <= 9;k++) {
            if (check(i, j, k)) {
                dq[i][j] = k;
                if (i < 8)fun(i + 1, j);
                else fun(0, j + 1);
                if (flag == 1)return 0;
                dq[i][j] = 0;
            }
        }
    }
    return 0;
}
int main(void) {
    for (int i = 0; i < 9;i++) {
        for (int j = 0; j < 9;j++) {
            cin >> dq[i][j];
        }
    }
    fun(0, 0);
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            cout<< dq[i][j]<<' ';
        }
        cout << endl;
    }
    return 0;
}
```
