# Binary-search-tree-application-
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None


class BST:
    def __init__(self):
        self.root = None

    # Insert a key into the BST
    def insert(self, key):
        self.root = self._insert(self.root, key)

    def _insert(self, node, key):
        if not node:
            return Node(key)

        if key < node.key:
            node.left = self._insert(node.left, key)
        elif key > node.key:
            node.right = self._insert(node.right, key)

        return node

    # Inorder Traversal
    def inorder(self, node, result=None):
        if result is None:
            result = []

        if node:
            self.inorder(node.left, result)
            result.append(node.key)
            self.inorder(node.right, result)

        return result

    # Preorder Traversal
    def preorder(self, node, result=None):
        if result is None:
            result = []

        if node:
            result.append(node.key)
            self.preorder(node.left, result)
            self.preorder(node.right, result)

        return result

    # Postorder Traversal
    def postorder(self, node, result=None):
        if result is None:
            result = []

        if node:
            self.postorder(node.left, result)
            self.postorder(node.right, result)
            result.append(node.key)

        return result

    # Delete a key from the BST
    def delete(self, key):
        self.root = self._delete(self.root, key)
        print(f"Deleted {key}. Inorder: {self.inorder(self.root)}")

    def _delete(self, node, key):
        if not node:
            return node

        if key < node.key:
            node.left = self._delete(node.left, key)

        elif key > node.key:
            node.right = self._delete(node.right, key)

        else:
            # Case 1: No child or only right child
            if not node.left:
                return node.right

            # Case 2: Only left child
            elif not node.right:
                return node.left

            # Case 3: Two children
            # Find inorder successor
            succ = node.right

            while succ.left:
                succ = succ.left

            print(
                f"Replacing {node.key} "
                f"with inorder successor {succ.key}"
            )

            node.key = succ.key
            node.right = self._delete(node.right, succ.key)

        return node


# ---------------- Main Program ----------------

bst = BST()

scores = [75, 60, 90, 45, 65, 85, 95]

# Insert elements
for s in scores:
    bst.insert(s)


# Display traversals
print("BST Traversals after insertion:")
print(" Inorder (sorted):", bst.inorder(bst.root))
print(" Preorder         :", bst.preorder(bst.root))
print(" Postorder        :", bst.postorder(bst.root))


# Deletions
print("\n--- Deletions ---")

bst.delete(45)   # Leaf node
bst.delete(65)   # Leaf node
bst.delete(75)   # Root with two children