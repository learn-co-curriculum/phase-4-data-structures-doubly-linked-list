# Doubly Linked List Data Structure

## Learning Goals

- Identify the use cases for a **doubly linked list**
- Demonstrate common methods for a doubly linked list
- Differentiate between a doubly linked list and a singly linked list

## Introduction

We learned in the last lesson about singly linked lists, their use cases, and
the general concept behind linked lists. In this lesson, we're going to dive
into what a doubly linked list is, and what the difference is between singly and
doubly linked lists.

## Defining a Doubly Linked List

In the previous lessons we learned how unlike arrays, linked lists are not
indexed, and in order to search from node to node, we need use pointers to go
from one node onto the next node. But what if we wanted to go back a node? In a
singly linked list, a node doesn't know which node came before it, because it
doesn't have a pointer pointing to the previous node. Doubly linked lists have
pointers to the next node _as well as_ the previous node. Let's take a look at
how we would build this out in our `Node` class:

```rb
class Node
  attr_accessor :data, :next_node, :prev_node

  def initialize(data, next_node = nil, prev_node = nil)
    @data = data
    @next_node = next_node
    @prev_node = prev_node
  end
end
```

All we really had to do was a `prev_node` attribute, and now we have two
pointers on our `Node` class, so that each node points in two directions: to the
next node in the list, and to the previous node. While this is a really small
and easy change to make to the structure of our node, by doing this change we
are able to make our linked list much more useful, efficient, and dynamic!

[Pup Linked List](https://curriculum-content.s3.amazonaws.com/pup_doubly_linked_list.png)

## Singly vs Doubly Linked Lists

One way we can improve the time complexity of our singly linked list
implementation is by adding additional references to modes in the list. For
example, consider the following `SinglyLinkedList` class:

```rb
class SinglyLinkedList
  attr_accessor :head

  def initialize
    @head = nil
  end

  def append(node)
    if head.nil?
      self.head = node
    else
      # traverse the entire list to find the last node
      curr = head
      while curr.next_node
        curr = curr.next_node
      end
      # add a new node at the end
      # 1 -> 2 -> 3
      curr.next_node = node
      # 1 -> 2 -> 3 -> 4
    end
  end
end
```

The time complexity of its `append` method is O(n), since we need to traverse
each element of the linked list in order to find the last node and remove it. We
can make the `append` method efficient by keeping track of the list's **tail**
node in addition to the head:

```rb
class SinglyLinkedList
  attr_accessor :head, :tail

  def initialize
    @head = nil
    @tail = nil
  end

  def append(node)
    if head.nil?
      self.head = node
      self.tail = node
    else
      # no need to traverse! we can access the last node directly (self.tail)
      # 1 -> 2 -> 3
      self.tail.next_node = node
      # 1 -> 2 -> 3 -> 4
      # we also need to make sure to keep track of the new tail
      self.tail = node
    end
  end
end
```

After this refactor, the time complexity of our `append` method is O(1), since
we no longer need to traverse the list in order to find the tail before
appending a new node. The tradeoff to keeping references to additional nodes in
our list, like the tail, is it takes more space to keep track of these
additional references. Adding additional reference can also increase the written
complexity of our code for certain methods, since we need to maintain those
references correctly.

A **doubly linked list** makes insertion and removal more efficient in certain
cases by keeping references to the **previous** node in addition to the next
node.

Let's say we have a singly linked list, and we wanted to remove the last item.
We would have to traverse the _entire_ list in order to find the _second to last
node_ in the list and assign it as the new tail, since we can't go directly to
the tail and work backwards:

```rb
class SinglyLinkedList
  attr_accessor :head, :tail

  def initialize
    @head = nil
    @tail = nil
  end

  def delete_tail
    return if head.nil?

    # traverse the entire list to find the second-to-last node (prev)
    curr = head
    prev = nil
    while curr.next_node
      prev = curr
      curr = curr.next_node
    end

    # remove the last node by removing the link between the second-to-last node and the tail
    # 1 -> 2 -> 3
    prev.next_node = nil
    # 1 -> 2
  end
end
```

In this case, the time complexity for removing a node from the end of the list
is O(n) since we need to traverse the list to find the second-to-last node.

With a **doubly linked list**, we already have a pointer on each node pointing
to the previous node, so we can just take one step backwards from the `tail` by
using `.prev_node` without needing to iterate:

```rb
class DoublyLinkedList
  attr_accessor :head, :tail

  def initialize
    @head = nil
    @tail = nil
  end
  
  def delete_tail
    return if head.nil?
    
    if head == tail
      self.head = nil
      self.tail = nil
    else
      # access the second-to-last node (self.tail.prev_node)
      prev = self.tail.prev_node
      # update the tail and next_node pointers
      # 1 <-> 2 <-> 3
      prev.next_node = nil
      self.tail = prev
      # 1 <-> 2
    end
  end
end
```

After this refactor, our time complexity for removing a node from the end of the
list is O(1), since we don't need to traverse the entire list to find the new
tail.

The tradeoff is that we now need to maintain these references between nodes
correctly for _all_ our linked list methods. For example, the `append` method
for a doubly linked list is more complicated than for a singly linked list,
since we have to keep track of the `next_node` and `prev_node` correctly any
time a node is added:

```rb
class DoublyLinkedList
  attr_accessor :head, :tail

  def initialize
    @head = nil
    @tail = nil
  end

  def append(node)
    if head.nil?
      self.head = node
      self.tail = node
    else
      node.prev_node = self.tail
      tail.next_node = node
      self.tail = node
    end
  end
end
```

## Conclusion

Singly linked lists and doubly linked lists are very similar. Unlike arrays,
they are not indexed, and they use pointers to reference their nodes. Singly
linked lists are one-directional, while doubly linked lists go both ways. Having
the extra `prev_node` pointer in a doubly linked list can be useful in the
following scenarios:

- Removing an item form the end of a list
- Reversing the list (traversal from tail to head)
- Implementing "previous/next" operations or "undo/redo" operations (like
  building a playlist or a text editor)

The trade-offs are that a doubly linked list takes up more memory than to a
singly linked list, since we have to keep track of multiple pointers on each
node; and the code for a doubly linked list is also more complicated to write
and maintain because of the added complexity of keeping both the `next_node` and
`prev_node` references.

## Resources

- [Linked List](https://en.wikipedia.org/wiki/Linked_list)
- [Practical Linked List in Ruby](https://www.rubyguides.com/2017/08/ruby-linked-list/)
