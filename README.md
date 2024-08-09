# Trees

Tree: Each node is pointing to number of nodes. Non-linear data structure. A tree structure is a way of representing hierarchal nature of a structure in a graphical form. In tree order of elements is not important. If we need ordering information linear data structure like linked list, stacks, queues can be used. A special kind of ordering where left child is smaller than parent node and right child is greater is called BST. 

======
Binary Tree Data Structure: A tree whose elements have at most 2 children is called a binary tree. Since each element in a binary tree can have only 2 children, we typically name them the left and right child.

======
Strict binary tree: A binary tree is called strict binary tree if each node has exactly two children or no children.
		A
	  /   \
	 B     C
	/ \
   D   E

======
Full binary tree: A binary tree is called full binary tree if each node has exactly two children and all leaf nodes are at same level.
		A
	  /   \
	 B     C
	/ \   / \
   D   E F   G

======
Complete binary tree: A Binary Tree is complete Binary Tree if all levels are completely filled except possibly the last level and the last level has all keys as left as possible.
          18
       /      \  
     15        30  
    /  \       /  \
  40    50   100   40
 /  \   /
8   7  9 

The following is also a Complete Binary Tree:

          18
       /     \   
     15       30    
    /  \     /   \   
  40    50  100  40 

======
ADOBE: Number of nodes n in a full binary tree is 2^(h+1) -1. There are h levels we need to add all the nodes at each level[2^0 + 2^1 + ... 2^h] = 2^(h+1) -1.

======
The number of nodes n in a complete binary tree is between 2^h(minimum) and 2^(h+1) - 1(maximum).

======
Number of leaf nodes in full binary tree is 2^h

======
Number of NULL links in a complete binary tree of n nodes is n+1

======
Usage of binary tree: Expression trees are used in compilers. Huffman coding trees are used in data compression algorithm. BST which supports search, insertion, deletion on a collection of items in O(logn). Priority queues which supports search and deletion of minimum on a collection of items in logarithmic time.

======
APPLIED MATERIALS
Traversing a binary tree: 
Left subtree is always visited before right subtree.
		1
	2		3
 4    5	  6    7
Preorder: <root><left><right>: 1 2 4 5 3 6 7
InOrder: <left><root><right>: 4 2 5 1 6 3 7
PostOrder: <left><right><root>: 4 5 2 6 7 3 1
PostOrder, InOrder and PostOrder are using Stack and also a DFS (Depth first search)
Level Order Traversing: It uses Queues and also a BFS (Bread first search)

======
Non recursive pre-order traversal
public void nonRecursivePreOrder(Node node) {
	if (node == null) {
		return;
	}
	Stack<Node> stack = new Stack<>();
    while (true) {
        while (node != null) {
            stack.push(node);
            System.out.print(node.getData() + " ");
            node = node.getLeftNode();
        }
        if (stack.empty()) {
            break;
        } else {
            node = stack.pop();
            node = node.getRightNode();
        }
    }
}

======
ADOBE
Non recursive level order traversal
public void nonRecursiveLeverOrderTraversal(Node node) {

	if (node == null) {
		return;
	}
	Queue queue = new Queue();
	queue.enqueue(node);

	while (!queue.empty()) {
		node = (Node) queue.dequeue();
		System.out.println(node.getData());

		if (node.getLeftNode() != null) {
			queue.enqueue(node.getLeftNode());
		}

		if (node.getRightNode() != null) {
			queue.enqueue(node.getRightNode());
		}
	}
}

=======
ADOBE
Non recursive inorder traversal
Stack<Node> stack = new Stack<>();
while (true) {
    while (node != null) {
        stack.push(node);
        node = node.getLeftNode();
    }
    if (stack.empty()) {
        break;
    } else {
        node = stack.pop();
        System.out.print(node.getData() + " ");
        node = node.getRightNode();
    }
}

=======
Non recursive postorder traversal: In preorder and inorder traversal after popping the stack element we do not need to visit the same vertex again. But in postorder traversal, each node is visited twice. That means after processing left subtree we will visit the current node and after processing the right subtree we will visit the same current node. But we should be processing the node during the second visit. So, problem is to differentiate whether we are returning from left subtree or right subtree. Trick to solving this problem is that after popping an element from stack, check whether that element and right of the stack are same or not. If they are same then we have completed the process or left subtree and right subtree. In this case we just need to pop the stack one more time and print the data.

Stack stack = new Stack();
while(true){
	if(root != null){
		stack.push(root);
		root = root.left();
	}else{
		if(stack.empty()){
			System.out.println("Stack is empty");
			return;
		}else{
			if(stack.peek().right() == null){
				root = stack.pop();
				System.out.println(root.getData());
				while(root == stack.top.right()){
					System.out.println(stack.top.getData());
					root = stack.pop();
				}
			}
			if(!stack.empty()){
				root = stack.top.right();
			}else{
				root = null;
			}
		}
	}
}
https://www.youtube.com/watch?v=cviyUv2TFG8

