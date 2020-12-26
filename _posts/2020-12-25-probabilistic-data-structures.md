---
layout: post
title: Probabilistic Data Structures
tags: 
excerpt_separator: <!--more-->
---

**What are Probabilistic Data Structures?**

When operating with large data sets, parsing through the dataset can be a highly expensive task, which makes computing precise metrics rather costly in practice. 
However, in many cases, an approximation of the metric to be generated can suffice for a given application. 
In these situations, we can employ probabilistic data structures, which allow for estimation of these metrics while significantly reducing the cost of computing them.
Probabilistic data structures (PDSs) are designed to serve specific queries with varying accuracy. Most commonly, they serve a few specific queries about the aggregate data set, 
including cardinality, frequencies, and membership queries. Let’s take Twitter as an example. Below are a few examples of how different PDSs can be used.

<!--more-->

**Linear Counting**

Let’s say we wanted to find the total number of active users within the U.S.. 
Normally, if we wanted to find this, we would have to parse through the entire dataset for every unique user who wrote a tweet in the U.S. 
(this count is called the cardinality) and tally them up. Obviously, when you’re dealing with such a large volume of data, this is a very expensive operation for a simple query. 

We can instead approximate the cardinality using a linear counter. First, we define a bit set of size N. 
For each value inserted into the data set, hash the value to a bit position and set the corresponding bit. 
We can then estimate the cardinality based on the number of set bits.
Due to the hashing, it is possible for values to get hashed to the same bit position (we call this a collision), reducing the accuracy of the cardinality estimation. 
The bigger the bit set is, the more accurate the results are.

**Count-Min Sketch**

Instead of finding the number of unique entries, we may want to find how often one entry occurs in a dataset. 
For example, we may want to find the number of times a tweet is liked. We call this the frequency. 
The most straight-forward means of obtaining the frequency would simply be to maintain a counter for each unique entry. 
The Count-Min Sketch allows us to approximate these frequencies in significantly less space. Let us take a 2D matrix with dimensions (D x W), initialized with zeros. 
We use multiple hash functions, which are each assigned to a row. Then, for every value inserted, we apply each hash function, giving us a column position on the corresponding row.
The value at that row and column position is then incremented. Finally, to check the frequency of a given value, we find these positions again, and choose the minimum of this set of values. 
Because collisions can only increase the count of a given cell, we know that the minimum of all of the corresponding positions will be closest to the real value.

**Bloom Filter**

Another question we might want answered is whether a user exists in the follower list of another user (we call this membership). 
For membership checking, a deterministic response would require a scan through the entire data set to check for existence. 
The Bloom filter serves as a means to guess the membership of an entry with fairly high accuracy, similar in implementation to linear counting. 
Like linear counting, we maintain a single bitset. However, instead of setting a single bit upon adding a value, we set some N bits based on N hash functions. 
Membership is then checked by checking that the N corresponding hashed positions are set. When these bits are not set, we are guaranteed that the value does not exist in the set. 
However, because two values can have overlapping positions that they hash to, it is possible that a collection of values can set the bits of a value not in the set. 
We call this a false-positive.

**Differential Privacy**

While this topic is not itself a use of probabilistic data structures, it uses the same concepts of allowance of error in data, so I’d like to spend some time discussing that here.
Differential privacy can be understood as the process of collecting data in a manner that allows the production and publication of general information (information regarding the data as a whole), 
without revealing private information (association of data to a specific individual contributing to the dataset). Let’s say we need to collect the votes of many users.
Ultimately, what we really want is not each individual user’s vote, but the distribution of votes across the entire user base.
This means we can introduce some randomness into the data entry and still obtain useful information. 
Here is a simple example of differential privacy in inserting data into a dataset. Whenever a user reports their vote, there is a 25% chance that their vote is flipped.
Given that we know the likelihood of whether the actual response will match their original answer, we can then find information about the whole dataset by accounting for this likelihood. Because of the injection of randomness, it is not possible to prove whether any individual’s answer was either ‘yes’ or ‘no’. As a consequence, the result of the data set is independent of any single individuals participation, and information about any one individual cannot be inferred reliably.
