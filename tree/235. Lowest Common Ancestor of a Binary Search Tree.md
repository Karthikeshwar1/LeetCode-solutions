**C++ solution:**

**1. With explanation**

```
class Solution {
private:
    struct Status {
        // number of target nodes found
        // num_target_nodes can be 0, 1 or 2
        int num_target_nodes;
        TreeNode* ancestor;
    };
    Status LCAHelper(TreeNode* root, TreeNode* p, TreeNode* q) {
        // if root is null, then it has no children
        // hence no LCA
        if (!root) {
            return {0, nullptr};
        }
        
        // find LCA in the left subtree
        auto left_result = LCAHelper(root->left, p, q);
        // if two target nodes are found, it means that
        // both the child nodes lie in this subtree
        if (left_result.num_target_nodes == 2) {
            return left_result;
        }
        
        // find LCA in the right subtree
        auto right_result = LCAHelper(root->right, p, q);
        // if two target nodes are found, it means that
        // both the child nodes lie in this subtree
        if (right_result.num_target_nodes == 2) {
            return right_result;
        }
        
        // if left_result has found < 2 target nodes
        // and also right_result has found < 2 target nodes
        // then there is a possibility that the current node
        // is one of the childs, and also the LCA
        int num_target_nodes = (left_result.num_target_nodes) +
                            (right_result.num_target_nodes) +
                            (root == p) + (root == q);
        
        // Now check if the above mentioned possiblity is true
        // If true, we have found LCA, otherwise we haven't
        TreeNode* LCA;
        if (num_target_nodes == 2) {
            LCA = root;
        } else {
            LCA = nullptr;
        }
        
        return {num_target_nodes, LCA};
    }
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return LCAHelper(root, p, q).ancestor;
    }
};
```

<br>

**2. Without explanation** (for code analyzing)

```
class Solution {
private:
    struct Status {
        int num_target_nodes;
        TreeNode* ancestor;
    };
    Status LCAHelper(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) {
            return {0, nullptr};
        }
        
        auto left_result = LCAHelper(root->left, p, q);
        if (left_result.num_target_nodes == 2) {
            return left_result;
        }
        
        auto right_result = LCAHelper(root->right, p, q);
        if (right_result.num_target_nodes == 2) {
            return right_result;
        }
        
        int num_target_nodes = (left_result.num_target_nodes) +
                            (right_result.num_target_nodes) +
                            (root == p) + (root == q);
        
        TreeNode* LCA;
        if (num_target_nodes == 2) {
            LCA = root;
        } else {
            LCA = nullptr;
        }
        
        return {num_target_nodes, LCA};
    }
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return LCAHelper(root, p, q).ancestor;
    }
};
```

<br>

Time complexity: **O(n)**
Space complexity: **O(h)** [where h = height of the tree, which could be O(logn) or O(n) in the worst case)

Code inspired from the book [EPI](https://elementsofprogramminginterviews.com/) 

<br>

**3. Quick solution**

Traversing the tree according to whether both the childs p and q are greater or lesser than the current root, leads directly to the LCA.

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while (root) {
            if (p->val < root->val && root->val > q->val) {
                root = root->left;
            } else if (p->val > root->val && root->val < q->val) {
                root = root->right;
            } else {
                return root;
            }
        }
        
        return nullptr;
    }
};
```

<br>

Time complexity: **O(n)**
Space complexity: **O(1)**

Code inspired from [Stefan Pochmann](https://leetcode.com/StefanPochmann/)'s [solution](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/discuss/64963/3-lines-with-O(1)-space-1-Liners-Alternatives)

<br>