=======
Vertical Order Traversal
We follow HD(Horizontal Distance) as 0 for root by default
for left child HD(parent)-1 and HD(parent)+1
-3 -2 -1 0 1 2 3
 h  d  b a k g m
	   i e l
	   j f c
	   
		a
	b		c
  d   e   f   g
 h i j k     l m
Algo: Enqueue the root in a queue. Update HD as 0. Dequeue and check left and right child and the HD as key and value as node value. Keep repeating.

=======
Get height of BST Recursively: The height of a binary tree is the largest number of edges in a path from the root node to a leaf node. 
public int getHeightRecursively(Node node) {
	if (node != null) {
		left = getHeightRecursively(node.getLeftNode());
		right = getHeightRecursively(node.getRightNode());

		if (left > right) {
			return left+1;
		} else {
			return right+1;
		}
	}else {
		return 0;
	}
}

========
ADOBE
Get height of BST non-recursively
public int getHeight(Node node) {
	int height = 0;
	if (node != null) {
		Queue queue = new Queue();
		queue.enqueue(node);
		queue.enqueue(null);
		while (!queue.empty()) {
			node = (Node) queue.dequeue();
			if (node == null) {
				if (!queue.empty()) {
					queue.enqueue(null);
				}
				height++;
			} else {
				if (node.getLeftNode() != null) {
					queue.enqueue(node.getLeftNode());
				}
				if (node.getRightNode() != null) {
					queue.enqueue(node.getRightNode());
				}
			}
		}
	}
	return height;
}
We can remove null from the queue, we can use the queue size in a variable whenever there is a level change and keep decrementing it. Whenever we reach the value 0 we check the queue and put the size of the queue in that variable and increment the level.

=======
Diameter: It is called width of the tree. Number of nodes in the longest path between two leaves of tree. Find the diameter of left subtree and right subtree + 1. We need to find it recursively. But the diameter may not pass through root. Finally: max(leftDiameter + rightDiameter + 1, max(leftDiameter, rightDiameter)). If the diameter passes through root node then :leftDiameter + rightDiameter + 1. If it does not passes then try to find the diameter of the leftSubtree and diameter from rightSubtree. Get the maximum value out of these and then again take the maximum value from diameter passing from root node.
int DiameterOfTree(Node node){
	if(node == null){
		return 0;
	}
	int leftHeight = HeightOfTree(node.leftNode());
	int rightHeight = HeightOfTree(node.rightNode());
	int lDiameter = DiameterOfTree(node.leftNode());
	int rDiameter = DiameterOfTree(node.rightNode());
	return max(leftHeight + leftHeight + 1, max(lDiameter, rDiameter));
}
https://www.youtube.com/watch?v=ey7DYc9OANo

=======
Size of binary tree recursively: Get the number of nodes of the binary tree to get the size
public int sizeOfBinaryTreeRecursively(Node node) {
	int size = 0;
	if (node != null) {
		size++;
		size = size + sizeOfBinaryTree(node.getLeftNode());
		size = size + sizeOfBinaryTree(node.getRightNode());
	}
	return size;
}

=======
Size of binary tree non-recursively
public int sizeOfBinaryTree(Node node) {
	int size = 0;
	Queue queue = new Queue();
	queue.enqueue(node);
	while (!queue.empty()) {
		size++;
		node = (Node) queue.dequeue();
		if (node.getLeftNode() != null) {
			queue.enqueue(node.getLeftNode());
		}
		if (node.getRightNode() != null) {
			queue.enqueue(node.getRightNode());
		}
	}
	return size;
}

========
Print all the paths from root to leaf nodes in a binary tree: The idea is to traverse the tree in preorder fashion and store every encountered node in the current path from root to leaf in a vector. Same can be done using the stack. Same can be done using the array while passing the length of the array used to print the nodes.

========
Find the path which has a sum input by user: We use inorder traversal and a stack. We need to keep track of the path using stack and also the sum. Push the root into the stack and check for left child. Push left child and check the sum. If not matching, then check the left child of this node and put that into the stack. Check sum. Keep doing, when left child is over go to right child and keep checking the sum (either by subtracting or by summing all the values of the nodes).

public boolean pathHasAsASum(Node node, int sum) {

	if (node == null) {
		return sum == 0;
	} else {
		return (pathHasAsASum(node.getLeftNode(), sum - node.getData()) || pathHasAsASum(node.getRightNode(), sum - node.getData()));
	}
}

