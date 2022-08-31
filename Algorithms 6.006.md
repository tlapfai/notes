Binary Search Tree


Application: Set

build in O(n log n)

find(k) O(log n)

every key in left subtree <= node’s key <= every key in right subtree

Application: Sequence

build in O(n) #用原本排序，切半 & recursion

find(k) O(log n) #用augmentation：加/減node=>all ancestor加1 (向上做)

    class Binary_Node:
        def __init__(self, x):
          self.item = x
          self.left = None
	      self.right = None
	      self.parent = None
	      self.subtree_update()

AVL Tree #把樹盡量平衡
