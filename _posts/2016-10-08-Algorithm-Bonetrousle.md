---
published: true
title: Algorithm - Bonetrousle
layout: post
author: jungwook
category: Algorithm
tags:
- Algorithm
- Loops
---
[issue from https://www.hackerrank.com/challenges/bonetrousle](https://www.hackerrank.com/challenges/bonetrousle)

>Here\`s a humerus joke:
>
>>Why did Papyrus the skeleton go to the store by himself? Because he had no body to go with him!
>
>Did you like it? Don\`t worry, I\`ve got a ton more. A skele-ton.
>
>Once upon a time, Papyrus the skeleton went to buy some pasta from the store. The store`s inventory is bare-bones and they only sell one thing — boxes of uncooked spaghetti! The store always stocks exactly *k* boxes of pasta, and each box is numbered sequentially from *1* to *k*. This box number also corresponds to the number of sticks of spaghetti in the box, meaning the first box contains  *1* stick, the second box contains *2* sticks, the third box contains *3* sticks, …, and the $k^{th}$ box contains *k* sticks. Because they only stock one box of each kind, the store has a tendon-cy to sell out of spaghetti.
>
>During each trip to the store, Papyrus likes to buy exactly *n* sticks of spaghetti by purchasing exactly *b* boxes (no more, no less). Not sure which boxes to purchase, Papyrus calls Sherlock Bones for help but he\`s also stumped! Do you have the guts to solve this puzzle?
>
>Given the values of n, k, and  b for t trips to the store, determine which boxes Papyrus must purchase during each trip. For each trip, print a single line of b distinct space-separated integers denoting the box number for each box of spaghetti Papyrus purchases (recall that the store only has one box of each kind). If it\`\s not possible to buy n sticks of spaghetti by purchasing b boxes, print `-1` instead.
>
>**Input Format**
>
> The first line contains a single integer, t, denoting the number of trips to the store. 
Each of the t subsequent lines describes a trip to the store in the form of three space-separated integers describing the respective values of n (the number of sticks to buy), k (the number of boxes the store has for sale), and b (the number of boxes to buy) for that trip to the store. 
>
>**Constraints**
>
>+ 1 $$\le$$ t $$\le$$ $$20$$
>
>+ 1 $$\le$$ b $$\le$$ $$10^5$$
>
>+ 1 $$\le$$ n $$\le$$ $$10^5$$
>
>+ 1 $$\le$$ k $$\le$$ $$10^5$$
>
>+ b $$\le$$ k
>
>**Output**
>
>For each trip to the store:
>
>+ If there is no solution, print `-1` on a new line.
>
>+ Otherwise, print a single line of b distinct space-separated integers where each integer denotes the box number (i.e., the number of spaghetti sticks in the box) that Papyrus must purchase.
>
>If there are multiple possible solutions, you can print any one of them. Do not print any leading or trailing spaces.
>
>**Sample Input 0**
>
>```bash
>4
>12 8 3
>10 3 3
>9 10 2
>9 10 2
>```
>
>**Sample Output 0**
>
>```bash
>2 3 7
>-1
>5 4
>1 8
>```
>
>**Explanation**
>
>Papyrus makes the following trips to the store:
>
>1. He wants to buy exactly b = 3 boxes of spaghetti and have a total number of n = 12 sticks. During this trip, the store has k = 8 boxes of spaghetti sticks where the first box has 1 stick, the second box has 2 sticks, the third box has 3 sticks, and so on. One possible solution would be the following: Papyrus can buy the 2-stick, 3-stick, and 7-stick boxes for the total of 2 + 3+ 7 = 12 sticks. Note that this is not the only valid solution; other valid solutions are acceptable.
>
>2. He wants to buy exactly b = 3 boxes of spaghetti and have a total number of  sticks. Because the store only has three boxes in stock containing 1, 2, and 3 sticks of spaghetti, it\`s not possible for Papyrus to buy sticks of spaghetti as buying all three boxes would only yield 1 + 2 + 3 = 6 sticks (which is less than the n = 10 that he wanted to purchase). Thus, we print `-1` on a new line.
>
>The third and fourth trips to the store both contain the same values (n = 9,k = 10,b = 2); this is simply to illustrate that there may be multiple solutions for any given trip to the store and any valid solution is acceptable.
>

**Answer**

Editorial 내용을 참고하면, 다음과 같다.

주어지는 값은 주어진 셋으로 생성할 수 있는 최대 혹은 최소 값 사이에 있어야한다. 그렇지 않은 경우에는 주어진 셋으로 해당값을 만들 수 없는 상태이므로 -1을 반환한다.

```{.cpp}
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int main() {
    int T, test_case;
    long long int n, k, b;
    long long minsum = 0, maxsum = 0;
    long long sum;
    int offset;
    int index = 0;
    long long value = 0;
    
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */
    scanf("%d", &T);
    for(test_case = 0; test_case < T; test_case++) {
        scanf("%lld %lld %lld", &n, &k, &b);
        minsum = 0;
        maxsum = 0;
        for (index = 0; index < b; index++) { //앞에서 부터 순차적으로 값을 더한 최소값을 구한다.
            minsum += (index+1);
        }
        for (index = b; index > 0; index--) { //뒤에서 부터 순차적으로 값을 더한 최대값을 구한다.
            maxsum += (k - b + 1);
        }
        //printf("minsum = %lld, maxsum = %lld\n", minsum, maxsum);
        if (minsum <= n && n <= maxsum) { //구하고자 하는 값이 최소/최대의 범위를 넘어가면 구할수 없는 수가 된다.
            sum = minsum;
            offset= 0;
            for (index = b; index > 0; index--) {//구할 수 있는 경우라면, 최소 값에서 값을 하나씩 변경가능한 최대값으로 변경해준다.
                if(sum - index + (k - offset) >= n) {
                    printf("%lld", n-(sum - index));
                    for(value = 1; value < b-offset; value++) {
                        printf(" %lld", value);
                    }
                    for(value = k-offset+1; value <= k; value++) {
                        printf(" %lld", value);                       
                    }
                    printf("\n");
                    break;
                } else {
                    sum = sum - index;
                    sum = sum + (k - offset);
                    offset++;
                }
            }
            
        } else {
            printf("-1\n");
        }
        
    }
    
    return 0;
}
```

결과적으로 해당 답은 다음과 같이 구할 수 있다.

1. 구하고자 하는 값 n이 해당 셋에서 구할 수 있는 최소/최대 범위 내에 있는지 확인한다.
2. 범위 내에 있다면, 현재 값중에서 하나씩 최대 값으로 변경하면서 구하고자 하는 n개의 값에 근접하는지 확인한다.
3. 근접 한값을 구했으면 현재 값과 구하고자 하는 값의 차를 계산하여 필요한 값을 구하고 나머지는 현재값을 그대로 찍어준다.