======
Print all the ancestors of Node
public boolean printAllAncestorsOfNode(Node node, int value) {
	if (node != null) {
		if ((node.getLeftNode() != null && value == node.getLeftNode().getData())
				|| (node.getRightNode() != null && value == node.getRightNode().getData())
				|| printAllAncestorsOfNode(node.getLeftNode(), value)
				|| printAllAncestorsOfNode(node.getRightNode(), value)) {
			System.out.println(node.getData());
			return true;
		}
	}
	return false;
}
- We can use stack also to traverse the array using inorder traversal non-recursively. And whenever the root is found we can break the loop and print all the node values present in the stack.
- We can use array initialized and passing on the length of array along with.

======
Find least common ancestor
public Node leastCommonAncestor(Node node, int value1, int value2) {
	Node leftNode, rightNode;
	if (node == null) {
		return node;
	}
	// check if any of the node is found, if yes return that node 
	if (node.getData() == value1 || node.getData() == value2) {
		return node;
	}
	// find recursively if left node or right node is found which is not null
	leftNode = leastCommonAncestor(node.getLeftNode(), value1, value2);
	rightNode = leastCommonAncestor(node.getRightNode(), value1, value2);

	// if both found to be not null means this parent node is a common ancestor as it has
	fount the nodes both on the left and the right node 
	if (leftNode != null && rightNode != null) {
		return node;
	}

	// if the parent node has found both null on both side left and right as null then return null
	if(leftNode == null && rightNode == null){
		return null;
	}
	
	// if the parent node has found any one of the node on his left or right just return 
	return leftNode!= null? leftNode : rightNode
}

======
Binary Search tree: A Binary Search Tree (BST) is a tree in which all the nodes follow the below-mentioned properties The left sub-tree of a node has a key less than or equal to its parent node's key. The right sub-tree of a node has a key greater than to its parent node's key.

======
Delete a node in BST
1) If node is leaf node: Remove the leaf node, nothing to do much
2) If node is having single child: just replace the node with its child
3) If node is having two child: just replace it with the minimum value node from right subtree. Why? In order traversal we get to know the value which is to be replaced must be less than all the values from right subtree. So we take minimum value from right subtree. This is in order successor
  
=======
Convert Binary Tree to its mirror image:  The basic behind this is for every node you have to swap its left and right child. We can use recursive functions. For left, For right, Swap for node

=======
Check two trees are isomorphic: Two trees are isomorphic if their children are isomorphic. When right child of tree 1 is same as left child of tree 2 then isomorphic too.
1) If any of the tree node is null but not both : return false
2) If both the tree nodes are null : return true
3) If value of that both the nodes is same or not
4) Recursively then call function for nodes
(isIsomorphic(left of t1 , left of t2) && isIsomorphic(right of t1 , right of t2)) || (isIsomorphic(left of t1 , right of t2) && isIsomorphic(right of t1 , left of t2))

========
Print nodes at 'k' distance from Root: Mark the root element as k and then start decreasing the k value when it reaches it child root child has values k-1. Till the value reaches 0. Print that node. Recursion will be used here. 

========
Print all the diagonal elements in BST
Rules: d is diagonal distance. For every left child you will increment the d by 1 i.e. d+1. For every right child, d remains same as of parent. The same values of for the node will form the diagonal. Code: Traverse the right child series, and if it has left child enqueue that child. Enqueue root child and add null. Then enqueue the left child of the node and assign the pointer the right child. Keep printing the right child.
Queue queue = new Queue();
queue.enqueue(root);
queue.enqueue(null);
while(!queue.isEmpty){
	p = queue.dequeue();
	if(p == null){
		queue.enqueue(null);
		p = queue.dequeue();
		if(p == null){
			break;
		}
	}
	while(p != null){
		print(p);
		if(p.left != null){
			queue.enqueue(p.left);
		}
		p = p.right;
	}
}
https://www.youtube.com/watch?v=e9ZGxH1y_PE

========
Vertical order traversal: We will be using the queue and HashMap. Enqueue root. Update the HD for root as 0. Add HD=0 in hashTable and root as the value. Dequeue and check for left and right value and update the HD in the hash table. We can add the 1 value for the right child and subtract 1 value in the left child. We got to know the parent HD value during the dequeue. Enqueue left child and right child in the queue. In this we are using queue which act as the level order traversing.

========
Print the top view in BST
			a
		 /      \
		b        c
	  /   \    /   \
	 d     e   f    g
    /          /\
   l          h  i
				   \
					j
					 \
					   k
Top view : ldbacgk. We use (Level order traversal + Vertical order traversal). Level order traversal: abcdefglhijk : For this we use hd (horizontal distance). hd for left is hd-1 and for right child it is hd+1. Vertical order traversal: 
-3 -2 -1 0 1 2 3 
 l  d  b a c g k
       h e i j
	     f
