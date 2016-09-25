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
[issue from https://www.hackerrank.com/challenges/bon-appetit](https://www.hackerrank.com/challenges/bon-appetit)

>Anna and Brian order n items at a restaurant, but Anna declines to eat any of the ${k}^{th}$ item (where 0 $\le$ k $\lt$ n) ue to an allergy. When the check comes, they decide to split the cost of all the items they shared; however, Brian may have forgotten that they didn't split the $k^{th}$ item and accidentally charged Anna for it.
>
>You are given n, k, the cost of each of the  items, and the total amount of money that Brian charged Anna for her portion of the bill. If the bill is fairly split, print `Bon Appetit` ; otherwise, print the amount of money that Brian must refund to Anna.
>
>**Input Format**
>
> The first line contains two space-separated integers denoting the respective values of  (the number of items ordered) and k (the O-based index of the item that Anna did not eat). 
>
>The second line contains n space-separated integers where each integer i denotes the cost, c[i] of item i (where 0 $$\le$$ i $$\lt$$ n).
>
>The third line contains an integer, b<sub>charged</sub>, denoting the amount of money that Brian charged Anna for her share of the bill.
>
>**Constraints**
>
>+ 2 $$\le$$ n $$\le$$ $$10^5$$
>
>+ 0 $$\le$$ k $$\lt$$ n
>
>+ 0 $$\le$$ c[i] $$\le$$ $$10^4$$
>
>+ 0 $$\le$$ b $$\le$$ $$\sum c[i]$$
>
>**Output**
>If Brian did not overcharge Anna, print `Bon Appetit` on a new line; otherwise, print the difference (i.e., b<sub>charged</sub> - b<sub>actual</sub> ) that Brian must refund to Anna (it is guaranteed that this will always be an integer).
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
>Anna didn't eat item c[1] = 10, but she shared the rest of the items with Brian. The total cost of the shared items is 3 + 2 + 9 = 14 and, split in half, the cost per person is b<sub>actual</sub> = 7. Brian charged her b<sub>charged</sub> = 12 or her portion of the bill, which is more than the 7 dollars worth of food that she actually shared with him. Thus, we print the amount Anna was overcharged, b<sub>charged</sub> - b<sub>actual</sub> = 12 - 7 = 5, on a new line.
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
>Anna didn't eat item c[1] = 10, but she shared the rest of the items with Brian. The total cost of the shared items is 3 + 2 + 9 = 14 and, split in half, the cost per person is b<sub>actual</sub> = 7. Because this matches the amount,  b<sub>charged</sub> = 7 that Brian charged Anna for her portion of the bill, we print `Bon Appetit` on a new line.

**Answer**

```{.cpp}
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {
    int k=0, n=0;
    int c[100000];
    int total = 0, charged = 0;
    int actual = 0;

    int index = 0;
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */  
    scanf("%d %d", &n, &k);
    for(index = 0; index < n; index++) {
      scanf("%d", &c[index]);
      if (index != k) {
        total += c[index];
      }
    }
    scanf("%d", &charged);
    actual = total / 2;

    if (charged == actual) {
      printf("Bon Appetit\n");
    } else {
      printf("%d", (charged - actual));
    }
    
    return 0;
}

```
