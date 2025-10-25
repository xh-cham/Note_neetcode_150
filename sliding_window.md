# Sliding Window

- [上一層](/note_nc150.md)

## Table of Contents

- [Sliding Window](#sliding-window)
  - [Table of Contents](#table-of-contents)
  - [121. Best Time to Buy and Sell Stock](#121-best-time-to-buy-and-sell-stock)
    - [解法 1](#解法-1)
    - [解法 2](#解法-2)
  - [3. Longest Substring Without Repeating Characters](#3-longest-substring-without-repeating-characters)
    - [解法 1](#解法-1-1)
    - [解法 2](#解法-2-1)
  - [424. Longest Repeating Character Replacement](#424-longest-repeating-character-replacement)
    - [解法](#解法)

## 121. Best Time to Buy and Sell Stock

> You are given an integer array `prices` where `prices[i]` is the price of NeetCoin on the `ith` day.
>
> You may choose a **single day** to buy one NeetCoin and choose a **different day in the future** to sell it.
>
> Return the maximum profit you can achieve. You may choose to **not make any transactions**, in which case the profit would be `0`.

### 解法 1

- 從頭 iterate 元素，用 `currMin` 記錄看過的元素中的最小值，用 `currMax` 記錄 `currMin` 右邊的元素中的最大值。
- 當 `currMin` 更新時，`currMax` 更新為 `currMin`
- 當 iterate 到 `prices[i]` 時，如果 `prices[i] > currMax`，則計算利潤；若當前利潤高於之前的最大利潤，則更新最大利潤。
- 關鍵知識點：
  - 觀察暴力法，發現「有可能買」的日子一定是目前出現過的最小值（如果價格一路下降，則不可能會買）
  - iterate 到 `prices[i]` 時的最大利潤代表時間區間為 `[0, i]` 的最大利潤
  - 對於一個固定的買入價格，最好的賣出價格為買入時間**之後**的最大值
  - 因此，固定「買的時間」為目前出現過的最小值，而只要發現今天賣可以賺更多，就更新利潤
- 複雜度：
  - 時間複雜度：$O(n)$，因為只需要一次遍歷。
  - 空間複雜度：$O(1)$，只使用了幾個變數。

### 解法 2

- Sliding window / Two pointers
- 因為買入一定在賣出之前，可以用兩個指針 `left`、`right` 來表示買入和賣出的時間點。
- 根據[解法 1](#解法-1)，`left` 指針指向**目前出現過的最小值**，`right` 指針指向 `left` 之後的目前的最大值。
- 一旦 `right` 指針指向的值小於 `left` 指針指向的值，則更新 `left` 指針為 `right` 指針，代表「買入」時間點更新為目前的最小值。
- 每次移動 `right` 指針時，計算利潤並更新最大利潤。
- 關鍵知識點：
  - `right` 指針每一輪都會往右移動，因此等價於解法 1
- 複雜度同解法 1：
  - 時間複雜度：$O(n)$，因為只需要一次遍歷。
  - 空間複雜度：$O(1)$，只使用了幾個變數。

## 3. Longest Substring Without Repeating Characters

> Given a string `s`, find the _length of the longest substring_ without duplicate characters.
>
> A **substring** is a contiguous sequence of characters within a string.

### 解法 1

- 使用 sliding window 的方式，使用兩個指針 `left` 和 `right` 來表示當前的 substring 範圍。其中 `right` 指向即將被加入 substring 的 char，`left` 指向當前 substring 的起始位置
- 使用一個 `unordered_set` 來記錄當前 substring 中出現過的 char
- 一旦 `right` 指針指向的 char 已經在 substring 中出現過，則移動 `left` 指針，直到該 char 不在 substring 中
- 每次更新 substring 的長度時，更新最大長度
- 關鍵知識點：
  - 範圍內連續資料，想到使用 sliding window
  - 使用 `unordered_set` （hash）來快速檢查是否有重複的 char
- 複雜度：
  - 時間複雜度：$O(n)$，因為每個 char 最多被訪問兩次（一次在 `right`，一次在 `left`）。
  - 空間複雜度：$O(m)$，其中 $m$ 為 substring 中不同 char 的數量，最壞情況為 $O(n)$。

### 解法 2

- 概念同解法 1，但使用 `unordered_map` 來記錄每個 char 的最後出現位置
- 一旦 `right` 指針指向的 char 已經在 substring 中出現過，不再需要使用 while 來逐步移動 `left` 指針，而是直接將 `left` 指針移動到該 char 的最後出現位置的下一個位置
- 但 `left` 指針需要取 `max(left, lastIndex + 1)`，以確保 `left` 不會向左移動
- 關鍵知識點：
  - 使用 `unordered_map` 可以更快地找到重複的 char 的位置
- 複雜度同解法 1：
  - 時間複雜度：$O(n)$
  - 空間複雜度：$O(m)$

## 424. Longest Repeating Character Replacement

> You are given a string `s` consisting of only uppercase english characters and an integer `k`. You can choose up to `k` characters of the string and replace them with any other uppercase English character.
>
> After performing at most `k` replacements, return the length of the longest substring which contains only one distinct character.

### 解法

- 令「主要字符」為數量最多的字符
- 每個 substring 中需要最少的替換次數 = 字串長度 - 主要字符的頻率
- 使用 sliding window 的方式，使用兩個指針 `left` 和 `right` 來表示當前的 substring 範圍（包含）
- 使用一個 map 來記錄當前 substring 中每個字符的頻率
- 每次移動 `right` 指針時，計算最少替換次數。如果最少替換次數大於 `k`，則移動 `left` 指針，直到最少替換次數小於等於 `k`
- 更新最大長度
- 關鍵知識點：
  - 最少的替換次數 = 字串長度 - 主要字符的頻率
  - 使用 sliding window 可以在 O(n) 的時間內解決問題
- 複雜度：
  - 時間複雜度：$O(n)$，因為每個字符最多被訪問兩次（一次在 `right`，一次在 `left`）。
  - 空間複雜度：$O(1)$，因為字符集是固定的（只有 26 個大寫字母），map 的大小不會超過 26。
