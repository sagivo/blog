---
layout: post
title: 'Permutations — Ruby Style Recursion '
description: ''
date: '2014-11-08T22:53:00.000Z'
categories: []
keywords: []
---

The permutation problem is as follows: Given a list of items, list all the possible orderings of those items.

We typically list permutations of letters. For example, here are all the permutations of CAT:

CAT

CTA

ACT

ATC

TAC

TCA

There are several different permutation algorithms, but since recursion an emphasis of the course, a recursive algorithm to solve this problem will be presented. (Feel free to come up with an iterative algorithm on your own.)

**The idea is as follows:**

In order to list all the permutations of CAT, we can split our work into three groups of permutations:

1) Permutations that start with C.

2) Permutations that start with A.

3) Permutations that start with T.

The other nice thing to note is that when we list all permutations that start with C, they are nothing put strings that are formed by attaching C to the front of ALL permutations of “AT”. This is nothing but another permutation problem!!!

**Number of recursive calls**

Often times, when recursion is taught, a rule of thumb given is, “recursive functions don’t have loops.” Unfortunately, this rule of thumb is exactly that; it’s not always true. An exception to it is the permutation algorithm.

The problem is that the number of recursive calls is variable. In the example on the previous page, 3 recursive calls were needed. But what if we were permuting the letters in the word, “COMPUTER”? Then 8 recursive calls (1 for each possible starting letter) would be needed.

In essence, we see the need for a loop in the algorithm:

```
for (each possible starting letter)  
  list all permutations that start with that letter
```

**What is the terminating condition?**

Permuting either 0 or 1 element. In the code that will be presented in this lecture, the terminating condition will be when 0 elements are being permuted. (This can be done in exactly one way.)

**Use of an extra parameter**

As we have seen, recursive functions often take in an extra parameter as compared to their iterative counterparts. For the pemutation algorithm, this is also the case. In the recursive characterization of the problem, we have to specify one more piece of information in order for the chain of recursive calls to work. Here is what our function prototype, pre-conditions and post-conditions will look like:

```
// Pre-condition: str is a valid C String, and k is non-negative   
//                          and less than or equal to the length of str.  
// Post-condition: All of the permutations of str with the first k  
//                            characters fixed in their original positions   
//                            are printed. Namely, if n is the length of str,   
//                            then (n-k)! permutations are printed.  
void RecursivePermute(char str\[\], int k);
```

Utilizing this characterization, the terminating condition is when k is equal to the length of the string str, since this means that all the letters in str are fixed. If this is the case, we just want to print out that one permutation.

Otherwise, we want a for loop that tries each character in index k. It’ll look like this:

```js
for (j=k; j<strlen(str); j++) {  
    ExchangeCharacters(str, k, j);             
    RecursivePermute(str, k+1);           
    ExchangeCharacters(str, j, k);  
}
```
where ExchangeCharacters swaps the two characters in str with the given indexes passed in as the last two parameters.


Here is the code is ruby:
[https://gist.github.com/sagivo/b7d772d9b3b531f9996f](https://gist.github.com/sagivo/b7d772d9b3b531f9996f)