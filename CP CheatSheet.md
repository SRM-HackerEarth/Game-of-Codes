<img src="https://res.cloudinary.com/dcykxiua2/image/upload/v1610645397/images_kciwzs.jpg" width="800" height="200" >

# **CP CHEATSHEET FOR BEGINNERS**

I hope you all have solved your first competitive programming problem. This is the time to introduce some widely used concepts you should know before digging deep into the world of CP. Some of my code in this post may be in C++, but you can translate that to the language of your choice by exploring it.

------------

## **Table of Contents**
- IDE and compilers
- Header files
- Data types
- Input and output
- Basic math functions
  - Modular Arithmetic
  - Power function, pow(x, n)
  - Greatest Common Divisor, gcd(a, b)
  - Lowest Common Multiple, lcm(a, b)
- Some standard templates
- Sorting a vector
  - Basic sorting
  - Using a comparator
  - Defining your own comparator
- Creating your graph and trees

------------

### **IDE AND COMPILERS:**

You can use any language of your choice but C++, Java, and Python are the most preferred. If you are equally good/bad in all of these, most people will recommend C++. Do not switch language just because of many people preferring a specific language, shift when you feel necessary.

### For C &#47; C++ :
* Using Linux is most recommended.
* Turbo C++ is not standard. Uninstall it (-_-)
*  If using Linux, gcc/g++ and any simple/lightweight IDE is recommended.
* If using Windows, setting up Code:Blocks is one of the best options.
* You can also use online compilers and IDE, but they come with their own disadvantages.

### For Java :
* I use Eclipse IDE on my Ubuntu machine.

------------


### **HEADER FILES:**
If you are a C++ user, you can use just one header file, which has the capacity to replace all the header files you will possibly require in any CP contest.
``` c
#include<bits/stdc++.h>
using namespace std; 
```

------------


### **DATA TYPES:**
- Use *long long* if the result/intermediate result can be greater than 10<sup>9</sup>. Use int for numbers smaller than that.
- Do not use *long long* for numbers greater than 10<sup>18</sup>. Keep intermediate results less than that. If it is required to handle numbers larger than that use BigInteger library or strings.
- Avoid comparing floats/doubles because of precision.
- Strings in Java are immutable, use them wisely.

------------

### **INPUT AND OUTPUT:**
 In most challenges, the problem asks you to solve multiple test cases. The number of test cases *t*   is given and you need to print the answer for these test cases one after the other. You can process each test cases separately and print the answer as you solve. There is no need to read the entire test cases in the beginning. Input and output as a separate file. Once your program terminates, the online judge compares the output file (whatever you printed) with the expected output. The following function shows t test cases where a number *n* is given as input and you need to calculate n+1.

```c
#include<stdio.h>
int solve(int n) {
    return n+1;
}
int main() {
    int t;
    scanf("%d",&t);
    while(t--) {
        // take input for each test case
        int n; 
        scanf("%d",&n);

        // process the current test case and print the answer
        int ans = solve(n);
        printf("%d\n",ans);
    }
    return 0;
}
```

------------


### **BASIC MATH FUNCTIONS**:
Let&#39;s start with some maths:

### **1. Modular Arithmetic :**

The first confusing thing that you will encounter while solving a challenge is the statement, &ldquo;*Answer the result modulo 10000007* &rdquo;.This statement is very common in challenges where the result may be very large. But, what does that even mean?

In **modular arithmetic**, numbers&ldquo;wrap around&rdquo;upon reaching a given fixed quantity (this given quantity is known as the modulus) to leave a remainder (Example, time wraps around after every 24 hours).

Modulo is almost similar to the % operator in C &#47; C++&#47; Java. *(Your task is to find the differences).*

In some cases, the result / intermediate results of the computation can be very large. Since integers have finite memory, this result/intermediate results may overflow. In order to preserve the essence of the problem and algorithms involved, the author does not generally want you to focus on these issues like designing big integers or making basic operations super fast on these numbers you design. In simple words, the author wants you to convert your result into a value which is guaranteed to be smaller than *10000007.*

