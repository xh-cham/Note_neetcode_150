# Two Pointers

## Table of Contents

- [Two Pointers](#two-pointers)
  - [Table of Contents](#table-of-contents)
  - [125. Valid Palindrome](#125-valid-palindrome)
    - [解法](#解法)
  - [167. Two Sum II - Input Array Is Sorted](#167-two-sum-ii---input-array-is-sorted)
    - [解法](#解法-1)
  - [15. 3Sum](#15-3sum)
    - [解法](#解法-2)
  - [11. Container With Most Water](#11-container-with-most-water)
    - [解法](#解法-3)
  - [42. Trapping Rain Water](#42-trapping-rain-water)
    - [解法 1](#解法-1)
    - [解法 2](#解法-2)

## 125. Valid Palindrome

> Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.
>
> A **palindrome** is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.
>
> Note: Alphanumeric characters consist of letters `(A-Z, a-z)` and numbers `(0-9)`.

### 解法

- 使用雙指針從兩端往中間遍歷，忽略 non-alphanumeric characters，並使用 `tolower` 轉換為小寫字母進行比較。
- 關鍵知識點：
  - 使用 `tolower` 轉換成小寫，以及使用 `isalnum` 檢查是否為 alphanumeric characters。
  - 從兩端收斂，想到使用雙指針。
- 複雜度：
  - 時間複雜度：排序需要 $O(n\log n)$，遍歷需要 $O(n)$，因此總時間複雜度為 $O(n \log n)$。
  - 空間複雜度：只有使用一些額外變數， $O(1)$。

## 167. Two Sum II - Input Array Is Sorted

> Given an array of integers `numbers` that is sorted in **non-decreasing order**.
>
> Return the indices (**1-indexed**) of two numbers, `[index1, index2]`, such that they add up to a given `target` number target and `index1 < index2`. Note that `index1` and `index2` cannot be equal, therefore you may not use the same element twice.
>
> There will always be **exactly one valid solution**.
>
> Your solution must use $O(1)$ additional space.

### 解法

- 非常基本的雙指針題。
- 使用雙指針指向兩端，根據兩端指針所指數字的和與 `target` 的比較來調整指針位置。如果和小於 `target`，則左指針右移；如果和大於 `target`，則右指針左移。
- 關鍵知識點：
  - 已排序的問題，想到使用雙指針。
- 複雜度：
  - 時間複雜度：每個元素最多被訪問一次，因此時間複雜度為 $O(n)$，其中 $n$ 為 `numbers` 長度。
  - 空間複雜度：只使用了幾個額外變數，空間複雜度為 $O(1)$。

## 15. 3Sum

> Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.
>
> The output should not contain any duplicate triplets. You may return the output and the triplets in **any order**.

### 解法

- 先對 `nums` 排序，然後使用三個指針：`left`、`right` 和 `target`。`target` 從最大的元素開始往左移動，`left` 指向最小元素，`right` 指向 `target` 的左邊元素。
- 根據三個指針所指元素的和與 0 的比較來調整指針位置。調整方式參考 two sum 的解法。
- 關鍵知識點：
  - 原本 brute force 的複雜度為 $O(n^3)$，考慮使用雙指針降低複雜度。
  - 雙指針與排序是很常見的搭配。
  - 去重：記錄 triplet 後使用 `while` 迴圈來跳過重複的元素。
- 複雜度：
  - 時間複雜度：排序需要 $O(n \log n)$，雙指針遍歷需要 $O(n^2)$，因此總時間複雜度為 $O(n^2)$。
  - 空間複雜度：只使用了幾個額外變數，空間複雜度為 $O(1)$。

## 11. Container With Most Water

> You are given an integer array `heights` where `heights[i]` represents the height of the $i$-th bar.
>
> All pairs of the bars define a rectangle with a height given by the shorter bar and a width given by the distance between the bars. Return the area of the rectangle with the largest area.

### 解法

- 考慮從兩端開始，`left` 指針指向最左邊的元素，`right` 指針指向最右邊的元素。計算區間面積後，移動指向較小元素的指針。題目所求為最大面積，因此希望端點的高度越大越好。當找到大的端點時，移動較小的端點指針，才可能找到更大的面積。
- 關鍵知識點：
  - 找兩個端點的題目，想到使用雙指針。
  - 位置資訊需要保留，因此不考慮排序。
  - sliding window （兩個指針都從某一端開始）的概念較不適合。這種情況下當 `right` 指針遍歷所有元素，`left` 指針指向的元素需為當前最佳解，並且 `left` 周圍的元素要是次佳解。
  - 換句話說，解的 goodness 需要有一定程度的空間相關性？
- 複雜度：
  - 時間複雜度：每個元素最多被訪問一次，因此時間複雜度為 $O(n)$，其中 $n$ 為 `heights` 長度。
  - 空間複雜度：只使用了幾個額外變數，空間複雜度為 $O(1)$。

## 42. Trapping Rain Water

> You are given an array of non-negative integers `height` which represent an elevation map. Each value `height[i]` represents the height of a bar, which has a width of `1`.
>
> Return the maximum area of water that can be trapped between the bars.

### 解法 1

- 原本考慮使用 sliding window（ `left` 從 0 開始，`right` 從 `left + 1` 開始），但光靠 `left` 指針遍歷過的元素沒辦法確定 `left` 指針是否為有效的解。
- 換個角度思考，每個 index 上的水量是由左邊的最大高度和右邊的最大高度所決定的。準確來說，由這兩個值中的最小值決定。
- 因此可以使用 **prefix maximum** 和 **suffix maximum** 的概念，記錄每個 index 左邊和右邊的最大高度。
- 如此一來，對於每個 index `i`，水量可以計算為 `min(prefixMax[i], suffixMax[i]) - height[i]`。
- 關鍵知識點：
  - sliding window 需要能夠根據目前資訊決定是否為最佳解。
- 複雜度：
  - 時間複雜度：計算 prefix 和 suffix 的時間為 $O(n)$，遍歷計算水量的時間為 $O(n)$，因此總時間複雜度為 $O(n)$。
  - 空間複雜度：需要額外的兩個數組來儲存 prefix 和 suffix 的最大高度，因此空間複雜度為 $O(n)$。

### 解法 2

- 使用雙指針 `left` 和 `right`，分別指向最左邊和最右邊的元素。用變數 `leftMax` 和 `rightMax` 分別記錄左邊和右邊的最大高度。
- `leftMax` 代表 `left` 指針以左（不包含）最大高度。同理，`rightMax` 代表 `right` 指針以右（不包含）最大高度。
- 根據 `height[left]` 和 `height[right]` 的大小來決定移動哪個指針。如果 `height[left]` 小於等於 `height[right]`，則移動 `left` 指針，否則移動 `right` 指針。
- 移動之前判斷 `leftMax` 或 `rightMax` 是否需要更新。只有當不需要更新時，才計算水量。
- 解釋如下：

  - 如果 `height[left]` 小於等於 `height[right]`，因為 `height[left] <= height[right] <= rightMax`，則該位置的水量由 `leftMax` 與 `height[left]` 的差值決定。不管右邊的高度如何，`left` 指針所指的水量都不受右邊影響。
  - 承上，如果 `height[left]` 大於 `leftMax`，則該位置的水量為 0；反之，則水量為 `leftMax - height[left]`。
  - `height[left]` 大於 `height[right]` 時同理。
  <details>
  <summary>C++ 程式碼</summary>

  ```cpp
  class Solution {
  public:
      int trap(vector<int>& height) {
          const int N = height.size();
          int left = 0, right = N - 1;
          int leftMax = 0, rightMax = 0;
          int res = 0;
          while (left < right) {
              if (height[left] <= height[right]) {
                  if (height[left] > leftMax) leftMax = height[left];
                  else res += leftMax - height[left];
                  ++left;
              }
              else { // height[left] > height[right]
                  if (height[right] > rightMax) rightMax = height[right];
                  else res += rightMax - height[right];
                  --right;
              }
          }
          return res;
      }
  };

  ```

  </details>

- 關鍵知識點：
  - 使用雙指針來遍歷，並根據兩端的高度決定移動哪個指針。
  - 儲水問題通常需要考慮最大高度/較小高度的概念，因此常用雙指針、prefix/suffix maximum 等技巧。
  -
- 複雜度：
  - 時間複雜度：每個元素最多被訪問一次，因此時間複雜度為 $O(n)$，其中 $n$ 為 `height` 長度。
  - 空間複雜度：只使用了幾個額外變數，空間複雜度為 $O(1)$。
