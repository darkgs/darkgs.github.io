---
layout: post
title:  "Preliminaries of C++ for coding interview"
date:   2019-07-16 20:41:00
author: Bonhun Koo
categories: [Coding Interview]
tags:    coding
cover:  "/assets/instacode.jpeg"
---

## Numeric Limits of Variables
The maximum/minimum values of variables often are used to initialized in problems asking to find optimal (maximum/minimum) values of solution.

{% highlight cpp %}
#include <limits>

int i_min = std::numeric_limits<int>::min();        // INT_MIN
int i_max = std::numeric_limits<int>::max();        // INT_MAX

float f_min = std::numeric_limits<float>::min();    // FLT_MIN
float f_max = std::numeric_limits<float>::max();    // FLT_MAX
{% endhighlight %}

## Sequence Containers
### std::vector
It manages <b>dynamically allocated array</b> to store elements.
Thus, pushing or popping (push_back, pop_back) the lasting element can be done in O(1).
However, inserting or erasing an element at specific position take O(n) because it should move all of the following elements.
Find detalied api of std::vector from [here][vector_api].
{% highlight c++ %}
#include <vector>

using namespace std;

int main() {
    int data[] = {0, 1, 2, 4, 5};
    vector<int> v(data, data + sizeof(data)/sizeof(data[0]));    // initialize from array
    vector<int> v2(5, 10);        // 10 10 10 10 10
    vector<int> v3(v2);           // copy v2
    vector<int> v4(v.begin(), v.begin()+2);    // subset of v, 0 1

    // Sequential access - Efficient
    vector<int>::iterator iter;   // random access iterator

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
    v.insert(v.begin()+3, 3);           // 0 1 2 3 4 5
    v.erase(v.begin()+3, v.begin()+4);  // 0 1 2 4 5

    // push_back / pop_back - O(1)
    cout << v.back() << endl;    // 5
    v.pop_back();                // 0 1 2 4
    v.push_back(5);              // 0 1 2 4 5

	// sort a vector
	sort(v.begin(), v.end());
	sort(v.begin(), v.end(), compare);	// bool compare(int i1, int i2) { return i1 < i2; };

    return 0;
}
{% endhighlight %}

### std::deque
It manages <b>dynamically allocated array</b> as <b>d</b>ouble-<b>e</b>nded <b>que</b>ue.
Compared with std:vector, it can push or pop an elements on both ends efficiently.
Find detalied api of std::deque from [here][deque_api].
{% highlight c++ %}
#include <deque>

using namespace std;
int main() {
    int data[] = {2, 3, 4};
    deque<int> d(data, data + sizeof(data)/sizeof(data[1]));

    // Sequential access - Efficient
    deque<int>::iterator iter;  // random access iterator

    // push_back / pop_back - O(1)
    cout << d.back() << endl;   // 4
    d.pop_back();               // 2 3
    d.push_back(4);             // 2 3 4

    // push_front / pop_front - O(1)
    cout << d.front() << endl;  // 2
    d.pop_front();              // 3 4
    d.push_front(2);            // 2 3 4

    return 0;
}
{% endhighlight %}

### std::forward_list
It had been implemented as <b>singly-linked lists</b>.
Compared to array, vector, and deque, it performs better in inserting.
It thus has benefits to perform sorting.
Find detalied api of std::forward_list from [here][forward_list_api].
{% highlight c++ %}
#include <forward_list>

using namespace std;

