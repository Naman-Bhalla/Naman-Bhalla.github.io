---
layout: post
title: "Graphs and their Representations"
date: 2017-10-17 16:53:00 +0530
categories: data-structures-and-algorithms
comments: true
description: Introduction to Graphs and their Representations
keywords: Graph, Graph Data Structure, Python Graph, Adjacency List, Adjacency Matrix, Code
---

In this post, I am going to talk about the Graph Data Structure and various ways in which Graphs can be represented, alongwith their Pros and Cons. We shall also be implementing one such representation here - **"Adjacency List"** in Python.

## So, What is a Graph Data Structure ?

A graph is a collection of vertices (or nodes) and edges. Umm !!! So, what are these edges ? Well, they are a collection of ordered pairs of vertices (u, v) and can be weighted or unweighted. An ordered pair (u, v) means there is a direct path from u to v. In case of undirected graphs, a direct path from u to v implies a direct path from v to u, thus, if there is an ordered pair (u,v) there has to be another ordered pair (v,u). Ok. So, what do we mean by weights ? Weights can represent distance, cost, time or anything else upon which a graph can be evaluated.  
Let me explain this to you with an example. Take the case of your city map. All the roads are, yes, you guessed correctly, edges !!! And all the junctions are our nodes. Normally, a road from one point to another also means there exists a road backwards too. Well, that is an example of our Undirected Graph. But, a few times there is a one-way. And before you say it, yes, it is an example of a directed graph (or, a diagraph). Now, coming to weights. Suppose, you have two different routes from a place to other. How do you decide which one is better ? The length of the route ? Time it shall take ? Quality of the route ? A combination of these ? Well, these are what  we imply by weights, and shall be more inituitive to you when we discuss shortest path problems etc. An unweighted graph, implies all paths are equally good.  

And, if images speak better than words, here is a visual representation, credits : [GeeksForGeeks](http://www.geeksforgeeks.org/graph-and-its-representations/).  

![Graph]({{"/assets/posts/graph_representation12.png" | absolute_url}})

I hope this gave you a fairly good sense of what a Graph is and what it represents. Now, how do we represent a graph in a computer ?

## Graph Representations

There are 2 commonly used representations of graphs -  
1. Adjacency Matrix 
2. Adjacency List

There do exist a few other representations too, but their concept can almost always be derived from these basic representations. Infact, the representation I will be implementing a few paragraphs from now will actually be a more efficient version of an Adjacency List (some call it Adjacency Map). Now, let us discuss these representations individually.

### Adjacency Matrix

An Adjacency Matrix is a 2D array of size \|V\| x \|V\| (square of number of vertices). In an adjacency matrix (let us consider it is called **_matrix_**, a non zero number (let us say _w_ at position (i , j) ( matrix[i][j] ) indicates an edge from vertex i to j with weight w. On the other hand, a zero indicates no edge between the two vertices. (Well, actually you can modify this representation as per your requirement !)

**PROS**
You can find if there is an edge from vertex i to j in O(1) (You just have to check the value at matrix[i][j] and that's it !). Also, you can update (add or remove) an edge in O(1) !.

**CONS**
One major concern with Adjacency Matrices is regarding their Space Requirements. As mentioned at the very beginning, they always occupy O(V<sup>2</sup>) space. This can be a major issue when the matrix is sparse (Think of a case when we have 10 vertices and only 1 edge .You have (10 x 10) - 1 spaces waste !!!).

>#### NOTE:
An Adjacency Matrix for an Undirected Graph is symmetrical !! (Think Why ?). Take it as an implementation hint if you want to reduce space requirements by half ! Don't construct one half of the matrix and adjust your algorithm.

### Adjacency List

![Adjacency List]({{"/assets/posts/adjacency_list_representation.png" | absolute_url}})

Let me explain this with the help of the above diagram, credits: [GeeksForGeeks](http://www.geeksforgeeks.org/graph-and-its-representations/). An Adjacency List representation consists of lists of lists !!! Didn't quite understand ? Ok. Let me explain a bit better. An Adjacency List is a linked list (sometimes, array also works !) where data part of each node points to a Linked List of all the vertices "adjacent" to it. You might ask, how to represent weights. Well, just alter the data part of the linked linked list accordingly !!!

Before I discuss the pros and ocns of an adjacency list representation, let me walk you through my Python implementation of it.

<script src="//repl.it/embed/MlJ2/0.js"></script>

Here, I have created a "Graph" class to represent our Graph. It includes 3 methods( __init\_\_ to initialize the Graph, addEdge to add an edge (wasn't that obvious ?), __str\_\_ to give a human readable representation of the graph.)  
Now, let us start with the initialization part. As can be clearly understood, the whole graph is represented as a dictionary (**{}**)(HashMap in Python?). The dictionary shall have all the vertices of our graph as the keys (keep reading to know how) and each key will again point to a dictionary, a dictionary with keys as all the adjacent vertices.) The reason I have implemented it as a dictionary of dictionary is to make our implementation fast. A dictionary has an expected look-up time of O(1) in contrast to O(n) for a linked list, so an issue with the adjaceny lists will be handled to a good probability. If you have any doubts with what a dictionary is, and how it works, I suggest you to read this [Python Tutorial - Dictionaries](https://docs.python.org/3.6/tutorial/datastructures.html#dictionaries) .  
We, also have a boolean variable, directed to implement cases for an Undirected Graph where a very (u, v) also implies a vertex (v, u). The code of addEdge, I guess, should be pretty clear if you understand how dictionaries work in Python. Feel free to ask about it in the comments. 2 points - We have added an if clause to also add an edge automatically from v to u when a method to add edge from u to v is called. And secondly, we have actually added vertex as a key with its value as the weight so as to make our implementation easy to use in varying cases. (If you don't understand, spend some time reading it and I am sure it will make sense !). The __str\_\_ code also is pretty self explanatory.

Now, let us discuss the PROS and CONS of an adjacency list.

**PROS**
The biggest pro is that it doesn't have space requirements like the matrix implementation and takes only O(|V| + |E|) space (total number of vertices and edges) as that is the only thing stored ! 

**CONS**
The biggest concern with an adjacency list is its look up time. To find if there is an edge, to delete an edge, you have to search the whole linked linked list and that is O(N).

>But But But ........ We have already overcome this con via our implementation of a variant of Adjacency List with a O(1) look up time to a good probability. So, looks like we are good with it !!

## Real World Graph Usage - Networks
I feel the post can't be complete till it gives you various real life application of graphs. You might find that you use graphs every day !!  Networks - Yep !! All networks, from an internet to your social network. It is all graphs. Your friends list on Facebook is an example of an Undirected Graph (Why ?) and your follower list on Twitter, it is an example of a directed graph (Why ?). Graph algortihms are used for GPS Navigation purposes, by Google Maps and all, to find you the best path possible. Their applications are infinite and you can read more of them on [Wikipedia](http://en.wikipedia.org/wiki/Graph_theory#Applications).

So, with this, I end the post. I hope it gave you a pretty good sense of what Graph is and prepares you for the various Graph algorithms I shall be discussing in my future posts. Looking forward to your feedback. I will be focussing on Graph related posts for this whole week. And probably, a few on NLP and Deep Learning too, if possible.

Regards.

{% include  share.html %}

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = "https://naman-bhalla.github.io/data-structures-and-algorithms/2017/10/17/graphs-and-their-representations.html";  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = "graph1"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://namanbhalla-in.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}


<!-- Go to www.addthis.com/dashboard to customize your tools -->
<script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-597a01df538d0348"></script>
