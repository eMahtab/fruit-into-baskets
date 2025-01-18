# Fruit into baskets
## https://leetcode.com/problems/fruit-into-baskets
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

1. You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.

2. Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.

3. Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array fruits, return the maximum number of fruits you can pick.
```java
Example 1:

Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.

Example 2:

Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].

Example 3:

Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

## Constraints:
1. 1 <= fruits.length <= 10^5
2. 0 <= fruits[i] < fruits.length

## Implementation : Sliding Window (Exact same as Longest Substring with at most 2 distinct characters)
```java
class Solution {
    public int totalFruit(int[] fruits) {
        if(fruits.length <= 2)
         return fruits.length;

        HashMap<Integer,Integer> treeTypeToIndexMap = new HashMap<>();
        int left = 0;
        int maxFruits = 2;
        for(int i = 0; i < fruits.length; i++) {
            treeTypeToIndexMap.put(fruits[i], i);
            if(treeTypeToIndexMap.size() > 2) {
                int indexOfTreeTypeToRemove = Collections.min(treeTypeToIndexMap.values());
                treeTypeToIndexMap.remove(fruits[indexOfTreeTypeToRemove]);
                left = indexOfTreeTypeToRemove + 1;
            }
            int fruitsCollected = i - left + 1;
            maxFruits = Math.max(maxFruits, fruitsCollected);
        } 
        return maxFruits;
    }
}
```

## Note :
In above implementation, when updating the left pointer, **make sure you are deleting the least recent character.**

It would be wrong to delete the recent occurrence of character pointed by left pointer, character at left may not be the least recent one.

e.g. {1,2,1,2,1,3,3,3,3,3}, **when seeing value 3, we should delete value 2 (because that was the least recent from 3 values in the map), and we should move the left pointer to point to 1.**

So answer in this case would be 6.

## References :
Similar Problem : https://github.com/eMahtab/longest-substring-with-at-most-two-distinct-characters