The top view will be vertical order traversal: l d b a c g k but when to select out of b and h, we consult the level order traversing, b comes before h so b will be in the top view
https://www.youtube.com/watch?v=c3SAvcjWb1E
		 
=========
Side view of the tree in BST
		      a
		 /        \
		b          c
	  /  \       /   \
	 d    e      f    g
    / \             /   \
   h   i           l     m
	  /	\	  	  /     / \
	 j   k		 n	   q   r
				/	   /     
			   p	  s
					   \
					    t
Left view:  abdhjpt : Level order traversal from the front
Right view: acgmrst : Level order traversal from the back
											
==========
Bottom view of the tree in BST: We are gonna use vertical order traversal. Vertical order traversal can be measured using horizontal distance. We write those hd distances in a hash table. For writing the hd distances in the hash table we use level order traversal.
-3 -2 -1 0 1 2 3 
 h  d  b a c g k
  	   i e j
	     f
Take the behind ones from each column. h d i f j g k will be the bottom view of the BST
https://www.youtube.com/watch?v=V7alrvgS5AI

==========
Boundary traversal of BST: First you have to print the left boundary nodes. Check if the left child is present, if not check right child. But do not print the left node. Then go to right boundary nodes. Check left and right child of the node. It is a recursive loop. Then print the leaf boundary: Check the left and right child are both null.

==========
Print nodes having 'k' leaves: Post order traversal and leaf counter. Check if the node is leaf node or not. If yes return 1. leftLeafNodes = nodesAtLeftSubtree(node.left), righLeafNodes = nodesAtRightSubtree(node.right), total leafs = leftLeafNodes + righLeafNodes and return this value. We also check if the returned value is equal to k.

==========
Zig Zag Spiral traversal in BST: Create two stack and do level order traversal. Put the first level in the stack and second level in the second stack. Then again put the second stack in the first stack which will reverse it and keep doing it.

==========
Inorder successor in Binary Search tree.
- We can traverse the whole tree using inorder traversal and then can find the successor.
- Case 1: If the node has the right child then find the least value from the right subtree i.e. the leftmost node from the right subtree.
- Case 2: If the node does not have right subtree than search this node from root. The node from where we take the last left is the answer.

==========
Inorder predecessor in Binary Search tree.
- We can traverse the whole tree using inorder traversal and then can find the successor.
- Case 1: If the node has left subtree, then jump to left child and get the rightmost node (max in the left subtree)
- Case 2: If the node doesn't have the left subtree then search this node from root. The node from where we take the last right is the answer.
https://www.youtube.com/watch?v=rukYFD8cYBY

========
How to check if a given Binary Tree is BST or not: If inorder traversal of a binary tree is sorted, then the binary tree is BST. The idea is to simply do inorder traversal and while traversing keep track of previous key value. If current key value is greater, then continue, else return false. 

========
Number of Binary Search trees possible with 'n' distinct keys
1 key  = 1 tree can be formed
2 keys = 2 trees
3 keys = 5 trees
4 keys = 14 trees
N[0] = N[1] = 1
for(int i=2; i<=6; i++){
	N[i] =0;
	
	for(j=0; j<i ; j++){
		N[i] = N[j]+ N[i-j-1] 
	}
}

========
Generic Trees (k-ary trees): A tree having k children. How you are gonna construct. At each node link the children of same parent from left to right. Remove the links from parent to all children except the first child. Basically node has another pointer nextSibling which will point to the next child of its parent node. Apart from sibling node there are two value one is the element and another is pointer to its child node. Basically you are using a list for maintaining the siblings of parent.

========
Issues with binary tree traversal
- storage space required for the stack and queue is large.
- majority of pointers in binary tree are null. Ex. binary tree with n nodes has n null pointers and these are wasted
- it is difficult to find successor node

========
Threaded binary tree: is used to store some useful information in null pointers. We have used stacks initially to move from left subtree to right subtree. If we store useful information in null pointers, then we don't have to store such information in stack/queue. Binary trees which store such information in NULL pointers are called threaded binary tree. The common convention is to put predecessor/successor information. The special pointer are called threads. The right thread of rightmost node will point to itself. The left thread of leftmost node will point to itself. Ex. We can have inorder threaded tree the left thread will point to inorder predecessor and right thread will point to inorder successor.

========
In threaded binary tree we don't need to use the stack or queue for traversing. It can be done using the successor from every node. 
http://freeusermanuals.com/backend/web/manuals/152110137817-Threaded-Binary-trees.pdf

========
Convert the BST to threaded binary tree: It mainly says that the leaf nodes or nodes having no child pointer values for left child and right child should have something useful to make use of wasted memory.
Steps:
1) Keep the left most and right most NULL pointers as null in the BST
2) Change all other null pointers as
 - The left null pointer will point to inorder predecessor
 - The right null pointer will point to inorder successor
