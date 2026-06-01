# Section 1: Array Manipulation

## Question 1: Find Maximum Element in an Array

### Problem Statement

Write a function that finds and returns the largest element from an array.

### Solution

```javascript
function findMax(arr) {
  let max = arr[0];

  for (let num of arr) {
    if (num > max) {
      max = num;
    }
  }

  return max;
}

console.log(findMax([10, 5, 20, 8])); // 20
```

### Short Interview Answer

> This solution initializes the first element as the maximum value and iterates through the array. If a larger element is found, it updates the maximum. The time complexity is O(n) and the space complexity is O(1).

---

## Question 2: Find Minimum Element in an Array

### Problem Statement

Write a function that finds and returns the smallest element from an array.

---

### Solution

```javascript
function findMin(arr) {
  let min = arr[0];

  for (let num of arr) {
    if (num < min) {
      min = num;
    }
  }

  return min;
}

console.log(findMin([10, 5, 20, 1])); // 1
```

### Short Interview Answer

> This solution initializes the first element as the minimum value and iterates through the array. If a smaller element is found, it updates the minimum. The time complexity is O(n) and the space complexity is O(1).

---

## Question 3: Reverse an Array

### Problem Statement

Write a function that reverses the elements of an array without using the built-in `reverse()` method.

---

### Solution

```javascript
function reverseArray(arr) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    [arr[left], arr[right]] = [arr[right], arr[left]];
    left++;
    right--;
  }

  return arr;
}

console.log(reverseArray([1, 2, 3, 4]));
```

### Alternative Solution Using Built-in Method

```javascript
const arr = [1, 2, 3, 4];
console.log(arr.reverse());
```

Output:

```javascript
[4, 3, 2, 1];
```

### Short Interview Answer

> This solution uses the two-pointer approach. One pointer starts from the beginning and another from the end of the array. Elements are swapped until both pointers meet. The time complexity is O(n) and the space complexity is O(1).

---

# Section 2: String Manipulation

## Question 1: Reverse a String

### Problem Statement

Write a function to reverse a string without using built-in methods like `reverse()`.

### Solution

```javascript
function reverseString(str) {
  let reversed = "";

  for (let i = str.length - 1; i >= 0; i--) {
    reversed += str[i];
  }

  return reversed;
}

console.log(reverseString("hello")); // olleh
```

### Alternative Two-Pointer Approach

```javascript
function reverseString(str) {
  let chars = [];

  for (let i = 0; i < str.length; i++) {
    chars[i] = str[i];
  }

  let left = 0;
  let right = chars.length - 1;

  while (left < right) {
    let temp = chars[left];
    chars[left] = chars[right];
    chars[right] = temp;

    left++;
    right--;
  }

  let result = "";

  for (let char of chars) {
    result += char;
  }

  return result;
}

console.log(reverseString("hello"));
```

---

### Short Interview Answer

> To reverse a string without using built-in methods, iterate from the last character to the first character and keep appending each character to a new string. The time complexity is O(n) and the space complexity is O(n).

---

## Question 2: Check if a String is a Palindrome

### Problem Statement

Write a function to check whether a given string is a palindrome.

A palindrome is a word that reads the same forward and backward.

#### Examples

```javascript
"madam"  => true
"racecar" => true
"hello" => false
```

---

## Solution

```javascript
function isPalindrome(str) {
  let left = 0;
  let right = str.length - 1;

  while (left < right) {
    if (str[left] !== str[right]) {
      return false;
    }

    left++;
    right--;
  }

  return true;
}

console.log(isPalindrome("madam"));
```

### Short Interview Answer

> A palindrome is a string that reads the same forward and backward. Using the two-pointer approach, we compare characters from both ends of the string. If any pair does not match, we return false; otherwise, the string is a palindrome. The time complexity is O(n) and the space complexity is O(1).

---

## Question 3: Count Vowels in a String

### Problem Statement

Write a function to count the number of vowels in a given string.

Vowels are:

```javascript
(a, e, i, o, u);
```

#### Example

```javascript
"JavaScript" => 3
```

## Solution

```javascript
function countVowels(str) {
  let count = 0;
  let vowels = "aeiou";

  for (let char of str.toLowerCase()) {
    if (vowels.includes(char)) {
      count++;
    }
  }

  return count;
}

console.log(countVowels("JavaScript")); // 3
```

