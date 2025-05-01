cccording to the description, if x is a palindromic integer and divisible by k, then x is called a k-palindromic integer  
the question requires finding the number of k-palindromic integers with a digit length of n  
according to the definition of palindromic integers, the sequence of digits on the left side of a palindromic integer is the same as the reverse sequence on the right side  
if the digits on the left side are known, the digits on the right side can be determined  
in the case of a digit length of n, we discuss the following categories:  
  
if n is even, then the first n/2 digits of the left half of the palindromic integer are in the same order as the reversed n/2 digits of the right half  
the range of values for the first n/2 digits of the left half is [0,10^(n/2)), since there cannot be leading zeros, there are a total of 10^(n/2) - 10^((n-2)/2) different palindromic integers  
  
and  
  
if n is odd, then the left half of the palindromic integer has the same sequence as the reverse of the right half for the first (n-1)/2 digits, and the middle digit has a value range of [0,9]  
the direct enumeration of the value range of the first (n+1)/2 digits of the left half of the integer is [0,10^((n+1)/2), since there cannot be leading zeros, there are a total of 10^((n+1)/2) - 10^((n-1)/2) different palindromic integers  
  
from these "conclusions", it can be known that when the length is n, there are a total of 10^⌊(n-1)/2⌋ - 10^⌊(n+1)/2⌋ palindromic integers  
the given range of n is [1,10], and there are at most 10^5 different k-palindromic integers  
therefore, it is possible to enumerate and find all k-palindromic integers. Let m=⌊(n-1)/2⌋, and let base=10^m  
enumerate the left half of the palindromic integer, whose value range is in [base,10×base), to generate a palindromic integer of length n  
at this point, if the palindromic integer is divisible by k, then the palindromic integer is a k-palindromic integer  

according to the description, if the digits of an integer can be rearranged to form a k-palindromic integer, then the integer is called a "good integer"  
that is, if an integer has the same digits as a k-palindromic integer and does not contain leading zeros, then it is a "good integer"  
the problem says finding the number of all "good integers" of length n  
we know that for a k-palindromic integer, any permutation of the characters that do not contain leading zeros can be called a "good integer"  
since all valid k-palindromic integers have been found, the problem now converts to finding the number of different permutation combinations of the given string  

when calculating, since different palindromic integers may consist of the same digits, to avoid redundant calculations, the string of each palindromic integer can be regularized  
the string can be sorted in lexicographical order, which ensures the uniqueness of the same digit characters  
we use the hash map dict to record the sorted strings  
if the sorted string s has appeared in the hash map, it will not be recorded again  
next, consider the problem of permutations and combinations, as the same characters may appear multiple times, which requires consideration of multiple combinations  
assuming the given string of length n has the occurrences of digits '0' to '9' as c0, c1,... c9, and ignoring leading zeros, the number of permutations that can be formed is: n!/[∏(i=0 to 9)ci!]

considering that there cannot be a leading 0, at this point, it is first necessary to select a character that is not '0' from the n characters to place at the first position  
there are n−c0 characters that are not '0'  
the remaining n−1 characters can be arranged arbitrarily, resulting in (n−1)! combinations  
in this case, without considering repeated elements, the number of combination schemes is (n−c0)x(n−1)!  
since some elements are repeated, it is necessary to divide by the permutations of the repeated elements  
therefore, the number of combinations is: ((n-c0)x(n-1)!)/∏(i=0 to 9)ci!  
  
enumerate the valid strings s in the hash map dict, and count the number of occurrences of characters from ‘0’ to ‘9’ in s, and store the counts in the array cnt  
according to cnt, calculate the number of different combinations that s can form, that is, the number of good integers that s can form  
add this to the result ans, and return the final result  
  
the permutation and combination proof is as follows:  
since there are n positions to place n characters, first consider the character '0', as it cannot be placed at the first position, it can only be chosen from the last n−1 positions to place c0 of them, at this time there are (n-1 choose c0) ways  
next consider the character '1', at this time it can be chosen from n−c0 positions to place c1 of them, at this time there are (n-c0 choose c1) ways  
similarly, the number of ways for '2',...,'9' can be derived  
therefore, the total number of ways is:  
```
S=(n-1 choose c0)*(n-c0 choose c1)*...*(n-c0-c1-...-c8 choose c9)  
the expansion of the above formula is as follows:  
S=[(n-1)!/(c0!*(n-1-c0)!)]*[(n-c0)!/(c1!*(n-c0-c1)!)]*...*[(n-c0-c1-...-c8)!/(c9!*(n-c0-c1-...-c9)!)]  
By simplifying the above expression, we can obtain:  
S=[(n-c0)*(n-1)!]/(c0!*c1!*...*c9!)=[(n-c0)*(n-1)!]/∏(i=0 to 9)ci!
```
  
(Implementation)[./challenge3272.cpp]  
  
let n be the given number, m=⌊((n+1)/2)⌋  
time complexity: O(n log n×10^m)  
since there can be at most 10^m k-palindromic integers, it takes O(10^m) time to enumerate all k-palindromic integers  
each k-palindromic integer has n digits, and the digits need to be sorted, which takes O(n log n) time  
calculating the factorial of n takes O(n) time, so the overall time complexity is O(n log n×10^m)  
space complexity: O(n×10^m)  
we need to enumerate all possible k-palindromic integers, there can be at most 10^m k-palindromic integers, each palindrome has n digits, the space required in the hash map is O(n), therefore, the required space is O(n×10^m)  