Now you will not know whether the left pointer is pointing to child node or predecessor. So node structure is changed. In this, two flags are added left_flag and right_flag which will help to tell us that the pointer is pointing to its child node or not. Now for leftmost node where we put 0 for the left_flag, means it is not a child. it will point to predecessor but the value at that pointer is null. So, we create a dummy node to which this left node will point to and same for the right node. The left pointer of dummy null node will point to root node and right pointer will point to itself. Why we doing this, just for the consistency.

======
Expression trees: A tree representing an expression is called expression tree. Expression tree is a binary tree in which each internal node corresponds to operator and each leaf node corresponds to operand so for example expression tree for 3 + ((5+9)*2) would be:
	+
   / \
  3   *
  	  /\
  	 +	2
  	/\
   5  9
It is easy to make expression tree from postfix expression using stack. ABC*+D/

======
XOR trees are similar to memory efficient doubly linked lists. Like threaded binary trees this representation does not need stacks or queues for traversals. This tree is used for traversing back to parent and forth to children using XOR operations. Each nodes left will have the XOR of its parent and its left children. Each nodes right will have the XOR of its parent and its right children. The root nodes parent is NULL and also leaf nodes children are NULL nodes.

======
Deleting the node from Binary search tree
1) If element is to be deleted is a leaf node. No need to do anything. Just replace it with null
2) If the element is to be deleted has one child. In this case we just need to send the current nodes child to its parent. 
3) If the element is to be deleted has both children. Strategy is to replace the key of this node with the largest element of the left subtree and recursively delete that node.

=======
Checking if given tree is BST or not: We can check if the current value of node is less/greater than the parent node. But this doesn't give guarantee that it is BST. So, in place we also find the max from left subtree and min from right subtree. If the given node is greater than max value and less than min value it is BST. Do this recursively for each node.
		6
	2		8
  1   9

=======
Trees whose have worst case complexity i.e. O(n) happens to be of skew trees. In general height balanced trees are represented with HB(k), where k is the difference between left subtree and right subtree height. k is called the balanced factor.

=========
In HB(k), if k=0, then we call this tree as full balanced binary search tree.
Balance factor of a BST = Height of left subtree - Height of right subtree.

=======================================
AVL (Andelson-Velskii and landis) Trees 
=======================================
(Self balancing trees): In HB(k), if k = 1, such binary tree is called an AVL tree. To get minimum number of nodes with height h, we should fill the tree with as minimum nodes as possible. If we fill left subtree with height h-1 and right subtree with height h-2. Then minimum number of nodes with height h is = N(h-1) + N(h-2) + 1. 
Similarly for getting the maximum nodes fill right and left subtree both with nodes having n-1 height i.e. = 2^h = h = logn. 
Height balance tree is the tree if the height of the subtrees differ by no more than 1. An empty tree is height balanced by default. If every node has balance factor of 0 or 1 or -1, then it is Balanced Binary Tree. If every node has balance factor of 0 then it is Perfectly Balanced Tree.

========
Operations on AVL Tree: Rotations LL(Left Left Rotation), RR, LR, RL
LL(Left Left Rotation) : we can make the middle node as the main node and parent will become the child
	   c      			b
      /				   /  \
     b				  a     c
    /  skewed tree
   a
RR(Right Right Rotation) : we can make the middle node as the main node and parent will become the child
	c      						   a
     \						     /  \
      a				  			c     b
       \  skewed tree
        b
		
LR(Left Right Rotation) : we can make the middle node as the main node and parent will become the child
	  4     					  3
     /						     / \
    2				  			2   4
     \  skewed tree
      3

RL(Right Left Rotation) : we can make the middle node as the main node and parent will become the child
	  2     					      3
       \						     / \
        4				  			2   4
       /  skewed tree 
       3
Just assume there is a hypothetically imaginary node attached to 3 which will make it LL and apply LL rotation. Then remove imaginary node and then use RR which will form this.
https://www.youtube.com/watch?v=_c9MekIdl98

===============
Red-Black tree 
===============
Each node is associated with extra attributes: the color, which is either red or black. Red-Black Tree is a self-balancing Binary Search Tree (BST) where every node follows following rules.
1) Every node has a color either red or black.
2) Root of tree is always black.
3) There are no two adjacent red nodes (A red node cannot have a red parent or red child).
4) Every path from a node (including root) to any of its descendant NULL node has the same number of black nodes.
It is also the Self balancing tree

