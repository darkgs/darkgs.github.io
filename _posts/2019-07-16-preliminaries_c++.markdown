---
layout: post
title:  "Preliminaries of C++ for coding interview"
date:   2019-07-16 20:41:00
author: Bonhun Koo
categories: [Coding Interview]
tags:	coding
cover:  "/assets/instacode.png"
---

## Numeric limits of variables

{% highlight cpp %}
#include <limits>

int i_min = std::numeric_limits<int>::min();
int i_max = std::numeric_limits<int>::max();

float f_min = std::numeric_limits<float>::min();
float f_max = std::numeric_limits<float>::max();
{% endhighlight %}

## Key-value containers
### Generate std::pair
{% highlight cpp %}
std::pair <int, std::string> p;
p = std::make_pair(10, "aa");
std::pair <int, std::string> p2 (20, "bb");

std::cout << p.first << ":" << p.second << endl;
{% endhighlight %}

### std::map
It stores key-value pair of entires to the balancing BST.
So it inserts, deletes, and searches an entry within the time complexity of O(log(n)) + rebalance.
You can find the detailed api of std::map [here][map_api].
It keeps the <b>uniqueness</b> of keys by ignoring additional insertions with duplicated keys.
{% highlight cpp %}
{% endhighlight %}

### std::unordered_map
c

[map_api]: http://www.cplusplus.com/reference/map/map/