Modulo provides us some important properties that help you prevent overflow of these results or intermediate results while calculating these results. Let&#39;s explore some of the important properties:
1. (a &#43; b)%m = ((a % m) &#43; (b % m)) % m
2. (a &#42; b)%m = ((a % m) &#42; (b % m)) % m
3. (a &#45; b)%m = ((a % m) &#45; (b % m)) % m
4. Any combination of &#43;, &#45; , &#42; operations with modulo is same as applying modulo operations on any of the intermediate results and applying on the final result. For example, (a &#42; b + c)%m = (((a % m) &#42;(b % m))%m + (c % m)) % m
5. a % m = (a&#43;m) % m

Observe the fourth property carefully, it guarantees that all of your intermediate results, as well as the final result, will be smaller than* m.* 

You just need to be careful that result after modulo operation cannot be negative by definition. This is where the fifth property helps. If your calculations involve minus operations and your programming language defines modulo as remainder, you may use the 5th property.

------------


### **2. Power function, pow(x, n) :**
This is one of the most used mathematical functions in CP. Almost all the programming languages provide this function. This function returns the value x raised to the power of n. But, the situation becomes tricky if you are required to print your result modulo m. You cannot use* pow(x, n) % m* because there will be overflow for large values of *n* inside the function itself. Let&#39;s rewrite *pow* function such that it takes third argument m as mentioned above. One solution is to use the fourth property mentioned above:
```c
int pow(int x, int n, int m) {
    int ans = 1;
    for(int i=1; i<=n; i++)
        ans = (ans * x)%m;
    return ans;
}
```
The above function takes O(n) time and can be improved. Let&#39;s use some recursion:
```c
int pow(int x,int n, int m) {
    if(n==0)
        return 1;
    int smallAns = pow(x, n/2, m) % m;
    if(n % 2 == 0)
        return (smallAns * smallAns) % m;
    return (x * (smallAns * smallAns) % m) % m;
}
```
If you do not understand what is going on inside the above function, go through the Recursion module I shared previously. Your task is to find the time complexity of the above function.

------------


### **3. Greatest Common Divisor, gcd(a, b) :**
All of you must be aware of GCD or HCF from your school. This function is very much popular in competitive programming. The function finds the greatest divisor that is shared by both of these numbers a and b. This function is provided by many programming languages. For example you can use *__gcd(a, b)*  in C++.

You can implement your own gcd function as below:
```c
int gcd(int a,int b){
    if (b == 0)
        return a;
    return gcd(b, a % b);
}
```
The above algorithm is based on the fact that, if we subtract the smaller number from the larger number, GCD doesn&#39;t change. So if we keep subtracting repeatedly the larger of two, we end up with GCD.

------------


### **4. Lowest Common Multiple, lcm(a, b):**
LCM of two numbers is equal to the product of the two numbers divided by their GCD. You may write your *lcm(a, b)* function as:
```c
int lcm(int a, int b) {
    return (a * b) / gcd(a, b);
}
```

------------



### **SOME STANDARD TEMPLATES:**
Go through the following [tutorial](https://www.topcoder.com/community/competitive-programming/tutorials/power-up-c-with-the-standard-template-library-part-1/)  to understand about STL. You need not require to read it one go, just have an idea what STL is and why is it used. As a beginner, you should be aware of vectors. You will come across all the other useful templates later as you grow.

Go through vectors and pair before you proceed to the next section.

### **SORTING A VECTOR:**
Sorting is one of the most discussed tricks in the history of programming. Thanks to the creators of these amazing programming languages, most of these languages have the inbuilt capability to sort your arrays.

### **1. Basic sorting:**
Suppose you have a vector of integers and you need to sort them in ascending (non-decreasing) order. The first variation sorts the entire vector while the second one sorts the first *n* elements.
```c
sort(myVec.begin(), myVec.end());
// or
sort(myVec.begin(), myVec.begin() + n);
```
### **2. Using a comparator:**
Suppose you want to sort the vector of integers in the descending (non-increasing) order. For this, you need to provide a comparator as the third argument in sort function. This comparator will tell *sort* how to order the elements.
```c
sort(myVec.begin(), myVec.end(), greater <>());
```

### **3. Defining your own comparator:**
Suppose instead of an integer vector, you need to sort a vector of pair of integers
```cpp
( vector<pair<int, int>> )
```
Vector of pair means each position in the vector has two integers. You want to do it in such a way that it is sorted according to the second element. Ties are broken by comparing the first element. * (Explore what happens if you do not provide the third argument in sort function)*. This can be done by writing your own comparator function and providing it as the third argument in sort.
```cpp
bool comp(const pair<int, int> &a, const pair<int, int> &b) {
    if(a.second == b.second)
        return (a.first < b.first);
    return a.second < b.second;
 }
sort(myVec.begin(), myVec.end(), comp);
```

------------


### **CREATING YOUR GRAPH AND TREES:**

Most beginners are afraid of graphs and trees. Let me tell you implementing these is not much difficult. Read about graph representation [here](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs). Particularly focus on the adjacency list representation as you will be using it most of the times. As mentioned in the article, it is just a list of lists. The simplest and most widely used way is to represent a graph is as a vector of vectors. The* i&#39;th* vector in the list represents the list of nodes it is connected with an edge. You can represent a graph as:
```cpp
#define n 1000000
vector G(n);
void addEdge(int u, int v) {
    G[u].push_back(v);
}
```
Using the above graph as a tree is straight forward as you can view trees as undirected graphs. You can perform a DFS on this tree as follows:

```cpp
void DFS(int node, int parent, int [] vis) {

    // use vis[] in graphs and parent in trees
    if(vis[node]) return;
    vis[node] = 1;
    for(int i=0; i<G[node].size(); i++) {
        if(G[node][i] == parent) continue;
        DFS(G[node][i], node);
        // do your task here..
    }
}
```
As you have studied recursion already, observe closely, what is happening in the above function.