### Short Interview Answer

> This solution iterates through each character of the string and checks whether it is a vowel. If the character is a vowel, the count is incremented. The time complexity is O(n) and the space complexity is O(1).

---

# Section 3: HashMap

## Question 1: Two Sum

### Problem Statement

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to the target.

You may assume that each input has exactly one solution.

#### Example

```javascript
nums = [2, 7, 11, 15];
target = 9;

Output: [0, 1];
```

Because:

```javascript
nums[0] + nums[1] = 2 + 7 = 9
```

## Solution

```javascript
function twoSum(nums, target) {
  let map = {};

  for (let i = 0; i < nums.length; i++) {
    let diff = target - nums[i];

    if (map[diff] !== undefined) {
      return [map[diff], i];
    }

    map[nums[i]] = i;
  }
}

console.log(twoSum([2, 7, 11, 15], 9)); // [0, 1]
```

### Short Interview Answer

> This solution uses a HashMap to store previously seen numbers and their indices. For each number, we calculate the required complement (`target - currentNumber`). If the complement already exists in the HashMap, we return the indices. This approach has a time complexity of O(n) and a space complexity of O(n).

---

## Question 2: Frequency Count

### Problem Statement

Write a function to count the frequency of each character in a string.

The function should return an object where:

- Key = Character
- Value = Number of occurrences

#### Example

```javascript
"apple";
```

Output:

```javascript
{
  a: 1,
  p: 2,
  l: 1,
  e: 1
}
```

## Solution

```javascript
function frequency(str) {
  let map = {};

  for (let char of str) {
    map[char] = (map[char] || 0) + 1;
  }

  return map;
}

console.log(frequency("apple"));
// { a: 1, p: 2, l: 1, e: 1 }
```

### Short Interview Answer

> This solution uses a HashMap (JavaScript object) to store the count of each character. While iterating through the string, if the character already exists, its count is incremented; otherwise, it is initialized to 1. The time complexity is O(n) and the space complexity is O(n).

---

## Question 3: Valid Anagram

### Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An anagram is a word formed by rearranging the letters of another word using all the original letters exactly once.

#### Example

```javascript
s = "listen";
t = "silent";

Output: true;
```

Because both strings contain the same characters with the same frequency.

## Solution

```javascript
function isAnagram(s, t) {
  if (s.length !== t.length) return false;

  let map = {};

  for (let char of s) {
    map[char] = (map[char] || 0) + 1;
  }

  for (let char of t) {
    if (!map[char]) return false;
    map[char]--;
  }

  return true;
}

console.log(isAnagram("listen", "silent")); // true
```

### Short Interview Answer

> This solution uses a HashMap to count the frequency of characters in the first string. Then it iterates through the second string and decreases the corresponding counts. If any character is missing or the counts do not match, it returns false. If all counts become zero, the strings are anagrams. The time complexity is O(n) and the space complexity is O(n).

---

# Section 4: Two Pointer

## Question 1: Sorted Two Sum

### Problem Statement

Given a sorted array of integers and a target value, return the indices of the two numbers whose sum equals the target.

The array is already sorted in ascending order.

#### Example

```javascript
arr = [1, 2, 3, 4, 6];
target = 6;

Output: [1, 3];
```

Because:

```javascript
arr[1] + arr[3] = 2 + 4 = 6
```

## Solution

```javascript
function twoSumSorted(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    let sum = arr[left] + arr[right];

    if (sum === target) {
      return [left, right];
    }

    if (sum < target) {
      left++;
    } else {
      right--;
    }
  }
}

console.log(twoSumSorted([1, 2, 3, 4, 6], 6)); // [1, 3]
```

### Short Interview Answer

> This solution uses the Two Pointer approach. One pointer starts from the beginning of the sorted array and the other from the end. If the current sum is smaller than the target, the left pointer is moved forward. If the sum is larger, the right pointer is moved backward. This achieves a time complexity of O(n) and a space complexity of O(1).

---

## Question 2: Remove Duplicates from Sorted Array

### Problem Statement

Given a sorted array `nums`, remove the duplicates in-place such that each unique element appears only once.

Return the number of unique elements.