=======
Why Red-Black Trees: Most of the BST operations (e.g., search, max, min, insert, delete.. etc) take O(h) time where h is the height of the BST. The cost of these operations may become O(n) for a skewed Binary tree. If we make sure that height of the tree remains O(Logn) after every insertion and deletion, then we can guarantee an upper bound of O(Logn) for all these operations. The height of a Red-Black tree is always O(Logn) where n is the number of nodes in the tree.

=======
Insertion Red-Black tree
- If empty tree create black root node
- Insert new leaf as red. If its parent is black than done. If parent is red then we have to do either
i) We need to check the sibling, if red sibling then recolor and check again the conditions (change the color of this node and sibling to Black and parent node as Red and check again)
ii) If the sibling is black then we need to have the rotations(right rotation, left->right rotation, left rotation, right->left rotation)
https://www.youtube.com/watch?v=UaLIHuR1t8Q

=======
Comparison with AVL Tree: The AVL trees are more balanced compared to Red-Black Trees, but they may cause more rotations during insertion and deletion. So if your application involves many frequent insertions and deletions, then Red Black trees should be preferred. And if the insertions and deletions are less frequent and search is a more frequent operation, then AVL tree should be preferred over Red-Black Tree. AVL trees provide faster lookups than Red Black Trees because they are more strictly balanced. Red Black Trees provide faster insertion and removal operations than AVL trees as fewer rotations are done due to relatively relaxed balancing. AVL trees store balance factors or heights with each node, thus requires storage for an integer per node whereas Red Black Tree requires only 1 bit of information per node. Red Black Trees are used in most of the language libraries like map, multimap, multiset in C++ whereas AVL trees are used in databases where faster retrievals are required.

=======
Black height is number of black nodes on a path from a node to a leaf. Leaf nodes are also counted black nodes. A node of height h has black-height >= h/2.

=======
Red-Black tree which is not an AVL tree
			  Black
			   / \
			Red  Black
			/ \
		Black Black
		  /
		Red
Above tree is an Red-Black tree but not an AVL tree as the height difference is more than 1.

=======
Red Black Tree: TreeMap and SortedMap in java

=======
Insertion in RED Black Tree:
1) If empty tree, create black root node.
2) Insert new leaf node as red:
	- If parent is black then done
	- If parent is red
		* If black or no sibling then rotate, recolor and check again (rotations same as AVL)
		* If the sibling is red then recolor and recheck
https://www.youtube.com/watch?v=UaLIHuR1t8Q

=======
Priority Queue: A PriorityQueue is used when the objects are supposed to be processed based on the priority. It is known that a queue follows First-In-First-Out algorithm, but sometimes the elements of the queue are needed to be processed according to the priority, that’s when the PriorityQueue comes into play. The elements of the priority queue are ordered according to the natural ordering, or by a Comparator provided at queue construction time, depending on which constructor is used. DeleteMin and DeleteMax are the operations.
Few important points on Priority Queue are as follows:
- PriorityQueue doesn’t permit NULL pointers.
- We can’t create PriorityQueue of Objects that are non-comparable
- PriorityQueue are unbound queues.
- The head of this queue is the least element with respect to the specified ordering. If multiple elements are tied for least value, the head is one of those elements — ties are broken arbitrarily.
- The queue retrieval operations poll, remove, peek, and element access the element at the head of the queue.
- It inherits methods from AbstractQueue, AbstractCollection, Collection and Object class.
- Used for data compression: Huffman coding algorithm, shortest path algo Dijkstra's Algo, Minimum spanning tree, Finding kth smallest element.
- Best way to implement priority queue is binary heap. It gives O(logn) for insertion and deletion and min max in O(1)

=======
Heap: is a tree with some special properties. Basic requirement of heap is that value of a node must be greater or lesser than its children. This is called heap property. Heap also has the additional property that all leaves should be at h or h-1 level. Means it should form complete binary tree. These are two types: Min heap and Max heap.

=======
Binary heap: In binary heap each node may have up to two children. 

=======
Min Binary Heap: The values of the binary heap are stored in array. We use array to represent the heap. And we use indices to represent parent child structure. The values are put in the array like level order traversing. Indexes representing the position of the node in heap tree can be found using below formula
Left child: (2*i + 1), Right child: (2*i + 2), Parent: (i-1)/2

Operations on Min Heap:
1) getMini(): It returns the root element of Min Heap. Time Complexity of this operation is O(1).
2) extractMin(): Removes the minimum element from MinHeap. Time Complexity of this Operation is O(Logn) as this operation needs to maintain the heap property (by calling heapify()) after removing root.
3) decreaseKey(): Decreases value of key. The time complexity of this operation is O(Logn). If the decreases key value of a node is greater than the parent of the node, then we don’t need to do anything. Otherwise, we need to traverse up to fix the violated heap property.
4) insert(): Inserting a new key takes O(Logn) time. We add a new key at the end of the tree. IF new key is greater than its parent, then we don’t need to do anything. Otherwise, we need to traverse up to fix the violated heap property.
5) delete(): Deleting a key also takes O(Logn) time. We replace the key to be deleted with Minimum infinite by calling decreaseKey(). After decreaseKey(), the minus infinite value must reach root, so we call extractMin() to remove the key.

