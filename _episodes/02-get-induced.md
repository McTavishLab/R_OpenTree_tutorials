---
source: Rmd
title: "Getting a piece of the Synthetic Open Tree of Life"
teaching: 5
exercises: 5
questions:
- "What is the synthetic Open Tree of Life?"
- "How do I interact with it?"
- "Why is my taxon not in the tree?"
objectives:
- "Get an induced subtree"
- "Get a subtree"
keypoints:
- "OTT ids and node ids allow us to interact with the synthetic OpenTree."
- "Portions of the synthetic OpenTree can be extracted from a single OTT id or from a bunch of OTT ids"
- "It is not possible to get a subtree from an OTT id that is not in the synthetic tree."
---

<br/>
<br/>

The synthetic Open Tree of Life (**synthetic OpenTree** from now on) summarizes information from **1239**
trees from **1184** peer-reviewed and published studies, that have been uploaded to the OpenTree database (the Phylesystem) through a [curator system](https://tree.opentreeoflife.org/curator).

Functions from the `rotl` package that interact with the synthetic OpenTree start with `tol_`.

To access general information about the current synthetic OpenTree, we can use the function `tol_about()`. This function requires no argument.


~~~
rotl::tol_about()
~~~
{: .language-r}



~~~

OpenTree Synthetic Tree of Life.

Tree version: opentree13.4
Taxonomy version: 3.3draft1
Constructed on: 2021-06-18 11:13:49
Number of terminal taxa: 2392042
Number of source trees: 1239
Number of source studies: 1184
Source list present: false
Root taxon: cellular organisms
Root ott_id: 93302
Root node_id: ott93302
~~~
{: .output}
This is nice!

As you can note, the current synthetic OpenTree was created not too long ago, on 2021-06-18 11:13:49.

This is also telling us that there are currently more than **2 million tips** on the synthetic OpenTree.

It is indeed a large tree. So, **_what if we just want a small piece of the whole synthetic OpenTree?_**

Well, now that we have some interesting taxon OTT ids, we can easily do this.


### Getting an induced subtree

The function `tol_induced_subtree()` allows us to get a tree of taxa from different taxonomic ranks.


~~~
my_tree <- rotl::tol_induced_subtree(resolved_names$ott_id)
~~~
{: .language-r}



~~~
Error: HTTP failure: 400
Expecting "ott_ids" argument to be an array
~~~
{: .error}

<br/>

> ## Note: What does this warning mean?
>
> This warning has to do with the way the synthetic OpenTree is generated.
> You can look at the [overview of the synthesis algorithm](https://docs.google.com/presentation/d/1RwoNTUK3LKgBupBNOc1TNsIpucuMUddlYW6rC56We10/edit?usp=sharing) for more information.
>
>
{: .discussion}

<br/>

Let's look at the output of `tol_induced_subtree()`.


~~~
my_tree
~~~
{: .language-r}



~~~
Error in eval(expr, envir, enclos): object 'my_tree' not found
~~~
{: .error}















