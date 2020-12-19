# Linked Lists

[Source: Linked Lists](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-05/resources/singly_linked_list.html)

[Source: What's a Linked List, Anyway? Part 1](https://medium.com/basecs/whats-a-linked-list-anyway-part-1-d8b7e6508b9d)

[Source: What's a Linked List, Anyway? Part 2](https://medium.com/basecs/whats-a-linked-list-anyway-part-2-131d96f71996)

A sequence of `Nodes` that are connected/linked to each other. Each `Node` references the next. Linear data structure.

![linked list](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-05/resources/images/LinkedList1.PNG)

## Terms

- `Singly` - Each `Node` has one reference. Points to `Next`
- `Doubly` - Each `Node` has two references. Point to `Next` and `Previous`
- `Circular` - `Tail` reference node points to `Head` instead of Null
- `Node` - Items/links in a linked list
- `Next` - Property on a `Node` containing reference to next node
- `Head` - Reference type of node to the first node in a linked list
- `Tail` - Reference type of node to the last node in a circular linked list
- `Current` - Reference type of node to the node currently being looked at. Helpful when traversing. Reset to head when done.

## Traversal

```javascript
ALGORTIHM Includes (value)
    // INPUT <-- integer value
    // OUTPUT <-- boolean

    Current <-- Head

    WHILE Current is not NULL
        IF Current.Value is equal to value
            return TRUE

        Current <-- Current.Next

    return FALSE
```

1. Set current to head to start @ beginning

2. While loop, until null (end)

3. Check values

4. Move to next if no match

5. If we hit end, not in list

## Adding a Node

### O(1) - Start

```javascript
ALGORITHM Add(newNode)
    // INPUT <-- Node to add 
    // OUTPUT<-- No output

        Current <-- Head
        newNode.Next <-- Head
        Head <-- newNode
        Current <-- Head
```

1. Set current to Head
2. Instantiate node, set Next to the current Head
3. Set Head to new node
4. Set Current to new node

### O(n) - Middle/End

```javascript
ALGORITHM AddBefore(newNode, existingNode)
    // INPUT <-- New Node, Existing Node
    // OUTPUT <-- No Output

        Current <-- Head

        while Current.Next is not equal to NULL
            if Current.Next.Value is equal to existingNode.Value
                newNode.Next <-- existingNode
                Current.Next <-- newNode

            Current <-- Current.Next
```

Traverse, but...

1. Check when current equal to new node
2. Set newNode.next = existing
3. Set current.next = newNode

## Printing

```javascript
ALGORITHM Print()
    // INPUT <-- None
    // OUTPUT <-- string to console

        Current <-- Head

        while Current.Next is not equal to NULL
            OUTPUT <-- "Current.Value --> "
            Current <-- Current.Next

        OUTPUT <-- "Current.Value --> NULL"
```

## Big O

- Traversing
  - Time: O(n) -- worst case is last
  - Space: O(1) -- no extra space needed besides list itself
- Adding
  - Start: O(1) -- constant
  - Middle/End: O(n) -- linear

## Memory

In a dynamically typed language (JS, Python) we don't think about memory use because types are abstracted from us. It's happening, but we don't see it.

### Array

- Store entire array memory sequentially
- Static data structure, new memory allocated for entire array when needs change
- Copy elements to change

### Linked List

- Nodes can be stored in various locations
- Dynamic data structure, memory added/removed as elemented added/removed
- Rearrange pointers to change

## Advantages / Disadvantages

- `Good`  Memory efficient when adding/removing
- `Bad` Memory iniefficent when searching
