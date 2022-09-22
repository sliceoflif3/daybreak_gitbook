# Trees

Trees are used to represent the relationship between data items. Height: The number of nodes on the longest path from root to a leaf.

## Binary tree

A binary tree is a tree where each node has no more than 2 children

### Binary Search Trees

For a binary search tree, it is really sorted according to the key values in the nodes. It allows us to traverse a binary tree and get our data in sorted order! For each node n, all values greater than n are located in the right subtree...all values less than n are located in the left subtree. Both subtrees are considered to be binary trees themselves. A full binary tree has all of its leaves at level h.

#### Implementing

```cpp
struct node {
    int val;
    node* left;
    node* right;
};

void insert(node*&root, int key) {
    if(!root){
        root=new node;
        root->val=key;
        root->left=NULL;
        root->right=NULL;
        return;
    }
    if(key==root->val) return;
    if(key<root->val) {
        insert(root->left,key);
    } 
    else {
    insert(root->right,key);
    }
}
```

#### Search

```cpp
node* search(node* root, int key) {
    if (root==NULL || root->val==key)
        return root;
    if (root->val<key)
        return search(root->right,key);
    else return search(root->left,key);
}
```

#### Delete entire tree

```cpp
void deleteTree(node* root){
    if(root==NULL) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}
```

#### Find height

```cpp
int findHeight(node* root){
    int heightLeft=-1;
    int heightRight=-1;
    if(root->left!=NULL)
        heightLeft = findHeight(root->left);
    if(root->right!=NULL)
        heightRight = findHeight(root->right);
    if(heightLeft>heightRight)
        return heightLeft+1;
    else
        return heightRight+1;
}
```

#### Display

```cpp
void display(node* root){
    if(root==NULL) return;
    display(root->left);
    cout<<root->val<<" ";
    display(root->right);
}
```

#### Display all leaf

```cpp
void displayLeaf(node* root){
    if(!root) return;
    if(!root->left && !root->right) {
        cout<<root->val<<" ";
        return;
    } 
    if(root->left){
        displayLeaf(root->left);
    } 
    if(root->right){
        displayLeaf(root->right);
    }
}
```

#### Lowest common ancestor

```cpp
node* lca(node* root, int n1, int n2){
    if(root==nullptr) return nullptr;
    if(root->val>n1 && root->val>n2){
        return lca(root->left,n1,n2);
    } 
    if(root->val<n2 && root->val<n1){
        return lca(root->right,n1,n2);
    } 
    return root;
}
```

#### Display k-th node

```cpp
void displayKthNode(node* root, int k){
    if(!root) return;
    if(k==1){
        cout<<root->val<<" ";
    } 
    else {
        displayKthNode(root->left,k-1);
        displayKthNode(root->right,k-1);
    }
}
```

#### Display path

```cpp
void displayPath(node* root, int x){
    node* nodeX = search(root,x);
    if(nodeX==NULL) return;
    nodeX=root;
    while(nodeX->val!=x){
        cout<<nodeX->val<<" ";
        if(nodeX->val<x)
            nodeX=nodeX->right;
        else
            nodeX=nodeX->left;
    } 
    cout<<nodeX->val;
}
```

#### Min node

```cpp
node* minNode(node* tree){
    node* cur= tree;
    while(cur->left!=nullptr)
        cur=cur->left;
    return cur;
}
```

#### Delete node

```cpp
node* deleteNode(node* tree, int x){
    if(tree==nullptr)
        return tree;
    if(x<tree->val){
        tree->left=deleteNode(tree->left,x);
    } 
    else if(x>tree->val){
        tree->right=deleteNode(tree->right,x);
    } 
    else{
        if(tree->left==nullptr){
            node* temp= tree->right;
            delete tree;
            return temp;
        }
        else if(tree->right==nullptr){
            node* temp = tree->left;
            delete tree;
            return temp;
        } 
        node* temp = minNode(tree->right);
        tree->val=temp->val;
        tree->right=deleteNode(tree->right,temp->val);
    } 
    return tree;
}
```
