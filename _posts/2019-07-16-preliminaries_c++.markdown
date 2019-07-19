---
layout: post
title:  "Preliminaries of C++ for coding interview"
date:   2019-07-16 20:41:00
author: Bonhun Koo
categories: [Coding Interview]
tags:    coding
cover:  "/assets/instacode.png"
---

## Numeric Limits of Variables
The maximum/minimum values of variables often are used to initialized in problems asking to find optimal (maximum/minimum) values of solution.

{% highlight cpp %}
#include <limits>

int i_min = std::numeric_limits<int>::min();
int i_max = std::numeric_limits<int>::max();

float f_min = std::numeric_limits<float>::min();
float f_max = std::numeric_limits<float>::max();
{% endhighlight %}

## Sequence Containers
### std::vector
It manages <b>dynamically allocated array</b> to store elements.
Thus, pushing or popping (push_back, pop_back) the lasting element can be done in O(1).
However, inserting or erasing an element at specific position take O(n) because it should move all of the following elements.
{% highlight c++ %}
#include <vector>

using namespace std;

int main() {
    int data[] = {0, 1, 2, 4, 5};
    vector<int> v(data, data + sizeof(data)/sizeof(data[0]));       // initialize from array
    vector<int> v2(5, 10);    // 10 10 10 10 10
    vector<int> v3(v2);        // copy v2
    vector<int> v4(v.begin(), v.begin()+2);        // subset of v, 0 1

    // Sequential access - Efficient
    vector<int>::iterator iter;        // random access iterator

    for (iter = v.begin(); iter != v.end(); ++iter) {
        cout << *iter << " ";
    }
    cout << endl;

    // Random access - Efficient
    for (int i=0; i<v.size(); ++i) {
        cout << v[i] << " ";
    }
    cout << endl;

    // insert / erase - O(n)
    v.insert(v.begin()+3, 3);        // 0 1 2 3 4 5
    v.erase(v.begin()+3, v.begin()+4);        // 0 1 2 4 5

    // push_back / pop_back - O(1)
    cout << v.back() << endl;    // 5
    v.pop_back();        // 0 1 2 4
    v.push_back(5);        // 0 1 2 4 5

    return 0;
}
{% endhighlight %}

### std::deque
It manages <b>dynamically allocated array</b> as <b>d</b>ouble-<b>e</b>nded <b>que</b>ue.
Compared with std:vector, it can push or pop an elements on both ends efficiently.
{% highlight c++ %}
#include <deque>

using namespace std;
int main() {
    int data[] = {2, 3, 4};
    deque<int> d(data, data + sizeof(data)/sizeof(data[1]));

    // Sequential access - Efficient
    deque<int>::iterator iter;        // random access iterator

    // push_back / pop_back - O(1)
    cout << d.back() << endl;    // 4
    d.pop_back();        // 2 3
    d.push_back(4);        // 2 3 4

    // push_front / pop_front - O(1)
    cout << d.front() << endl;    // 2
    d.pop_front();        // 3 4
    d.push_front(2);        // 2 3 4

    return 0;
}
{% endhighlight %}

### std::forward_list
It had been implemented as <b>singly-linked lists</b>.
Compared to array, vector, and deque, it performs better in inserting.
It thus has benefits to perform sorting.
{% highlight c++ %}
#include <forward_list>

using namespace std;

int main() {
    forward_list<int> fl = {2, 5, 4};
    forward_list<int> fl2 = {1, 3, 7};

    // push/pop to the head in O(1)
    fl.push_front(1);   // 1 2 5 4
    cout << fl.front() << endl;     // 1
    fl.pop_front();     // 2 5 4

    // q-sort in O(nlog(n))
    fl.sort(std::greater<int>());       // 5 4 2
    fl.sort(std::less<int>());       // 2 4 5

    // merge two sorted lists
    fl2.sort(std::less<int>());
    fl.merge(fl2, std::less<int>());    // 1 2 3 4 5 7
	fl.reverse();		// 7 5 4 3 2 1

    // insert an elements
    fl.insert_after(++(++(++(++fl.begin()))), 6);   // can't fl.begin()+4
    fl.erase_after(fl.before_begin());      // 2 3 4 5 6 7

    forward_list<int>::iterator iter;   // forward iterator
    for (iter=fl.begin(); iter!=fl.end(); ++iter) {
        cout << *iter << " ";
    }
    cout << endl;
    
    return 0;
}
{% endhighlight %}