#### Example

```javascript
nums = [1, 1, 2, 2, 3, 4, 4];

Output: 4;
```

Unique elements:

```javascript
[1, 2, 3, 4];
```

## Solution

```javascript
function removeDuplicates(nums) {
  let i = 0;

  for (let j = 1; j < nums.length; j++) {
    if (nums[i] !== nums[j]) {
      i++;
      nums[i] = nums[j];
    }
  }

  return i + 1;
}

console.log(removeDuplicates([1, 1, 2, 2, 3, 4, 4])); // 4
```

### Short Interview Answer

> This solution uses the Two Pointer approach. Pointer `i` tracks the position of the last unique element, while pointer `j` scans the array. Whenever a new unique element is found, it is placed at the next position of `i`. The function returns the count of unique elements. The time complexity is O(n) and the space complexity is O(1).

---

# Section 5: Sliding Window

## Question 1: Maximum Sum Subarray of Size K

### Problem Statement

Given an array of integers and a number `k`, find the maximum sum of any contiguous subarray of size `k`.

#### Example

```javascript
arr = [2, 1, 5, 1, 3, 2];
k = 3;

Output: 9;
```

Because:

```javascript
[2, 1, 5] = 8
[1, 5, 1] = 7
[5, 1, 3] = 9
[1, 3, 2] = 6
```

Maximum sum = `9`

## Solution

```javascript
function maxSum(arr, k) {
  let windowSum = 0;

  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let max = windowSum;

  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];
    max = Math.max(max, windowSum);
  }

  return max;
}

console.log(maxSum([2, 1, 5, 1, 3, 2], 3)); // 9
```

### Short Interview Answer

> This solution uses the Sliding Window technique. First, it calculates the sum of the first `k` elements. Then it slides the window by adding the new element and removing the old element from the sum. This avoids recalculating the entire window each time. The time complexity is O(n) and the space complexity is O(1).

---

## Question 2: Longest Substring Without Repeating Characters

### Problem Statement

Given a string `str`, find the length of the longest substring without repeating characters.

A substring is a continuous sequence of characters.

#### Example

```javascript
str = "abcabcbb";

Output: 3;
```

Because:

```javascript
"abc";
```

is the longest substring without repeating characters.

## Solution

```javascript
function lengthOfLongestSubstring(str) {
  let chars = {};

  let left = 0;

  let max = 0;

  for (let right = 0; right < str.length; right++) {
    while (chars[str[right]]) {
      chars[str[left]] = false;
      left++;
    }

    chars[str[right]] = true;

    let currentLength = right - left + 1;

    if (currentLength > max) {
      max = currentLength;
    }
  }

  return max;
}

console.log(lengthOfLongestSubstring("abcabcbb")); // 3
```

### Short Interview Answer

> This solution uses the Sliding Window technique with two pointers. An object is used to track characters currently in the window. When a duplicate character is found, the window is shrunk from the left until the duplicate is removed. The maximum window length found during the traversal is returned. The time complexity is O(n) and the space complexity is O(n).

---

# Section 5: Stack

## Question 1: Valid Parentheses

### Problem Statement

Given a string containing only parentheses `()`, curly braces `{}`, and square brackets `[]`, determine whether the input string is valid.

A string is valid if:

- Open brackets are closed by the same type of brackets.
- Open brackets are closed in the correct order.
- Every closing bracket has a corresponding opening bracket.

#### Example

```javascript
str = "()[]{}";

Output: true;
```

#### Example 2

```javascript
str = "([)]";

Output: false;
```

## Solution

```javascript
function isValid(str) {
  let stack = [];

  let map = {
    ")": "(",
    "}": "{",
    "]": "[",
  };

  for (let char of str) {
    if ("([{".includes(char)) {
      stack.push(char);
    } else {
      if (stack.pop() !== map[char]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}

console.log(isValid("()[]{}")); // true
console.log(isValid("([)]")); // false
```

### Short Interview Answer

> This solution uses a Stack data structure. Opening brackets are pushed onto the stack. When a closing bracket is encountered, the top element of the stack is checked to ensure it matches the corresponding opening bracket. If all brackets are matched correctly and the stack is empty at the end, the string is valid. The time complexity is O(n) and the space complexity is O(n).

---
