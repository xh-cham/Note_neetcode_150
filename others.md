# Other Notes

- 當 `std::unordered_map` 的 key 不存在時，直接存取 `myMap[someKey]` 會自動呼叫預設的 constructor 並插入相應的 `pair<>`。
  - 例如：
    ```cpp
    unordered_map<int, int> myMap;
    ++myMap[5];
    ```
  - 當 key `5` 不存在時，會自動插入 `pair<int, int>(5, 0)`（因為 `int` 的預設值為 `0`），然後再將其值加 1，最終結果為 `myMap[5] = 1`。這是一個語法糖。
  - 承上，當需要檢查 key 是否存在時，如果使用 `myMap[someKey] == 0`，會自動插入 `pair<int, int>(someKey, 0)`，因此最好使用 `myMap.find(someKey) == myMap.end()` 來檢查。
- > From the condition `|x - y| ≤ k`, we know every frequency in the final string must lie within some interval of length `k`.
  - 上述提到一個關鍵觀察：差距不超過 `k` 可以轉換成「頻率在某個長度為 `k` 的區間內」。自然可以聯想到 sliding window 或 double pointer。
