

## 几种常见的排序算法

### 冒泡排序

```cpp
void bubbleSort(int* arr, int len) {
    // 从后向前冒泡，从小到大排序
    /*for (int i = 0; i < len; ++i) {
        for (int j = len - 1; j > i; --j) {
            if (arr[j - 1] > arr[j]) {
                int temp = arr[j - 1];
                arr[j - 1] = arr[j];
                arr[j] = temp;
            }
        }
    }*/

    // 从前向后冒泡，从小到大排序
    for (int i = len - 1; i > 0; --i) {
        for (int j = 0; j < i; ++j) {
            if (arr[j + 1] < arr[j]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

