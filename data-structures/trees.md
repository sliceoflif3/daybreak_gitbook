# 🌳 Trees

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

### 2-3 Tree

A 2-3 tree is always balanced.

2-3 trees permit the number of children of an internal node to vary between two and three.

The big difference is that nodes can contain more than one set of data.

If a node has two children, it must only contain 1 data item. But, if a node has three children, it must contain 2 data items.

For "nodes" that contain only one data item: there can be either no children or 2 children.

For "nodes" that contain two data items: there can be either no children or 3 children.

#### Implementation

```cpp
struct Node{
    int left_val, right_val;
    Node* leftNode, *rightNode, *middleNode;
    Node(){
        left_val = right_val = -1;
        leftNode = rightNode = middleNode = nullptr;
    }
};
```

#### Insert

```cpp
void insert(Node*&root, int&x, Node*&tNode1, Node*&tNode2){
    //check leaf
    if(!(root->leftNode||root->rightNode||root->middleNode)){
        if(root -> right_val==-1){ //only have 1 value
            root -> right_val = x;
            if(root -> right_val < root -> left_val) 
                swap(root -> right_val, root -> left_val);
            x = -1;
            tNode1 = nullptr;
            tNode2 = nullptr;
            return;
        } 
        else { //if already have 2 value then split
            vector<int> vec1;
            vec1.push_back(root -> left_val); vec1.push_back(root -> right_val);
            vec1.push_back(x);
            sort(vec1.begin(),vec1.end());
            tNode1 -> left_val = vec1[0]; tNode2 -> left_val = vec1[2];
            x = vec1[1];
            return;
        }
    } 
    // have child
    Node* tNode3 = new Node();
    Node* tNode4 = new Node();
    if(x < root->left_val){
        insert(root->leftNode,x,tNode3,tNode4);
    } 
    else if((root->right_val!=-1 && x > root->right_val)||(root->right_val==-1
    && x > root->left_val)){
        insert(root->rightNode,x,tNode3,tNode4);
    } 
    else{
        insert(root->middleNode,x,tNode3,tNode4);
    } 
    if(x==-1) return; //success
    //have 1 value
    if(root -> right_val == -1){
        if(x < root->left_val){ //2 new node on left
            root -> leftNode = tNode3;
            root -> middleNode = tNode4;
        } 
        else{ //2 new node on right
            root -> middleNode = tNode3;
            root -> rightNode = tNode4;
        } 
        root -> right_val = x;
        if(root -> left_val > root -> right_val) 
            swap(root -> right_val, root -> left_val);
        x=-1;
        return;
    } 
    // have 2 value
    vector<int> vec2;
    vec2.push_back(root -> right_val);
    vec2.push_back(root -> left_val);
    vec2.push_back(x);
    sort(vec2.begin(),vec2.end());
    tNode1 -> left_val = vec2[0];
    tNode2 -> left_val = vec2[2];
    if(x < root -> left_val){ //2 new node of left
        tNode1 -> leftNode = tNode3;
        tNode1 -> rightNode = tNode4;
        tNode2 -> leftNode = root -> middleNode;
        tNode2 -> rightNode = root -> rightNode;
    } 
    else if ((root -> right_val != -1 && x > root -> right_val) || (root ->
    right_val == -1 && x > root -> left_val)) { // 2 new Node is on the right
        tNode1 -> leftNode = root -> leftNode;tNode1 -> rightNode = root -> middleNode;
        tNode2 -> leftNode = tNode3;
        tNode2 -> rightNode = tNode4;
    } 
    else{ //middle
        tNode1 -> leftNode = root -> leftNode;
        tNode1 -> rightNode = tNode3;
        tNode2 -> leftNode = tNode4;
        tNode2 -> rightNode = root -> rightNode;
    } 
    x=vec2[1];
    return;
}
```

#### In order traverse

<pre class="language-cpp"><code class="lang-cpp"><strong>void inOrder(Node* root){
</strong>    if(!root) return;
    preOrder(root -> leftNode);
    cout &#x3C;&#x3C; root -> left_val &#x3C;&#x3C; ' ';
    preOrder(root -> middleNode);
    if (root -> right_val != -1)
        cout &#x3C;&#x3C; root -> right_val &#x3C;&#x3C; ' ';
    preOrder(root -> rightNode);
}</code></pre>