### std::list
It stores elements to <b>doubly-linked lists</b>.
Most of APIs are similar to std::forward_list.
Compared to std::forward_list, it can insert/erase an element at certain position.
{% highlight c++ %}
#include <list>

using namespace std;

int main() {
    list<int> l = {2, 4, 5};

    // push/pop to the front in O(1)
    l.push_front(1);   // 1 2 4 5 
    cout << l.front() << endl;     // 1
    l.pop_front();     // 2 4 5

    // push/pop to the back in O(1)
    l.push_back(6);   // 2 4 5 6
    cout << l.back() << endl;     // 6
    l.pop_back();     // 2 4 5

    // insert an element
    l.insert(++l.begin(), 3);

    list<int>::iterator iter;   // bidirectional iterator
    for (iter=l.begin(); iter!=l.end(); ++iter) {
        cout << *iter << " ";
    }
    cout << endl;
    
    return 0;
}
{% endhighlight %}

## Associative Containers (Key-Value paired)
### std::pair
std::pair is entry of key-value containers api in the standard library of c++.
{% highlight cpp %}
std::pair <int, std::string> p;
p = std::make_pair(10, "aa");
std::pair <int, std::string> p2 (20, "bb");

std::cout << p.first << ":" << p.second << endl;
{% endhighlight %}

### std::map
It stores key-value pair of entires to the <b>balancing BST</b>.
So it inserts, deletes, and searches an entry within the time complexity of O(log(n)) + rebalance.
You can find the detailed api of std::map [here][map_api].
It keeps the <b>uniqueness</b> of keys by ignoring additional insertions with duplicated keys.
{% highlight cpp %}
#include <map>

using namespace std;

int main() {
    map<int, int> m;

    m.insert(pair<int, int>(1, 40));    // O(log(n) + rebalance)
    m.insert(pair<int, int>(2, 30));
    m.insert(pair<int, int>(3, 30));
    m.insert(pair<int, int>(3, 40));    // ignored

    map<int,int>::iterator iter;        // bidirectional iterator

    for (iter = m.begin(); iter != m.end(); ++iter) {
        cout << "key : " << iter->first \
            << "value : " << iter->second << endl;
    }

    m.erase(2);        // O(log(n) + rebalance)
    iter = m.find(3);        // O(log(n))
    if (iter != m.end()) {
        m.erase(iter);
    }

    m.size();    // the size of container
    m.count(3);    // the number of entires with key, 0 or 1
    m.empty();    // is empty?
    m.clear();    // reset

    return 0;
}
{% endhighlight %}

### std::unordered_map
It stores key-value pair of entires to the <b>hash table</b>.
So it inserts, deletes, and searches an entry within the time complexity of O(1) in the average and O(n) in the worst case.
You can find the detailed api of std::map [here][unordered_map_api].
It keeps the <b>uniqueness</b> of keys by ignoring additional insertions with duplicated keys.
The usage of std::unordered_map is similar to std::map.
{% highlight cpp %}
#include <unordered_map>

using namespace std;

int main() {
    unordered_map<int, int> m;

    unordered_map<int,int>::iterator iter;        // forward iterator

    return 0;
}
{% endhighlight %}

[map_api]: http://www.cplusplus.com/reference/map/map/
[unordered_map_api]: http://www.cplusplus.com/reference/unordered_map/unordered_map/
