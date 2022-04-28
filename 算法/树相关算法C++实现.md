### 树遍历方法大全

```c
void fun(TreeNode* root) {
    if (!root)return;
    cout << root->val<<',';
    fun(root->left);
    fun(root->right);
}//递归先序遍历，中序、后序遍历同理
void fun(TreeNode* root) {
    queue<TreeNode*> tmp;
    tmp.push(root);
    while (!tmp.empty()) {
        TreeNode* x = tmp.front();
        if(x->left)tmp.push(x->left);
        if (x->right)tmp.push(x->right);
        cout << x->val << ' ';
        tmp.pop();
    }
}//层序遍历
void fun(TreeNode* root) {
    stack<TreeNode*> tmp;
    while (!tmp.empty()||root) {
        while (root) {
            tmp.push(root);
            root = root->left;
        }
        root = tmp.top();       
        tmp.pop();
		cout << root->val << ' ';
        root = root->right;
    }   
}//非递归中序遍历，利用栈
void fun(TreeNode* root) {
    stack<TreeNode*> tmp;
    while (!tmp.empty()||root) {
        while (root) {
            tmp.push(root);
          	cout << root->val << ' ';
            root = root->left;
        }
        root = tmp.top();       
        tmp.pop();
        root = root->right;
    }   
}//和非递归中序遍历比仅更改打印位置
void trifun(TreeNode *root){
    stack<pair<TreeNode *,bool>> s;
    s.push(make_pair(root, false));
    bool visited;
    while(!s.empty()){
        root = s.top().first;
        visited = s.top().second;
        s.pop();
        if(root == NULL)continue;
        if(visited)cout<<root->val<<' ';
        else{
            s.push(make_pair(root, true));
            s.push(make_pair(root->right, false));
            s.push(make_pair(root->left, false));//后序遍历，压栈顺序与自然顺序相反，为中->右->左
        }
    }
}//树三种非递归遍历的改版
```

### 一个二叉树递归的经典问题

题目：输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一

直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

思路分析：

（1）首先该题是基于递归去遍历整棵树，遍历完每一条路径，遍历的顺序是先根节点，然后是左节点，接着是右节点；

（2）如果节点的左右子树都为空，且路径之和等于参数，就说明该路径是需要输出的

（3）如果不满足条件，在遍历完之后需要把最后一颗节点弹出来。

```c
class Solution {
private:
	vector<vector<int>>ret;
	vector<int>temp;
public:
    void def(TreeNode* root,int expectNumber){
        temp.push_back(root->val);
        if(expectNumber - root->val ==0 &&root->left == NULL &&root->right== NULL)ret.push_back(temp);
        else{
            if(root->left != NULL)def(root->left,expectNumber - root->val);
            if(root->right != NULL)def(root->right,expectNumber - root->val);
        }
        temp.pop_back();
    }
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root == NULL)return ret;
        def(root,expectNumber);
        return ret;
    }  
};
```
