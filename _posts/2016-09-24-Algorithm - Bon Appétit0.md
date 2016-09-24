---
published: true
title: Algorithm - Bon Appétit
layout: post
author: jungwook
category: Algorithm
tags:
- Algorithm
- Permutation
---
{% if page.includeMathJax == true %}
   <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
{% endif %}

[issue from https://www.hackerrank.com/challenges/bon-appetit](https://www.hackerrank.com/challenges/bon-appetit)

>Anna and Brian order n items at a restaurant, but Anna declines to eat any of the ${k}^{th}$ item (where 0 $\le$ k $\lt$ n) ue to an allergy. When the check comes, they decide to split the cost of all the items they shared; however, Brian may have forgotten that they didn't split the $k^{th}$ item and accidentally charged Anna for it.
>
>You are given n, k, the cost of each of the  items, and the total amount of money that Brian charged Anna for her portion of the bill. If the bill is fairly split, print `Bon Appetit` ; otherwise, print the amount of money that Brian must refund to Anna.
>
>**Input Format**
>
> The first line contains two space-separated integers denoting the respective values of  (the number of items ordered) and k (the O-based index of the item that Anna did not eat). 
>
>The second line contains n space-separated integers where each integer i denotes the cost, c[i] of item i (where 0 $\le$ i $\lt$ n).
>
>The third line contains an integer, b~charged~, denoting the amount of money that Brian charged Anna for her share of the bill.
>
>**Constraints**
>
>+ 2 $\le$ n $\le$ $10^5$
>
>+ 0 $\le$ k $\lt$ n
>
>+ 0 $\le$ c[i] $\le$ $10^4$
>
>+ 0 $\le$ b $\le$ $\sum c[i]$
>
>**Output**
>If Brian did not overcharge Anna, print `Bon Appetit` on a new line; otherwise, print the difference (i.e., b~charged~ - b~actual~ ) that Brian must refund to Anna (it is guaranteed that this will always be an integer).
>
>**Sample Input 0**
>
>```bash
>4 1
>3 10 2 9
>12
>```
>
>**Sample Output 0**
>
>```bash
>5
>```
>
>**Explanation 0**
>
>Anna didn't eat item c[1] = 10, but she shared the rest of the items with Brian. The total cost of the shared items is 3 + 2 + 9 = 14 and, split in half, the cost per person is b~actual~ = 7. Brian charged her b~charged~ = 12 or her portion of the bill, which is more than the 7 dollars worth of food that she actually shared with him. Thus, we print the amount Anna was overcharged, b~charged~ - b~actual~ = 12 - 7 = 5, on a new line.
>
>**Sample Input 1**
>
>```bash
>4 1
>3 10 2 9
>7
>```
>
>**Sample Output 0**
>
>```bash
>Bon Appetit
>```
>
>**Explanation 0**
>
>Anna didn't eat item c[1] = 10, but she shared the rest of the items with Brian. The total cost of the shared items is 3 + 2 + 9 = 14 and, split in half, the cost per person is b~actual~ = 7. Because this matches the amount,  b~charged~ = 7 that Brian charged Anna for her portion of the bill, we print `Bon Appetit` on a new line.



참고. [Project Nayuki](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm)



