---
layout: post
title:  "Essential Algorithms"
date:   2019-08-22 20:41:00
author: Bonhun Koo
categories: [Coding Interview]
tags:    coding
cover:  "/assets/instacode.jpeg"
---

## Graycode
Graycode is a bit encoding method such that two adjacent numbers differ in only one bit.
{% highlight c++ %}
int bin2gray(int bin) {
	return bin ^ (bin >> 1);
}

int gray2bin(int gray) {
	int mask = (gray >> 1);
	int num = gray;

	while (mask != 0) {
		num = num ^ mask;

		mask = mask >> 1;
	}

	return num;
}
{% endhighlight %}

## Rotate a string
Rotate a string on the in-place memory with the time complexity of O(n)
Please find the detailed reference code fromt [here][rotate_string].
{% highlight c++ %}
#include <bits/stdc++.h>

using namespace std;

void left_rotate(string &s, int d) {
	reverse(s.begin(), s.begin()+d);
	reverse(s.begin()+d, s.end());
	reverse(s.begin(), s.end();
}

void right_rotate(string &s, int d) {
	left_rotate(s, s.size()-d);
}

{% endhighlight %}

[rotate_string]: https://www.geeksforgeeks.org/left-rotation-right-rotation-string-2

