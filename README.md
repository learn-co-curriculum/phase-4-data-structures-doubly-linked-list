# Doubly Linked List Data Structure

## Learning Goals

- Identify the use cases for a Doubly Linked List
- Demonstrate common methods for a Doubly Linked List
- Differentiate between a Doubly Linked List and a Singly Linked List

## Introduction

We learned in the last lesson what about Singly Linked Lists, their use cases
and the general concept behind linked lists. In this lesson we're going to dive
into what a Doubly Linked List is, and what the difference is between Singly and
Doubly Linked Lists.

## Defining a Doubly Linked List

In the previous lessons we learned how unlike arrays, linked lists are not
indexed, and in order to search from node to node, we need use pointers to go
from one node onto the next node. But what if we wanted to go back a node? In a
singly linked list a node doesn't know which node came before it, because it
doesn't have a pointer pointing to the previous node. Doubly linked lists
however have pointers to the next node as well as the previous node. Let's take
a look at how we would build this out in our `Node` class.

```rb
class Node
  attr_accessor :data, :next_node

  def initialize(data, next_node = nil, prev_node = nil)
    @data = data
    @next_node = next_node
    @prev_node, = prev_node
  end
end
```

All we really had to do was add `@prev_node` into our initializer, and now we
have two pointers on our Node, one going to the nextnode in the list and one
going to the orevious. ## When to use a Doubly Linked List

<!-- Linked Lists are ideal for situations when you need quick insertion and
deletion, but are more expensive than arrays when it comes to searching, since
arrays are indexed. The Big O for both insertion as well as deletion at a known
node in a linked list is `0(1)` because we don't need to update indexes for the
other elements in the list when a new element is added: we just need to adjust
which node the `next_node` points to. With an array, insertion and deletion from
anywhere other than the end are `O(n)`, because other elements need to be
reindexed. -->

## Conclusion

We use linked lists because they can be less expensive than arrays when it comes
to insertion and deletion within lists. Linked Lists are a very common interview
data structure, so make sure you get to know them! In the next lesson, we'll
build more methods in our `LinkedList` class.

## Resources

- [Linked list](https://en.wikipedia.org/wiki/Linked_list)
- [Practical Linked List in Ruby](https://www.rubyguides.com/2017/08/ruby-linked-list/)
