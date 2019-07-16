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
b
### std::unordered_map
c

[liquid]: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers
