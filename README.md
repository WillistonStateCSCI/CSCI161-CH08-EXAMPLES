# CSCI161-CH08-EXAMPLES
Examples from Data Structures and Algorithms in Java


### EX08_01 — Numbering Note

The reference `0.1` could not be verified in the sixth-edition textbook. It is retained here rather than assigned an unsupported description.

The `Tree<E>` interface specifies operations for accessing the root, parent, and children; identifying internal nodes, leaves, and the root; checking size and emptiness; and iterating over elements or positions. This interface supplies the contract used by the remaining tree examples.

### EX08_02 — AbstractTree

`AbstractTree<E>` provides an initial partial implementation of `Tree<E>`. It defines four queries in terms of other tree operations:

- `isInternal(p)` checks whether a position has children.
- `isExternal(p)` checks whether a position is a leaf.
- `isRoot(p)` compares a position with the root.
- `isEmpty()` checks whether the size is zero.

This example illustrates code reuse through an abstract class. The queries do not depend on how nodes are stored, so concrete tree implementations can inherit them. Additional functionality is introduced later in the chapter; Fragment 8.2 alone is not a complete concrete tree implementation.

### EX08_06 — BinaryTree Interface

`BinaryTree<E>` extends `Tree<E>` with `left(p)`, `right(p)`, and `sibling(p)`. These operations distinguish the two possible child locations and provide access to the other child of a position's parent.

The interface describes navigation, not storage or insertion. A binary tree allows at most two children per node; it does not, by itself, impose the ordering rules of a binary search tree.

### EX08_07 — AbstractBinaryTree

`AbstractBinaryTree<E>` extends `AbstractTree<E>` and implements `BinaryTree<E>`. It derives three operations from parent, left-child, and right-child access:

- `sibling(p)` returns the other child of the parent, or `null` when no sibling exists.
- `numChildren(p)` counts the existing left and right children.
- `children(p)` returns a snapshot of child positions, with the left child before the right child.

This layer allows a concrete binary tree to inherit common behavior while implementing its own underlying representation.

### EX08_08 — Linked Nodes and Tree Initialization

The first portion of `LinkedBinaryTree<E>` defines a nested `Node<E>` class implementing `Position<E>`. Each node stores an element and references to its parent, left child, and right child.

It also introduces the `createNode(...)` factory method, the root reference, the size field, and the constructor for an initially empty tree. The factory method provides a customization point for subclasses that need specialized node types.

### EX08_09 — Validation and Accessors

This portion defines `validate(p)` and the accessors `size()`, `root()`, `parent(p)`, `left(p)`, and `right(p)`.

Validation checks that a position has the expected node type and has not been marked as removed. A removed node is identified by a parent reference pointing to itself. This check should not be confused with a comprehensive check that a node belongs to a particular tree instance.

The accessors expose the tree's relationships through positions while keeping the linked representation inside the implementation.

### EX08_10 — Adding Nodes and Replacing Elements

This portion supplies `addRoot(e)`, `addLeft(p, e)`, `addRight(p, e)`, and `set(p, e)`.

Insertion methods enforce structural constraints: a root can be added only to an empty tree, and a child can be added only when its corresponding child location is vacant. New nodes are created through `createNode(...)`, linked to their parent, and included in the size count.

`set(p, e)` replaces the element stored at a position and returns the previous element without changing the tree's structure.

### EX08_11 — Attaching Subtrees and Removing Nodes

`attach(p, t1, t2)` attaches two trees as the left and right subtrees of a leaf. It transfers their nodes rather than copying them, updates parent links and the receiving tree's size, and leaves the donor trees empty.

`remove(p)` removes a node with zero or one child and returns its element. If a child exists, that child takes the removed node's place. A node with two children cannot be removed by this operation.

The removed node's references are cleared, and its parent reference is set to itself so later validation can recognize it as defunct.

Fragments 8.8–8.11 are consecutive portions of one class, not four independent programs.

### EX08_24 — Labeled Preorder Output

`printPreorderLabeled(...)` prints each position before visiting its children. Indentation shows depth, while labels such as `1`, `2`, and `2.1` describe the path through the tree.

The method shares one `ArrayList<Integer>` across recursive calls. Each call temporarily adds a child-number entry, increments it between children, and removes it before returning. Restoring the list prevents one branch's numbering from affecting another branch.

For example, a tree might be displayed as:

```text
University
  1 Academic Affairs
    1.1 Computer Science
    1.2 Mathematics
  2 Student Services
```

To print an entire nonempty tree, begin at its root with an empty path list. The example also requires the `spaces(int)` helper used to produce indentation. Check for an empty tree before passing its root to the method.