=======
Heapify: While inserting the element in heap, it may not fulfill the heap properties. In that case we need to adjust the locations of the heap to make it heap again. This process is called heapifying. Starting from the second last level of the tree, keep checking if the parent node is the minimum of the children of not. If not, replace it with the minimum. and keep doing it at all the levels. We have to keep doing this till the tree is heapify. Each insert in the binary heap will need O(logn). So if we insert n items then complexity will be O(nLogn). Time complexity of heap sort algorithm is O(nlogn). Height of a heap with n elements is logn.

public class MinHeap { 
    private int[] Heap; 
    private int size; 
    private int maxsize; 
    private static final int FRONT = 1; 
    public MinHeap(int maxsize) { 
        this.maxsize = maxsize; 
        this.size = 0; 
        Heap = new int[this.maxsize + 1]; 
        Heap[0] = Integer.MIN_VALUE; 
    } 
  
    // Function to return the position of the parent for the node currently at pos 
    private int parent(int pos){ 
        return pos / 2; 
    } 
    // Function to return the position of the left child for the node currently at pos 
    private int leftChild(int pos){ 
        return (2 * pos); 
    } 
    // Function to return the position of the right child for the node currently at pos 
    private int rightChild(int pos) { 
        return (2 * pos) + 1; 
    } 
    // Function that returns true if the passed node is a leaf node 
    private boolean isLeaf(int pos) { 
        if (pos >= (size / 2) && pos <= size) { 
            return true; 
        } 
        return false; 
    } 
    // Function to swap two nodes of the heap 
    private void swap(int fpos, int spos) { 
        int tmp; 
        tmp = Heap[fpos]; 
        Heap[fpos] = Heap[spos]; 
        Heap[spos] = tmp; 
    } 
    // Function to heapify the node at pos 
    private void minHeapify(int pos) { 
        // If the node is a non-leaf node and greater than any of its child 
        if (!isLeaf(pos)) { 
            if (Heap[pos] > Heap[leftChild(pos)] 
                || Heap[pos] > Heap[rightChild(pos)]) { 
  
                // Swap with the left child and heapify 
                // the left child 
                if (Heap[leftChild(pos)] < Heap[rightChild(pos)]) { 
                    swap(pos, leftChild(pos)); 
                    minHeapify(leftChild(pos)); 
                } 
  
                // Swap with the right child and heapify 
                // the right child 
                else { 
                    swap(pos, rightChild(pos)); 
                    minHeapify(rightChild(pos)); 
                } 
            } 
        } 
    } 
    // Function to insert a node into the heap 
    public void insert(int element) { 
        if (size >= maxsize) { 
            return; 
        } 
        Heap[++size] = element; 
        int current = size; 
        while (Heap[current] < Heap[parent(current)]) { 
            swap(current, parent(current)); 
            current = parent(current); 
        } 
    } 
    // Function to print the contents of the heap 
    public void print() { 
        for (int i = 1; i <= size / 2; i++) { 
            System.out.print(" PARENT : " + Heap[i] 
                             + " LEFT CHILD : " + Heap[2 * i] 
                             + " RIGHT CHILD :" + Heap[2 * i + 1]); 
            System.out.println(); 
        } 
    } 
    // Function to build the min heap using the minHeapify 
    public void minHeap() { 
        for (int pos = (size / 2); pos >= 1; pos--) { 
            minHeapify(pos); 
        } 
    } 
    // Function to remove and return the minimum element from the heap 
    public int remove() { 
        int popped = Heap[FRONT]; 
        Heap[FRONT] = Heap[size--]; 
        minHeapify(FRONT); 
        return popped; 
    } 
}
=========
How to implement PriorityQueue?
- Using arrays: getHighestPriority() operation can be implemented by linearly searching the highest priority item in array. This operation takes O(n) time. deleteHighestPriority() operation can be implemented by first linearly searching an item, then removing the item by moving all subsequent items one position back.
- Using Linked list: Time complexity of all operations with linked list remains same as array. The advantage with linked list is deleteHighestPriority() can be more efficient as we don’t have to move items.
- Using Heap: Heap is generally preferred for priority queue implementation because heaps provide better performance compared arrays or linked list. In a Binary Heap, getHighestPriority() can be implemented in O(1) time, insert() can be implemented in O(Logn) time and deleteHighestPriority() can also be implemented in O(Logn) time.

