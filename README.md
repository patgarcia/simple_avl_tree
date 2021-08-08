# simple_avl_tree
First revision from conceptual description. Handles all rotation types, but not rotations with children.

```javascript
class SimpleAVLTree {
  constructor() {
    this.root = null;
  }
  add(val) {
    if (!this.root) {
      this.root = new Node(val);
    } else {
      this.root.add(val);
    }
  }
}

class Node {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
  balance() {
    // balance displace values around only,
    // create a value migration method
    // to handle more properties.
    // If we want to rewire edge pointers
    // move special case detection logic
    // to the granparent level, alternatively
    // pass the parent during "add" method.
    const nodes = [this, this.left, this.right];
    const [leftVal, parentVal, rightVal] = nodes
      .sort((a, b) => a.val - b.val)
      .map((n) => n.val);
    this.val = parentVal;
    this.left.val = leftVal;
    this.right.val = rightVal;
  }
  getSide(val) {
    return val >= this.val ? "right" : "left";
  }
  add(val) {
    //if both edges full (continue to next node)
    const side = this.getSide(val);
    const newNode = new Node(val);
    if (this.left && this.right) {
      this[side].add(val);
    }
    //if both edges empty (add node to corresponding side)
    else if (!this.left && !this.right) {
      this[side] = newNode;
    }
    //if only one edge empty (attention may be special case)
    else {
      if (!this[side]) this[side] = newNode;
      else {
        // if corresponding side taken, balance node.
        const otherSide = side === "right" ? "left" : "right";
        this[otherSide] = newNode;
        this.balance();
      }
    }
  }
}
```
