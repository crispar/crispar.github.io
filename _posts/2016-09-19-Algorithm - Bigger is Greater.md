---
published: true
title: Algorithm - Bigger is Greater
layout: post
author: jungwook
category: Algorithm
tags:
- Algorithm
- Permutation
---

>Given a word _w_, rearrange the letters of  _w_ to construct another word _s_ in such a way that _s_ is lexicographically greater than _w_. In case of multiple possible answers, find the lexicographically smallest one among them.  
>
>**Input Format**  
>
>$$>1 <= t <= 10^5$$  
>
>$$1 <= |w| <= 100$$
>
> _w_ will contain only lower-case English letters and its length will not exceed _100_.  
>
>**Output Format**  
>For each testcase, output a string lexicographically bigger than  in a separate line. In case of multiple possible answers, print the lexicographically smallest one, and if no answer exists, print `no answer`.  
>  
>**Sample Input**  
>
>```bash  
>5  
>ab  
>bb  
>hefg  
>dhck  
>dkhc  
>```  
>
>**Sample Output**
>
>```bash  
>ba  
>no answer  
>hegf  
>dhkc  
>hcdk  
>```  
>
>**Explanation**  
>
>+ Test case 1:  
>
>There exists only one string greater than `ab` which can be built by rearranging `ab`. That is `ba`.
>
>+ Test case 2:
>Not possible to rearrange `bb` and get a lexicographically greater string.
>
>+ Test case 3: 
>`hegf` is the next string lexicographically greater than `hefg`.
>
>+ Test case 4: 
>`dhkc` is the next string lexicographically greater than `dhck`.
>
>+ Test case 5: 
>`hcdk` is the next string lexicographically greater than `dkhc`.

**Answer**  
```cpp  
#include <stdio.h>  
#include <string.h>  
#include <math.h>  
#include <stdlib.h>  
  
int next_permutation(char *array, int length);  
  
int main() {  
  
    /* Enter your code here. Read input from STDIN. Print output to STDOUT */  
    int T=0, test_case = 0;  
    char letters[100];  
    scanf("%d", &test_case);  
    for ( T = 0; T < test_case; T++) {  
        scanf("%s", letters);  
        if( next_permutation(letters, strlen(letters)) == 1 ) {  
            printf("%s\n", letters);  
        } else {  
            printf("no answer\n");  
        }  
    }  
      
    return 0;  
}  
  
int next_permutation(char *array, int length) {  
	int i, j;  
	char temp;  
	  
	// Find non-increasing suffix  
	if (length == 0)  
		return 0;  
	i = length - 1;  
	while (i > 0 && array[i - 1] >= array[i])  
		i--;  
	if (i == 0)  
		return 0;  
	  
	// Find successor to pivot  
	j = length - 1;  
	while (array[j] <= array[i - 1])  
		j--;  
	temp = array[i - 1];  
	array[i - 1] = array[j];  
	array[j] = temp;  
	  
	// Reverse suffix  
	j = length - 1;  
	while (i < j) {  
		temp = array[i];  
		array[i] = array[j];  
		array[j] = temp;  
		i++;  
		j--;  
	}  
	return 1;  
}  
```  
참고. [Project Nayuki](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm)