=========	
Heap sort: After heapyfying the tree we will get minimum number at the top that will be root node. Replace the root node with the last element of the level and heapify the tree again. It will give another minimum value at the top. Keep doing it and you will get the answer.

***=====***
    TRIE
***=====***
Trie is an ordered tree data structure that uses strings as keys. Unlike Binary Trees, Tries do not store keys associated with the node. The key is actually determined based on the position of the node on the tree. Any descendants of a node shares a common prefix of the key string associated with that node. Hence, trie is also called as Prefix Tree. The word "trie" comes from Retrieval, and it is pronounced as "try". The root is associated with an empty String. By structuring the nodes in a particular way, words and strings can be retrieved from the structure by traversing down a branch path of the tree

=====
If 2 strings have common prefix then they will have a same ancestor in this tree. If you have thousands or hundred thousand strings in the collection, then it would be easy to find a string in this data structure. It is a ideal data structure for storing a dictionary. Can also be used for prefix based search. Comparing to hashTable, you cannot do prefix based searching and also hashTable takes lot of space compared to trie.

=====
Trie is composed of TrieNode which is having the two main components. Map and Boolean.
public class TrieNode {
    private HashMap<Character, TrieNode> children;
    private boolean isWord;
}
// if there are characters of only uppercase and lowercase then we can replace the hashMap with array of 26 characters but for generalization purpose we are using hashMap. Every node (except the root node) stores one character or a digit. By traversing the trie down from the root node to a particular node n, a common prefix of characters or digits can be formed which is shared by other branches of the trie as well. By traversing up the trie from a leaf node to the root node, a String or a sequence of digits can be formed.

=====
class TrieNode {
    char c;
    HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
    boolean isLeaf;
    public TrieNode() {}
    public TrieNode(char c){
        this.c = c;
    }
}
public class Trie {
    private TrieNode root;
 
    public Trie() {
        root = new TrieNode();
    }
 
    // Inserts a word into the trie.
    public void insert(String word) {
        HashMap<Character, TrieNode> children = root.children;
 
        for(int i=0; i<word.length(); i++){
            char c = word.charAt(i);
 
            TrieNode t;
            if(children.containsKey(c)){
                    t = children.get(c);
            }else{
                t = new TrieNode(c);
                children.put(c, t);
            }
 
            children = t.children;
 
            //set leaf node
            if(i==word.length()-1)
                t.isLeaf = true;    
        }
    }
 
    // Returns if the word is in the trie.
    public boolean search(String word) {
        TrieNode t = searchNode(word);
 
        if(t != null && t.isLeaf) 
            return true;
        else
            return false;
    }
 
    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        if(searchNode(prefix) == null) 
            return false;
        else
            return true;
    }
 
    public TrieNode searchNode(String str){
        Map<Character, TrieNode> children = root.children; 
        TrieNode t = null;
        for(int i=0; i<str.length(); i++){
            char c = str.charAt(i);
            if(children.containsKey(c)){
                t = children.get(c);
                children = t.children;
            }else{
                return null;
            }
        }
        return t;
    }
}
https://www.programcreek.com/2014/05/leetcode-implement-trie-prefix-tree-java/

=====
It's performance can be improved using the array[26]
Each trie node can only contains 'a'-'z' characters. So we can use a small array to store the character.

class TrieNode {
    TrieNode[] arr;
    boolean isEnd;
    // Initialize your data structure here.
    public TrieNode() {
        this.arr = new TrieNode[26];
    }
 
}
 
public class Trie {
    private TrieNode root;
 
    public Trie() {
        root = new TrieNode();
    }
 
    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode p = root;
        for(int i=0; i<word.length(); i++){
            char c = word.charAt(i);
            int index = c-'a';
            if(p.arr[index]==null){
                TrieNode temp = new TrieNode();
                p.arr[index]=temp;
                p = temp;
            }else{
                p=p.arr[index];
            }
        }
        p.isEnd=true;
    }
 
    // Returns if the word is in the trie.
    public boolean search(String word) {
        TrieNode p = searchNode(word);
        if(p==null){
            return false;
        }else{
            if(p.isEnd)
                return true;
        }
 
        return false;
    }
 
    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode p = searchNode(prefix);
        if(p==null){
            return false;
        }else{
            return true;
        }
    }
 
    public TrieNode searchNode(String s){
        TrieNode p = root;
        for(int i=0; i<s.length(); i++){
            char c= s.charAt(i);
            int index = c-'a';
            if(p.arr[index]!=null){
                p = p.arr[index];
            }else{
                return null;
            }
        }
 
        if(p==root)
            return null;
        return p;
    }
}

=====
There are two kind of searches i.e. prefix based(when we start typing) or whole string based search.
https://www.youtube.com/watch?v=AXjmTQ8LEoI
