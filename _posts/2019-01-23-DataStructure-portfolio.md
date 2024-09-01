---
layout: post
title: Data Structure 
subtitle: To show what I did in university
excerpt_image: https://user-images.githubusercontent.com/34437660/51545796-1bb8a180-1ea6-11e9-8c52-e0f571f58f29.png
categories: "university"
tags: ["Python", "Data Structure"]
---


### Summary

I did a lot of data structure assignments which are listed below. 

The first is about calculator with two stack
The second is about tree traversal(inorder, preorder, postorder)

![4](https://user-images.githubusercontent.com/34437660/51545796-1bb8a180-1ea6-11e9-8c52-e0f571f58f29.png)



### Calculator with two stack

```
from ArrayStack import *
import operator

valStack = ArrayStack()
opStack = ArrayStack()

def opCompare(op):
    if op is "+":
        return 1
    elif op is "-":
        return 1
    elif op is "*":
        return 2
    elif op is "/":
        return 2
    elif op is "$":
        return 0

def doOp():
    ops = {"+":operator.add, "-":operator.sub, 
            "*":operator.mul, "/":operator.truediv}
    x= valStack.pop()
    y = valStack.pop()
    op = opStack.pop()
    
    valStack.push(ops[op](int(y),int(x)))
    
def repeatOps(e):
    if opStack.is_empty():
        return None
    while (valStack.size() >= 1) and (opStack.size() >=1):
        if opCompare(e) <= opCompare(opStack.top()):
            doOp()
        else:
            break

def evalExp(exp):
    operators = "+-=*/"
    for e in exp:
        if e not in operators:
            valStack.push(e)
        else:
            repeatOps(e)
            opStack.push(e)
    repeatOps('$')       
    return valStack.top()

def main():
    print(evalExp("3+4*5-9*3"))

if __name__ == "__main__":
    main()
```



### Tree traversal
```
# **traversal** 
def inorder(self):
    if self._left:
        for other in self._left.inorder():
            yield other
    yield self
    if self._right:
        for other in self._right.inorder():
            yield other

def preorder(self):
    yield self
    if self._left:
        for other in self._left.preorder():
            yield other
    if self._right:
        for other in self._right.preorder():
            yield other

def postorder(self):
    if self._left:
        for other in self._left.postorder():
            yield other
    if self._right:
        for other in self._right.postorder():
            yield other
    yield self

def levelorder(self):
    queue = []
    queue.append(self)
    while len(queue) != 0:
        p = queue.pop(0)
        if p._left is not None:
            queue.append(p._left)
        if p._right is not None:
            queue.append(p._right)
        yield p

**Pre and Next travel about each traversal**
def inorder_prev(self):
    if self.is_external():
        if self._parent._left == self:
            iter = self._parent
            while iter._parent._right != iter:
                iter = iter._parent
                if iter.is_root():
                    return None
            return iter._parent
        elif self._parent._right == self:
            return self._parent
    elif self.is_internal():
        if self._left._right == None:
            return self._left
        elif self._left.right():
            iter = self._left
            while iter._right is not None:
                iter = iter._right
            return iter

def inorder_next(self):
    if self.is_external():
        if self._parent._left == self:
            return self._parent
        elif self._parent._right == self:       
            iter = self._parent                 
            while iter._parent._left != iter:
                iter = iter._parent
                if iter.is_root():
                    return None
            return iter._parent
    elif self.is_internal():
        if self._right._left is None :
            return self._right
        else:
            iter = self._right
            while iter._left is not None:
                iter = iter._left
            return iter

def preorder_prev(self):
    if self.is_root() is True:
        return None
    elif self.is_internal() is not None:
        if self._parent._left == self:
            return self._parent
        elif self._parent._right == self:
            if self._parent._left._right is None:
                return self._parent._left
            elif self._parent._left._right is None \
                and self._parent._left._left is None:
                return self._parent._left
            elif self._parent._left.right():
                iter = self._parent._left
                while iter._right is not None :
                    iter = iter._right
                return iter
    elif self.is_external() is True:
        if self._parent._left == self:
            return self._parent
        elif self._parent._right == self:
            if self._parent._left is None :
                return self._parent
            elif self._parent._left is not None \
                and self._parent._left.is_external() is True:
                return self._parent._left
            elif self._parent._left is not None \
                and self._parent._left.is_internal() is not None:
                iter = self._parent._left
                while iter._right is not None :
                    iter = iter._right
                return iter

def preorder_next(self):
    if self.is_external():
        if self._parent._left == self:
            return self._parent._right
        elif self._parent._right == self:
            iter = self._parent
            while iter._parent._left != iter:
                iter = iter._parent
                if iter.is_root():
                    return None
            return iter._parent._right
    elif self.is_internal():
        return self._left

def postorder_prev(self):
    if self.is_external():
        if self._parent._left == self:
            iter = self._parent
            check = self
            while iter._left == check:
                if iter.is_root() and iter._left == check:
                    return None
                iter = iter._parent
                check = self._parent
            return iter._left
        elif self._parent._right == self:
            if self._parent._left is None:
                return None
            else:
                return self._parent._left
    elif self.is_internal():
        if self.right():
            return self._right
        else:
            return self._left
    else:
        if self.right():
            return self._right
        else:
            return self._left

def postorder_next(self):
    if self.is_external():
        if self._parent._left == self:
            if self._parent._right is None:
                return None
            elif self._parent._right.is_internal():
                iter = self._parent._right
                while iter.is_external() is False:
                    iter = iter._left
                return iter
            elif self._parent._right.is_external():
                return self._parent._right
        elif self._parent._right == self:
            return self._parent
    elif self.is_internal():
        if self.is_root():
            return None
        if self._parent._left == self:
            iter = self._parent._right
            while iter.is_external() is False:
                iter = iter._left
            return iter
        elif self._parent._right == self:
            return self._parent
```
