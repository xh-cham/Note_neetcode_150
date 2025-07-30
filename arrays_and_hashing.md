# Arrays and Hashing

- [上一層](/note_nc150.md)

## Table of Contents

- [Arrays and Hashing](#arrays-and-hashing)
  - [Table of Contents](#table-of-contents)
  - [217. Contains Duplicate](#217-contains-duplicate)
    - [解法](#解法)
  - [242. Valid Anagram](#242-valid-anagram)
    - [解法](#解法-1)
  - [1. Two Sum](#1-two-sum)
    - [解法](#解法-2)
  - [49. Group Anagrams](#49-group-anagrams)
    - [解法](#解法-3)
  - [347. Top K Frequent Elements](#347-top-k-frequent-elements)
    - [解法 1](#解法-1)
    - [解法 2](#解法-2)
  - [271. Encode and Decode Strings](#271-encode-and-decode-strings)
    - [解法 1](#解法-1-1)
    - [解法 2](#解法-2-1)
  - [238. Product of Array Except Self](#238-product-of-array-except-self)
    - [解法](#解法-4)
  - [36. Valid Sudoku](#36-valid-sudoku)
    - [解法](#解法-5)
  - [128. Longest Consecutive Sequence](#128-longest-consecutive-sequence)
    - [解法 1](#解法-1-2)
    - [解法 2](#解法-2-2)

## 217. Contains Duplicate

> Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

### 解法

- 用 hash set 來儲存已經出現過的數字，並將新遇到的數字與出現過的比較，一旦發現重複就返回 `true`，否則遍歷完後返回 `false`。
- 關鍵知識點：
  - **hash set** 查找只需 $O(1)$ 時間，適合用來檢查**重複元素**。
- 複雜度：
  - 最多只需遍歷整個 array 一次，時間複雜度為 $O(n)$。
  - 需要 $O(n)$ 的額外空間來存儲 hash set，其中 $n$ 為 array 長度。

## 242. Valid Anagram

> Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.
>
> An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

### 解法

- 使用 hash map 計算每個字母的出現頻率，比較兩個字串對應的頻率是否相同。
- 進階解法：同時遍歷兩個字串，同時減去 s 中的字母頻率，加上 t 中的字母頻率，最後檢查是否所有字母的頻率都為 0。
- 關鍵知識點：
  - **hash map** 的查找和更新操作時間複雜度為 $O(1)$。
  - 基於上述的原因，hash map 適合用來實現隨時需要查找的頻率記錄。
- 複雜度：
  - 需檢查所有字母，時間複雜度為 $O(n)$，其中 $n$ 為字串長度。
  - 儲存 26 個字母的頻率，空間複雜度為 $O(1)$。

## 1. Two Sum

> Given an array of integers `nums` and an integer `target`, return the indices `i` and `j` such that `nums[i] + nums[j] == target` and `i != j`.
>
> You may assume that every input has exactly one pair of indices `i` and `j` that satisfy the condition.
>
> Return the answer with the smaller index first.

### 解法

- 使用 hash map 儲存每個數字與 target 的差值。遍歷 `nums`，如果當前數字與 `target` 的差值存在於 hash map 中，則回傳 `i` 與 `j`。
- 進階解法：遍歷 `nums` 的同時更新 hash map。
- 關鍵知識點：
  - 需要隨時檢查歷史紀錄，因此聯想到使用 hash map。
- 複雜度：
  - 時間複雜度為 $O(n)$，其中 $n$ 為 `nums` 長度。
  - 空間複雜度為 $O(n)$。

## 49. Group Anagrams

> Given an array of strings `strs`, group all anagrams together into sublists. You may return the output in any order.
>
> An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

### 解法

- 使用 hash map 將字串對應的字母頻率作為 key，字串作為 value，便可將字串分組。
- 難點：一般的 `std::vector<int>` 不能作為 hash map 的 key，因此考慮實現一個自定義的 hash function。另外一個做法是將字母頻率轉換為字串作為 key，例如 `aabb` 轉換為 `,2 , 2, 0, 0, ..., 0`。
- 關鍵知識點：
  - 因為要反覆查找字母的頻率，想到用 hash map。
  - hash map 的 key 可以是自定義類型，但需要實現相應的 hash function，或是轉換成 hash-able 的類型。
- 複雜度：
  - 時間複雜度：計算字串的字母頻率需要 $O(n) + O(26) = O(n)$，因此遍歷所有字串的時間複雜度為 $O(mn)$，其中 $m$ 為字串數量，$n$ 為平均/最大字串長度。
  - 空間複雜度：儲存所有字串的頻率，需要 $O(26 m ) = O(m)$ 的空間。

## 347. Top K Frequent Elements

> Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

### 解法 1

- 使用 **min-heap** 儲存頻率最高的 k 個元素。先遍歷 `nums` 的前 k 個元素，將它們加入 min-heap （儲存 `int` 而非 `pair`）。然後遍歷剩餘的元素，如果當前元素的頻率大於 heap 中最小的頻率，則替換掉最小的元素。
- 關鍵知識點：
  - 要選最大的 k 個元素，想到使用 heap。但實際上，遍歷過程中要反覆跟目前的 k 個元素中最小的比較，因此使用 min-heap。
  - `std::vector<int>` 配合 `std::make_heap()`, `std::pop_heap()` 以及匿名的比較函式可以實現 min-heap。
  - 匿名函式：
    1. ```cpp
       [this](int& a, int& b) {
           return freq[a] > freq[b];
        }
       ```
    2. `this` 指向當前物件，因此 `freq` 需作為 `private` 的 member。
    3. min-heap 的比較函式會用來判斷 **a 比 b 重要**，其中重要代表靠近底部（比較晚被 pop 出來），因此 min-heap 的比較函式需要回傳 `freq[a] > freq[b]`。
- 複雜度：
  - 時間複雜度：遍歷 `nums` 的時間為 $O(n)$，插入 heap 的時間為 $O(\log k)$，因此總時間複雜度為 $O(n \log k)$。
  - 空間複雜度：儲存頻率的 hash map 需要 $O(n)$，而 min-heap 需要 $O(k)$，因此總空間複雜度為 $O(n + k)$。

### 解法 2

- 一樣使用 hash map 計算頻率（數字 -> 頻率），計算完之後建立「頻率 -> 數字」的表，再從最高的可能頻率開始遍歷，將頻率對應的數字加入結果中。
  - 該表可以用 `std::vector<std::vector<int>>` 或 `std::unordered_map<int, int>`，`std::unordered_map` 對稀疏的頻率表更有效率。
- 關鍵知識點：
  - 因為最高的頻率一定不會超過 `n`，因此從最高頻率開始遍歷不會增加時間複雜度。
  - 第二步驟的建表相當於對頻率進行特殊的 bucket sort or counting sort。
- 複雜度：
  - 時間複雜度：建立 hash map 的時間為 $O(n)$，建立頻率表的時間為 $O(n)$，遍歷頻率表的時間為 $O(n)$，因此總時間複雜度為 $O(n)$。
  - 空間複雜度：儲存頻率的 hash map 需要 $O(n)$，而頻率表需要 $O(n)$，因此總空間複雜度為 $O(n)$。

## 271. Encode and Decode Strings

> Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.
>
> Please implement `encode` and `decode`

### 解法 1

- 把長度資訊以及字串連接起來，並且因為長度限制在 100 以內，因此可以用 8-bit 二進位數字來表示長度。因此，每個字串都會變成例如 `"00000101hello"` 的形式，其中 `"00000101"` 是長度 5 的二進位表示，`"hello"` 是字串內容。
- decode 的時候用 `std::substr()` 來提取長度資訊，並用 `idx` 儲存目前的 index，每次提取 8-bit 長度資訊，然後再移動 `idx` 到下一個字串的起始位置。
- 複雜度：
  - 時間複雜度：編碼需要遍歷所有字串，因此時間複雜度為 $O(m)$，其中 m 為所有字串的總長度。解碼則需要遍歷所有字串，因此時間複雜度為 $O(n)$，其中 n 為原本輸入的字串個數。
  - 空間複雜度：編碼需要 $O(m+n)$ 的空間儲存編碼後字串，解碼則需要 $O(n)$ 的空間儲存解碼後的字串，因此總空間複雜度為 $O(m+n)$。

### 解法 2

- 使用特殊的分隔符號來分隔每個字串，例如 `"#"` 或 `","`。`"hello"` 會變成 `"5#hello"`，其中 `5` 是字串長度，`#` 是分隔符號。
- 複雜度同上

## 238. Product of Array Except Self

> Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
>
> The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.
>
> You must write an algorithm that runs in `O(n)` time and without using the division operation.

### 解法

- 在不使用除法的情況下，可以分別計算 prefix 和 suffix 的乘積。因為不包含第 `i` 個元素的總乘積相當於 `[0...i-1]` 的乘積乘以 `[i+1...n-1]` 的乘積。最直觀的做法是使用兩個 array 分別儲存 prefix 和 suffix 的乘積，然後再將兩個 array 相乘得到結果。更節省空間的做法是，只在一個 array 中儲存 prefix 的乘積，然後逐步更新 suffix 的乘積並與 prefix 的乘積相乘。最少需要遍歷兩次。
- 關鍵知識點：
  - 遍歷 array 的方向除了從 0 到 n-1 之外，還可以從 n-1 到 0。
  - 乘積/總和需注意 overflow 的問題。
- 複雜度：
  - 時間複雜度：需要遍歷兩次 array，因此時間複雜度為 $O(n)$，其中 $n$ 為 `nums` 的長度。
  - 空間複雜度：無論使用一個或兩個 array 儲存當前的乘積，皆需要 $O(n)$ 的空間。

## 36. Valid Sudoku

> You are given a `9 x 9` Sudoku board `board`. A Sudoku board is valid if the following rules are followed:
>
> 1. Each row must contain the digits `1-9` without duplicates.
> 1. Each column must contain the digits `1-9` without duplicates.
> 1. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without duplicates.
>
> Return `true` if the Sudoku board is valid, otherwise return `false`
>
> Note: A board does not need to be full or be solvable to be valid.

### 解法

- 使用 hash 或 array 儲存每一列、每一行和每個 block 的數字出現情況。遍歷 `board` 時，檢查對應的列、行和 block 是否已經出現過相同的數字。若無則將數字加入對應的 hash 或 array 中。
- 關鍵知識點：
  - 需要快速查找，想到使用 hash 或 array。因為數字是連續的 `1-9`，可以使用 array 來更有效率地儲存出現情況。
  - 可以使用 `std::bitset<9>` 或 `std::array<bool, 9>` ，更節省空間。
- 複雜度：
  - 時間複雜度：遍歷 `board` 的時間為 $O(n^2)$，其中 $n$ 為 `board` 的列數/行數，本題中為 9。
  - 空間複雜度：儲存每一列、每一行和每個 block 的數字出現情況皆需要 $O(n^2)$ 的空間。若將 `std::bitset<9>` 佔用空間當成常數，則空間複雜度為 $O(n)$。

## 128. Longest Consecutive Sequence

> Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
>
> A consecutive sequence is a sequence of elements in which each element is exactly `1` greater than the previous element. The elements do not have to be consecutive in the original array.
>
> You must write an algorithm that runs in `O(n)` time.

### 解法 1

- 第一次遍歷中，使用 hash map (`int` -> `bool`)記錄出現過的所有數字。第二次遍歷中，對每個數字使用 while loop 檢查與其相鄰的數字是否存在，若存在則繼續向前/向後遍歷，直到遇到不存在的數字為止，並更新目前的序列長度，同時把 hash map 中的數字對應的值設為 `false`。每遍歷一個數字就會找到一條新的連續的序列，因此需要更新一次最大長度。
- 關鍵知識點：
  - 需要快速查找數字是否存在，想到使用 hash map。
  - 類似 flood fill 演算法或 DFS 的遍歷方式。
  - 使用 hash map 去重（de-duplication）。
  - 使用 `unordered_map<int, bool>` 來儲存數字是否已經被遍歷過。
- 複雜度：
  - 時間複雜度：兩次遍歷的時間皆為 $O(n)$，其中 $n$ 為 `nums` 的長度。在第二次遍歷中，每個數字只會被看過一次，因此整體時間複雜度仍為 $O(n)$。
  - 空間複雜度：儲存 hash map 需要 $O(n)$ 的空間。

### 解法 2

- 使用 hash map (`int` -> `int`)記錄每個數字**第一次被遍歷時**，所在序列的當前長度。遍歷 `nums` 時，對每個數字 `num`，檢查 `num-1` 和 `num+1` 對應到的值（如果不存在 hash map 中則為 0），並相加得到當前數字所在序列的當前長度。最後要更新序列的頭跟尾，因為這個做法只檢查每個數字的前後兩個數字，因此更新頭尾即可。
- 例如：已經看過 `1`、`4` 和 `2`，現在看到 `3`，則 `3` 對應的長度為 `2 + 1 + 1 = 4`，然後更新 `1` 和 `4` 對應的長度為 `4`。如此一來，如果遇到 `0` 或 `5`，照著操作即可更新序列長度。
- 關鍵知識點：
  - 因為只有**第一次被遍歷時**才有新資訊，因此在每次遍歷時檢查 hash map 中是否已經存在該數字的長度。這個步驟剛好可以用來 de-dupe。
  - buttom-up 合併計算總和的概念。有點類似 disjoint set 的 union 操作。
- 複雜度：
  - 時間複雜度：每個元素最多被遍歷一次，因此時間複雜度為 $O(n)$。
  - 空間複雜度：同上。