int main() {
    forward_list<int> fl = {2, 5, 4};
    forward_list<int> fl2 = {1, 3, 7};

    // push/pop to the head in O(1)
    fl.push_front(1);                 // 1 2 5 4
    cout << fl.front() << endl;       // 1
    fl.pop_front();                   // 2 5 4

    // q-sort in O(nlog(n))
    fl.sort(std::greater<int>());     // 5 4 2
    fl.sort(std::less<int>());        // 2 4 5

    // merge two sorted lists
    fl2.sort(std::less<int>());
    fl.merge(fl2, std::less<int>());  // 1 2 3 4 5 7
    fl.reverse();                     // 7 5 4 3 2 1

    // insert an elements
    fl.insert_after(++(++(++(++fl.begin()))), 6);  // can't fl.begin()+4
    fl.erase_after(fl.before_begin());             // 2 3 4 5 6 7

    forward_list<int>::iterator iter;              // forward iterator
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
Find detalied api of std::list from [here][list_api].
{% highlight c++ %}
#include <list>

using namespace std;

int main() {
    list<int> l = {2, 4, 5};

    // push/pop to the front in O(1)
    l.push_front(1);            // 1 2 4 5 
    cout << l.front() << endl;  // 1
    l.pop_front();              // 2 4 5

    // push/pop to the back in O(1)
    l.push_back(6);             // 2 4 5 6
    cout << l.back() << endl;   // 6
    l.pop_back();               // 2 4 5

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

### std::stack
It is designed to serve LIFO (last-in first-out) operations.
To do this, it pushes and pops an element only at the end of the container.
By default, it uses std::deque as internal data structure.
Find detalied api of std::stack from [here][stack_api].

{% highlight c++ %}
#include <stack>

using namespace std;

int main() {
    stack<int> s;

    for (int i=0; i<5; ++i) {
        s.push(i);
    }

    while (s.size() > 0) {
        cout << s.top() << " ";
        s.pop();                    // return nothing
    }
    cout << endl;
    
    return 0;
}
{% endhighlight %}

### std::queue
It is designed to serve FIFO (first-in first-out) operations.
To do this, it pushes an element into the back and pops from the front.
By default, it uses std::deque as internal data structure.
Find detalied api of std::queue from [here][queue_api].
{% highlight c++ %}
#include <queue>

using namespace std;

int main() {
    queue<int> q;

    for (int i=0; i<5; ++i) {
        q.push(i);
    }

    while (q.size() > 0) {
        cout << q.front() << " ";
        q.pop();                    // return nothing
    }
    cout << endl;
    
    return 0;
}
{% endhighlight %}

## Associative Containers (Key-Value paired)
### std::set
It stores entries by its key to the <b>balancing BST</b> as red-black tree.
So it inserts, deletes, and searches an entry within the time complexity of O(log(n)) + rebalance.
It also makes it possible to provide the ordered iteration of entires.
You can find the detailed api of std::set [here][set_api].
It keeps the <b>uniqueness</b> of keys by ignoring additional insertions with duplicated keys.

{% highlight cpp %}
#include <set>

using namespace std;

int main() {
    set<int> s;

    // Each insertion takes O(log(n)) + rebalance
    s.insert(4);
    s.insert(2);
    s.insert(5);
    s.insert(1);
    s.insert(3);

    // Duplicated insertion would be ignored
    s.insert(3);

    // Each remove takes O(log(n)) + rebalance
    s.erase(2);

    s.find(4) != s.end();       // contains

    set<int>::iterator iter;	// bidirectional iterator

    for (iter=s.begin(); iter!=s.end(); ++iter) {
        cout << *iter << " ";   // 1 3 4 5
    }

	cout << endl;

    
    return 0;
}
{% endhighlight %}

### std::unordered_set
It stores entires by its key to the <b>hash table</b>.
So it inserts, deletes, and searches an entry within the time complexity of O(1) in the average and O(n) in the worst case.
You can find the detailed api of std::map [here][unordered_set_api].
It keeps the <b>uniqueness</b> of keys by ignoring additional insertions with duplicated keys.
The usage of std::unordered_set is similar to std::set.
{% highlight cpp %}
#include <unordered_set>

using namespace std;

int main() {
    unordered_set<int> s;

    unordered_set<int>::iterator iter;    // forward iterator

    return 0;
}
{% endhighlight %}

### std::pair
std::pair is entry of key-value containers api in the standard library of c++.
{% highlight cpp %}
std::pair <int, std::string> p;
p = std::make_pair(10, "aa");
std::pair <int, std::string> p2 (20, "bb");

std::cout << p.first << ":" << p.second << endl;
{% endhighlight %}

### std::map
It stores key-value pair of entires to the <b>balancing BST</b> as red-black tree.
So it inserts, deletes, and searches an entry within the time complexity of O(log(n)) + rebalance.
It also makes it possible to provide the ordered iteration of entires.
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

    m.erase(2);          // O(log(n) + rebalance)
    iter = m.find(3);    // O(log(n))
    if (iter != m.end()) {
        m.erase(iter);
    }

    m.size();        // the size of container
    m.count(3);      // the number of entires with key, 0 or 1
    m.empty();       // is empty?
    m.clear();       // reset

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

	unordered_map<int,int>::iterator iter;    // forward iterator

    return 0;
}
{% endhighlight %}

## Binary search applications
C++ STL provides binary-search based libraries for sorted iterable containers.
Three functions (binary_search, lower_bound, and upper_bound) are implemented with B-search thus the time complexity of them are O(logn).

{% highlight cpp %}
#include <vector>
#include <algorithm>

using namespace std;

int main() {
	vector<int> data = {0, 1, 2, 4, 5};

	// iterable should be sorted for using binary-search based functions.
	sort(data.begin(), data.end(), less<int>());

	// check whether an element is in a or not.
	cout << binary_search(data.begin(), data.end(), 3) << endl;		// 0
	cout << binary_search(data.begin(), data.end(), 4) << endl;		// 1

	// lower_bound
	// returns the starting iterator which is pointing the element equal or greater then target.
	// returns a.end() when there is no matching.
	// If you want to push an element with keeping the iterable being sorted, you can push an element here.
	cout << *lower_bound(data.begin(), data.end(), 2) << endl;		// 2
	cout << *lower_bound(data.begin(), data.end(), 3) << endl;		// 4
	lower_bound(data.begin(), data.end(), 6);						// data.end()


	// upper_bound
	// returns the starting iterator which is pointing the element greater then target.
	// returns a.end() when there is no matching.
	cout << *upper_bound(data.begin(), data.end(), 2) << endl;		// 4
	lower_bound(data.begin(), data.end(), 6);						// data.end()

	// erase the elements in [a, b).
	data.erase(lower_bound(data.begin(), data.end(), 2),
			upper_bound(data.begin(), data.end(), 4));				// 0 1 5

	return 0;
}
{% endhighlight %}

[stack_api]: http://www.cplusplus.com/reference/stack/stack/
[queue_api]: http://www.cplusplus.com/reference/queue/queue/
[deque_api]: http://www.cplusplus.com/reference/deque/deque/
[forward_list_api]: http://www.cplusplus.com/reference/forward_list/forward_list/
[list_api]: http://www.cplusplus.com/reference/list/list/
[vector_api]: http://www.cplusplus.com/reference/vector/vector/
[map_api]: http://www.cplusplus.com/reference/map/map/
[set_api]: http://www.cplusplus.com/reference/set/set/
[unordered_map_api]: http://www.cplusplus.com/reference/unordered_map/unordered_map/
[unordered_set_api]: http://www.cplusplus.com/reference/unordered_set/unordered_set/
