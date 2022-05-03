

## Counting Valleys

```cpp
int countingValleys(int steps, string path) {
    int ans = 0;
    int level = 0;
    bool flag = false;
    
    for(const auto& x : path) {
        x == 'U' ? ++level : --level;
        // 只要出山谷，就表示过了一个山谷
        if(x == 'U' && level==0) {
            ++ans;
        }
    }
    
    return ans;
}
```

