---
layout: "post"
title: "Top 10 Microsoft Coding Interview Questions"
author: "mingi hong"
date: 2020-01-23 20:01:59 -0700
categories: Career Interview Tech
permalink: /:categories/:title.html
---

## Tech Interview

# Check if a binary tree is BST or not
1. (Smaller than parent) left <- Node -> right (Bigger than parent)
2. Node starts with (-infinity, infinity)

# Clone a doubly linked list which has a next and an arbitrary (random) pointer
1. Make new list and point to new node
2. Copy arbitrary from new node to original node
3. Copy it to new node

# Connect nodes at the same level in a binary tree
1. Make a queue
2. Put node and null node (to separate)
3. Another level, put nodes and null node at the end
4. O(n)

# Remove duplicates from a string and arrange its letters in order
1. Self balancing Binary search tree and the elements inserted in a set are unique and automatically sorted.
2. If we put it in the set, we are not going to accept same character.
3. O(blogs)

# Given a rolated array which is sorted search for an element in it.
1. Since array is rotated, we need to divide it into two sub arrays.
2. Go to first sub array and compare mynum with first element in subarray
3. If it is smaller than first element in subarray, we are going to switch subarray
4. Use binary search

# Two of the nodes of a BST are swapped correct the BST
1. We do Inorder traversal (left to the right)
2. We now know which value is bigger than parent at the left and which value is smaller than parent at the right

# Detect Cycle in a linked list
1. We will take fast pointer and slow pointer
2. Fast pointer move twice faster than slow pointer
3. If those two pointer meet at some point, we can judge there is a cycle

# Find the Least Common Ancestor (LCA) of a binary tree
1. We start from the top node, look for both left and right
2. Make search result to parent (Null,a)
3. If it makes (a,b) then we found LCA
4. Pass(L,Null) to top node

# Check if a binary tree is height balanced
1. If left sub tree and right sub tree height difference is less than 1, It is balanced.

# Given an unsorted array of size 'n' such that only one element occurs twice. All the elements are from 1 to n. Find the missing element.
1. Put array in to XOR and make another XOR that contains 1 to n
2. And we cancel the element from both side if it matches, then we can get missing element
3. Duplicate element XOR missing element

