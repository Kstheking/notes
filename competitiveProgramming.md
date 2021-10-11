# cp

- atcoder
-  interviewbit 
-  placement doc

#### Max/min obtainable (Binary search ?)

##  game theory
[topcoder article (game of nim and composite games, grundy numbers](https://www.topcoder.com/thrive/articles/Algorithm%20Games) 

### game of nim 
winning state, xor != 0   
losing state, xor == 0

 the Sprague–Grundy theorem states that every impartial game under the normal play convention is equivalent to a one-heap game of nim, or to an infinite generalization of nim.   
 nimbers, also called Grundy numbers

### grundy number 
 a single grundy number can define the entire state and every impartial game has it.   
 grundy of any state = mex of (grundy of all states we can reach from the current one)  
 also equals to 0 for a losing state   

 - a grundy number of a state is the value of one heap game of nim it is equivalent to 
 -  if let's say from a state we can reach to grundy numbers of 0, 1 , 2 it means we can reach heap sizes of 0, 1 , 2 which means the current heap size and therefore the grundy number must be 3, that is why we use the mex 
 -  for independent impartial games together, like k knights on a chessboard which can be placed together, losing state can be xor of all their grundy numbers being equal to 0, and the vice versa 

### nim addition
  : for finite ordinals = xor of all heaps count 

##### [Roxor](https://www.topcoder.com/tc?module=Static&d1=match_editorials&d2=srm216)

#### area of any polygon
[area of any polygon whose coords are known](https://www.mathopenref.com/coordpolygonarea.html)

## Number theory 

### number of prime numbers in a range 
approx n/logn [link](https://en.wikipedia.org/wiki/Prime_number_theorem)

## questions cache 

- [step by step](https://www.interviewbit.com/problems/step-by-step/) so basically given numbers 1 to n you can make any number between 1 and total sum by selecting some numbers from these 
- [make pair, a question on segment dp](https://atcoder.jp/contests/abc217/tasks/abc217_f) 
- [celebrity problem](https://www.geeksforgeeks.org/the-celebrity-problem/)
- [longest common subsequence](https://www.geeksforgeeks.org/longest-common-subsequence-dp-4/) (kinda same ideas as longest palindromic subsequence)
- [longest common substring](https://www.geeksforgeeks.org/longest-common-substring-dp-29/) can be done in O(m*n) the same way as above and also in linear time using suffix structures 
- [kmp](https://github.com/Kstheking/Code/blob/master/String/KMP.cpp)
- [number of bits from 1 to A](https://www.interviewbit.com/problems/count-total-set-bits/)
- [max sum triplet](https://www.interviewbit.com/problems/maximum-sum-triplet/) 
- [maximum absolute difference](https://www.interviewbit.com/problems/maximum-absolute-difference/)
- [longest balanced parantheses](https://www.techiedelight.com/find-length-longest-balanced-parenthesis-string/)
- [next permutation](https://www.geeksforgeeks.org/find-next-greater-number-set-digits/)
- [copy linked list with nxt and random pointer](https://www.geeksforgeeks.org/clone-linked-list-next-random-pointer-o1-space/)
- [minimum steps with i step at every ith turn](https://www.google.com/amp/s/www.geeksforgeeks.org/find-minimum-moves-reach-target-infinite-line/amp/) 
- sum of floor of a/b for all pairs in array (for each number find numbers less than 2*x , 3*x and so on) 
- [check if tiles puzzle solvable](https://www.geeksforgeeks.org/check-instance-15-puzzle-solvable/)
