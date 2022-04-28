```cpp
#include <iostream>
#include <string>
#include <cmath>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;
int m, n;
vector<vector<int>> maze;
vector<vector<int>> tmp;
vector<vector<int>> ma;
void fun(int i,int j) {
    maze[i][j] = 1;
    tmp.push_back({ i,j });
    if (i == m - 1 && j == n - 1) {
        if (ma.empty() || tmp.size() < ma.size())ma = tmp;
    }
    if (i - 1 >= 0 && maze[i - 1][j] == 0)fun(i - 1, j);
    if (i + 1 < maze.size() && maze[i + 1][j] == 0)fun(i + 1, j);
    if (j - 1 >= 0 && maze[i][j-1] == 0)fun(i, j-1);
    if (j + 1 < maze[0].size() && maze[i][j+1] == 0)fun(i, j+1);
    maze[i][j] = 0;
    tmp.pop_back();
}
int main(void) {
    while (cin>>m>>n) {
        tmp.clear();
        ma.clear();
        maze = vector<vector<int>>(m, vector<int>(n, 0));
        for (int i = 0; i < m;i++) {
            for (int j = 0; j < n;j++) {
                cin >> maze[i][j];
            }
        }
        fun(0, 0);
        for (int i = 0; i < ma.size(); i++) {
            cout << '(' << ma[i][0] << ',' << ma[i][1] << ')' << endl;
        }
    }
    return 0;
}
```