#### Print

```cpp
void print(Node* root){
    if(!root) return;
    cout<< root -> left_val << ' ' << root -> right_val<<": ";
    if(root -> leftNode) 
        cout<< "|L "<< root -> leftNode -> left_val <<' '<<root->leftNode -> right_val<<"|";
    if(root -> middleNode) 
        cout<< "|M "<< root -> middleNode -> left_val<<' '<<root->middleNode->right_val<<"|";
    if(root->rightNode) 
        cout<<"|R "<<root->rightNode->left_val<<' '<<root->rightNode->right_val<<"|";
    cout<<endl;
    print(root->leftNode);
    print(root->middleNode);
    print(root->rightNode);
    return;
}
```

#### Example main program

```cpp
int main(){
    Node* root = nullptr;
    int t;
    cout<<"Number of number: ";
    cin>>t;
    while(t--){
        int x;
        cin>>x;
        if(!root){
            root = new Node();
            root -> left_val = x;
        } 
        else {
            Node* tNode1 = nullptr;
            Node* tNode2 = nullptr;
            tNode1 = new Node();
            tNode2 = new Node();
            insert(root,x,tNode1,tNode2);
            if(x==-1) {} //success
            else {
                root -> left_val = x;
                root -> right_val = -1;
                root -> leftNode = tNode1;
                root -> middleNode = nullptr;
                root -> rightNode = tNode2;
            }
        }
    } 
    //print(root);
    inOrder(root);
}
```

### AVL Tree

After you insert/delete:

* The tree is checked to see if any node differs by more than 1 in height
* If it does, you rearrange the nodes to restore balance.
* What we do is rotate the tree to make it balanced. Rotations are not necessary after every insertion & deletion (it is only needed when the height differs by more than 1).
*   Left left case:&#x20;

    <figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
*   Left right case:&#x20;

    <figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
*   Right right case:&#x20;

    <figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
*   Right left case:&#x20;

    <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### Rotate implementation:

```cpp
Node *rightRotate(Node *y)
{
    Node *x = y->left;
    Node *T2 = x->right;
    // Perform rotation
    x->right = y;
    y->left = T2;
    // Return new root
    return x;
}
```

<pre class="language-cpp"><code class="lang-cpp"><strong>Node *leftRotate(Node *x)
</strong>{
    Node *y = x->right;
    Node *T2 = y->left;
    // Perform rotation
    y->left = x;
    x->right = T2;
    // Return new root
    return y;
}</code></pre>

### 2-3-4 tree

* a node can either be a leaf or,&#x20;
* if it has 1 data item there are 2 children,&#x20;
* 2 data items has 3 children,&#x20;
* and 3 data items has 4 children

Its insertion algorithm splits the nodes as it traverses down the tree toward a leaf, rather than upon the return to the root. As you travel down the tree to insert a data item,

* if you encounter a node with 3 pieces of data you immediate split the node at that time.
* then, you continue traveling towards a leaf to insert the data.

What this means is that the tree cannot contain all nodes with 3 pieces of data.

#### The advantage of both the 2-3 and 2-3-4 trees

* is that they are easy to maintain balance.
* where the 2-3-4 tree has an advantage is that the insertion/deletion algorithm require only one pass through the tree so they are simpler than those for a 2-3 tree.
* On the other hand, 2-3-4 trees require more storage than a binary search tree.

### Red-black tree

A red-black tree is a BST representation of a 2-3-4 tree with 2 extra fields in the node to represent whether the connection is within the current node or a child, and that bit is often interpreted as the color (red or black). These colors are used to ensure that the tree remains balanced during insertions and deletions.

#### Rules That Every Red-Black Tree Follows:

1. Every node has a color either red or black.
2. The root of the tree is always black.
3. There are no two adjacent red nodes (A red node cannot have a red parent or red child).
4. Every path from a node (including root) to any of its descendants NULL nodes has the same number of black nodes.

#### Comparison with AVL Tree:

The AVL trees are more balanced compared to Red-Black Trees, but they may cause more rotations during insertion and deletion. So if your application involves frequent insertions and deletions, then Red-Black trees should be preferred. And if the insertions and deletions are less frequent and search is a more frequent operation, then AVL tree should be preferred over Red-Black Tree.

