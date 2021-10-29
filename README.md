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
  attr_accessor :data, :next_node, :prev_node

  def initialize(data, next_node = nil, prev_node = nil)
    @data = data
    @next_node = next_node
    @prev_node, = prev_node
  end
end
```

All we really had to do was add `@prev_node` into our initializer, and now we
have two pointers on our Node, so that each Node points in two directions: to
the next node in the list, and to the previous node. While this is a really
small and easy change to make to the structure of our Node, by doing this change
we are able to make our Linked List much more useful, efficent, and dynamic!

>> pupper DDL image here 

## When to use a Doubly Linked List

Let's say we have a Singly Linked List, and we wanted to `pop` (or remove) an
item off, we would have to iterate through the _entire_ list in order to find
the second to last node in the list and assign it as the new tail, since we
can't go directly to the tail and work backwards.

```rb
  def pop
    return unless head

    curr = head
    prev = nil
    while curr.next_node
      prev = curr
      curr = curr.next_node
    end

    prev.next_node = nil
  end
```

But, with a Doubly Linked List, we already have a pointer on the tail pointing
to the previous node, so we are able to just take that one step backwards by
using `.prev_node`, without needing to iterate!

```rb
  def pop
    return unless head
    
      old_tail = @tail
    if @length==1
      @head = nil
      @tail = nil
    else 
      @tail = old_tail.prev_node
      new_tail.next = nil
      old_tail.prev_node = nil 
    end
    @length -= 1
    return old_tail
  end
```


## Conclusion

In the end Singly Linked Lists and Doubly Linked Lists are very similar; unlike
arrays they are not indexed, and they use pointers to access the nodes around
them. Singly Linked Lists are just one directional while Doubly Linked Lists go
both ways. The biggest trade off is the memory and space that a Doubly Linked
List takes up in comparison to a Singly Linked List, since we have to account
for and keep track of multiple pointers on each node. However, they are (or can
be) less expensive in terms of time because of the flexibility that added memory
and space gives us!

## Resources

- [Linked list](https://en.wikipedia.org/wiki/Linked_list)
- [Practical Linked List in Ruby](https://www.rubyguides.com/2017/08/ruby-linked-list/)
