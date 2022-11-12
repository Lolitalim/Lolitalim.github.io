---
layout: post
title: "[Recurrent Problems]Josephus Problem"
date: 2018-04-08
tags: Algorithms
---


Remarks:

I wrote this paragraph in my junior year on another blog platform.

<https://www.cnblogs.com/HuisClos/p/8438960.html> 

I move it over here.

Surprisingly, I've read so many things in undergraduate years, but keep on nothing up until now :)

-----



$n$ people (numbered $1$ to $n$) around a circle, eliminate every second remaining person until only one survives.

The problem — given the number of people, starting point, direction, and number to be skipped — is to choose the position in the initial circle to avoid execution.

Solution:

In the following, $n$ denotes the number of people in the initial circle, and $k$ denotes the count for each step, that is, $k-1$ people are skipped and the $k$th is executed.

The people in the circle are numbered from $1$ to $n$.
$k=2$

We explicitly solve the problem when every second person will be killed, i.e. $k=2$. (For the more general case k\neq 2, we outline a solution below.) We express the solution recursively.

Let $f(n)$ denote the position of the survivor when there are initially $n$ people (and $k=2$).

The first time around the circle, all of the $even$-numbered people die.

The second time around the circle, the new $2$nd person dies, then the new $4$th person, etc.;

it's as though there were no first time around the circle.

 
If the initial number of people was $even$, then the person in position x during the second time around the circle was originally in position $2x-1$ (for every choice of $x$).

Let $n=2j$. The person at $f(j)$ who will now survive was originally in position $2f(j)-1$.

This gives us the recurrence $f(2j)=2f(j)-1$;.

If the initial number of people was $odd$, then we think of person $1$ as dying at the end of the first time around the circle.

Again, during the second time around the circle, the new $2$nd person dies, then the new $4$th person, etc. In this case, the person in position $x$ was originally in position $2x+1$.

This gives us the recurrence $f(2j+1)=2f(j)+1$;.

When we tabulate the values of $n$ and $f(n)$ we see a pattern:
n	1	2	3	4	5	6	7	8	9	10	11	12	13	14	15	16
f(n)	1	1	3	1	3	5	7	1	3	5	7	9	11	13	15	1

This suggests that $f(n)$ is an increasing odd sequence that restarts with $f(n)=1$ whenever the index $n$ is a power of $2$.

Therefore, if we choose m and l so that $n=2^{m}+l$ and $0\leq l<2^{m}$, then $f(n)=2\cdot l+1$.

It is clear that values in the table satisfy this equation. Or we can think that after $l$ people are dead there are only $2^{m}$ people and we go to the $2l+1th$ person.

He must be the survivor. So $f(n)=2l+1$. Below, we give a proof by induction.

Theorem: If $n=2^{m}+l$ and $0\leq l<2^{m}$, then $f(n)=2l+1$.

Proof: We use strong induction on $n$.
      The base case $n=1$ is true.
      We consider separately the cases when $n$ is $even$ and when $n$ is $odd$.
      If $n$ is $even$, then choose $l_{1}$ and $m_{1}$ such that $n/2=2^{{m_{1}}}+l_{1}$ and $0\leq l_{1}<2^{{m_{1}}}$. Note that $l_{1}=l/2$.
      We have $f(n)=2f(n/2)-1=2((2l_{1})+1)-1=2l+1$, where the second equality follows from the induction hypothesis.
      If $n$ is $odd$, then choose $l_{1}$ and $m_{1}$ such that $(n-1)/2=2^{{m_{1}}}+l_{1}$ and $0\leq l_{1}<2^{{m_{1}}}$. Note that $l_{1}=(l-1)/2$.
      We have $f(n)=2f((n-1)/2)+1=2((2l_{1})+1)+1=2l+1$, where the second equality follows from the induction hypothesis.
      This completes the proof.
      We can solve for $l$ to get an explicit expression for $f(n)$:
      $f(n)=2(n-2^{{\lfloor \log _{2}(n)\rfloor }})+1$
      The most elegant form of the answer involves the binary representation of size $n$: $f(n)$ can be obtained by a one-bit left cyclic shift of $n$ itself.
      If we represent $n$ in binary as $n=1b_{1}b_{2}b_{3}\dots b_{m}$, then the solution is given by $f(n)=b_{1}b_{2}b_{3}\dots b_{m}1$.
      The proof of this follows from the representation of $n$ as $2^{m}+l$ or from the above expression for $f(n)$.

Implementation:

If $n$ denotes the number of people, the safe position is given by the function $f(n)=2l+1$ ,where $n=2^{m}+l$ and $0\leq l<2^{m}$.

Now if we represent the number in binary format, the first bit denotes $2^{m}$ and remaining bits will denote $l$.

For example, when $n=41$, its binary representation is

$n$ = 1 0 1 0 0 1

$2m$ = 1 0 0 0 0 0

$l$ = 0 1 0 0 1

```C++
/**
     * 
     * @param n the number of people standing in the circle
     * @return the safe position who will survive the execution 
     *   f(N) = 2L + 1 where N =2^M + L and 0 <= L < 2^M
     */
    public int getSafePosition(int n) {
        // find value of L for the equation
        int valueOfL = n - Integer.highestOneBit(n);
        int safePosition = 2 * valueOfL  + 1;
        
        return safePosition;
    }
```

The general case:
The easiest way to solve this problem in the general case is to use dynamic programming by performing the first step and then using the solution of the remaining problem. 
When the index starts from one, then the person at $s$ shifts from the first person is in position $((s-1){\bmod n})+1$, where $n$ is the total number of persons.

Let $f(n,k)$ denote the position of the survivor. After the $k-th$ person is killed, we're left with a circle of $n-1$, and we start the next count with the person whose number in the original problem was $(k{\bmod n})+1$.

The position of the survivor in the remaining circle would $bef(n-1,k)$ if we start counting at $1$;

shifting this to account for the fact that we're starting at $(k{\bmod n})+1$ yields the recurrence

$f(n,k)=((f(n-1,k)+k-1){\bmod n})+1,{\text{ with }}f(1,k)=1$,

which takes the simpler form $g(n,k)=(g(n-1,k)+k){\bmod n},{\text{ with }}g(1,k)=0$

if we number the positions from ${\displaystyle 0} to n-1$ instead.

This approach has running time $O(n)$, but for small $k$ and large $n$ there is another approach.

The second approach also uses dynamic programming but has running time $O(k\log n)$.

It is based on considering killing $k-th, 2k-th, ...,(\lfloor n/k\rfloor k)-th$ people as one step, then changing the numbering.

This improved approach takes the form
$$ {\displaystyle g(n,k)={\begin{cases}0&{\text{if }}n=1\\(g(n-1,k)+k){\bmod {n}}&{\text{if }}1<n<k\\\left\lfloor {\frac {k((g(n',k)-n{\bmod {k}}){\bmod {n}}')}{k-1}}\right\rfloor {\text{where }}n'=n-\left\lfloor {\frac {n}{k}}\right\rfloor &{\text{if }}k\leq n\\\end{cases}}}
$$