Black height is the number of black nodes on a path from the root to a leaf. Leaf nodes are also counted black nodes. From the above properties 3 and 4, we can derive, a Red-Black Tree of height h has black-height >= h/2

#### Search

<pre class="language-cpp"><code class="lang-cpp">searchElement (tree, val)
Step 1:
If tree -> data = val OR tree = NULL
<strong>    Return tree
</strong>Else
    If val data
        Return searchElement (tree -> left, val)
    Else
        Return searchElement (tree -> right, val)
    [ End of if ]
[ End of if ]

Step 2: END</code></pre>

#### Insertion

In the Red-Black tree, we use two tools to do the balancing:

* Recoloring
* Rotation

We always try recoloring first, if recoloring doesn’t work, then we go for rotation.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

First, you have to insert the node similarly to that in a binary tree and assign a red colour to it. Now, if the node is a root node then change its colour to black, but if it does not then check the colour of the parent node. If its colour is black then don’t change the colour but if it is not i.e. it is red then check the colour of the node’s uncle. If the node’s uncle has a red colour then change the colour of the node’s parent and uncle to black and that of grandfather to red colour and repeat the same process for him (i.e. grandfather).

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

But, if the node’s uncle has black colour then there are 4 possible cases:

*   Left left case:&#x20;

    <figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
*   Left right case:&#x20;

    <figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
*   Right right case:&#x20;

    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
*   Right left case:&#x20;

    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Now, after these rotations, if the colours of the nodes are miss matching then recolour them.

```
Let x be the newly inserted node.
Perform standard BST insertion and make the colour of newly inserted nodes as
RED.
If x is the root, change the colour of x as BLACK (Black height of complete tree
increases by 1).
Do the following if the color of x’s parent is not BLACK and x is not the root.
a) If x’s uncle is RED (Grandparent must have been black from property 4)
(i) Change the colour of parent and uncle as BLACK.
(ii) Colour of a grandparent as RED.
(iii) Change x = x’s grandparent, repeat steps 2 and 3 for new x.
b) If x’s uncle is BLACK, then there can be four configurations for x, x’s parent
(p) and x’s grandparent (g) (This is similar to AVL Tree)
(i) Left Left Case (p is left child of g and x is left child of p)
(ii) Left Right Case (p is left child of g and x is the right child of p)
(iii) Right Right Case (Mirror of case i)
(iv) Right Left Case (Mirror of case ii)
```

### Tree reversal

#### In order

```
1. Traverse the left subtree, i.e., call Inorder(left-subtree)
2. Visit the root.
3. Traverse the right subtree, i.e., call Inorder(right-subtree)
```

```cpp
void printInorder(struct Node* node)
{
    if (node == NULL)
        return;
        
    /* first recur on left child */
    printInorder(node->left);
    
    /* then print the data of node */
    cout << node->data << " ";
    
    /* now recur on right child */
    printInorder(node->right);
}
```

#### Pre order

```
1. Visit the root.
2. Traverse the left subtree, i.e., call Preorder(left-subtree)
3. Traverse the right subtree, i.e., call Preorder(right-subtree)
```

```cpp
void printPreorder(struct Node* node)
{
    if (node == NULL)
        return;
        
    /* first print data of node */
    cout << node->data << " ";
    
    /* then recur on left sutree */
    printPreorder(node->left);
    
    /* now recur on right subtree */
    printPreorder(node->right);
}
```

#### Post order

```
1. Traverse the left subtree, i.e., call Postorder(left-subtree)
2. Traverse the right subtree, i.e., call Postorder(right-subtree)
3. Visit the root.
```

```cpp
void printPostorder(struct Node* node)
{
    if (node == NULL)
        return;
        
    // first recur on left subtree
    printPostorder(node->left);
    
    // then recur on right subtree
    printPostorder(node->right);
    
    // now deal with the node
    cout << node->data << " ";
}
```

#### Example

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

```
(a) Inorder (Left, Root, Right) : 4 2 5 1 3
(b) Preorder (Root, Left, Right) : 1 2 4 5 3
(c) Postorder (Left, Right, Root) : 4 5 2 3 1
```
