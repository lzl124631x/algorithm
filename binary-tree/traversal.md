# Traversal

## Preorder

[144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

See more my solutions [here](https://github.com/lzl124631x/LeetCode/tree/master/leetcode/144.%20Binary%20Tree%20Preorder%20Traversal)

```cpp
// OJ: https://leetcode.com/problems/binary-tree-preorder-traversal/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(H)
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return {};
        vector<int> ans;
        stack<TreeNode*> s{{root}};
        while (s.size()) {
            root = s.top();
            s.pop();
            ans.push_back(root->val);
            if (root->right) s.push(root->right);
            if (root->left) s.push(root->left);
        }
        return ans;
    }
};
```

Or

```cpp
// OJ: https://leetcode.com/problems/binary-tree-preorder-traversal/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(H)
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> s;
        while (root || s.size()) {
            if (!root) {
                root = s.top();
                s.pop();
            }
            ans.push_back(root->val);
            if (root->right) s.push(root->right);
            root = root->left;
        }
        return ans;
    }
};
```

See also [589. N-ary Tree Preorder Traversal (Easy)](https://leetcode.com/problems/n-ary-tree-preorder-traversal)

## Inorder

[94. Binary Tree Inorder Traversal (Easy)](https://leetcode.com/problems/binary-tree-inorder-traversal/)

See more my solutions [here](https://github.com/lzl124631x/LeetCode/blob/master/leetcode/94.%20Binary%20Tree%20Inorder%20Traversal)

```cpp
// OJ: https://leetcode.com/problems/binary-tree-inorder-traversal/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(H)
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> s;
        while (root || s.size()) {
            while (root) {
                s.push(root);
                root = root->left;
            }
            root = s.top();
            s.pop();
            ans.push_back(root->val);
            root = root->right;
        }
        return ans;
    }
};
```

## Postorder

[145. Binary Tree Postorder Traversal (Easy)](https://leetcode.com/problems/binary-tree-postorder-traversal/)

See more my solutions [here](https://github.com/lzl124631x/LeetCode/blob/master/leetcode/145.%20Binary%20Tree%20Postorder%20Traversal)

```cpp
// OJ: https://leetcode.com/problems/binary-tree-postorder-traversal/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(H)
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> s;
        TreeNode *prev = nullptr;
        while (root || s.size()) {
            while (root) {
                s.push(root);
                root = root->left;
            }
            root = s.top();
            if (!root->right || root->right == prev) { // if root->right is nonexistent or visited, visit root
                ans.push_back(root->val);
                s.pop();
                prev = root;
                root = nullptr;
            } else root = root->right; // otherwise, visit right subtree
        }
        return ans;
    }
};
```

See also [590. N-ary Tree Postorder Traversal (Easy)](https://leetcode.com/problems/n-ary-tree-postorder-traversal)

## Level-order

[102. Binary Tree Level Order Traversal \(Medium\)](https://leetcode.com/problems/binary-tree-level-order-traversal/)

See more my solutions [here](https://github.com/lzl124631x/LeetCode/blob/master/leetcode/102.%20Binary%20Tree%20Level%20Order%20Traversal)

```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        while (q.size()) {
            int cnt = q.size();
            ans.emplace_back();
            while (cnt--) {
                root = q.front();
                q.pop();
                ans.back().push_back(root->val);
                if (root->left) q.push(root->left);
                if (root->right) q.push(root->right);
            }
        }
        return ans;
    }
};
```

* [2641. Cousins in Binary Tree II (Medium)](https://leetcode.com/problems/cousins-in-binary-tree-ii)

## Other variations

* [107. Binary Tree Level Order Traversal II \(Easy\)](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
* [103. Binary Tree Zigzag Level Order Traversal \(Medium\)](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Problems

* [124. Binary Tree Maximum Path Sum \(Hard\)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

