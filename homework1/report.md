# 41043236 41043257

作業一

## 解題說明

實作四種排序法，包括 Insertion Sort、Quick Sort、Merge Sort、Heap Sort。
每種排序法需有排序前、排序後的資料，且使用者可自行插入數字後再次排序。

### 解題策略

1.	Insertion Sort(插入排序)：一個元素從未排序的部分找到適當的位置，並插入到已排序的部分，直到所有元素都完成排序。
2.	Quick Sort(快速排序)：數列分割成左右兩部分，左部分的元素小於等於基準元素，右部分的元素大於等於基準元素，然後對每個子部分再進行同樣的操作。
3.	Merge Sort(合併排序)：將數列分割成兩個子數列，對每個子數列遞迴地進行排序，然後將兩個排序好的子數列合併起來。	
4.	Heap Sort(堆積排序)：將數列轉換成最大堆，然後反覆從頂部取出最大元素，並將堆的大小減少，直到排序完成。

## 程式實作

Insertion Sort 程式碼：

```cpp
#include <iostream>
#include <vector>
#include <random>
using namespace std;

void insertionSort(vector<int>& arr) {
    size_t n = arr.size();
    for (size_t i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr;
    vector<int> originalArr;
    int numCount;
    cout << "請輸入陣列大小:";
    cin >> numCount;
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dis(1, 100);
    for (int i = 0; i < numCount; i++) {
        int num = dis(gen);
        arr.push_back(num);
        originalArr.push_back(num);
    }

    cout << "未排序前: ";
    for (int num : originalArr) {
        cout << num << " ";
    }

    insertionSort(arr);

    cout << "\n排序後: ";
    for (int num : arr) {
        cout << num << " ";
    }
    int n;
    cout << "\n請輸入要插入的數字: ";
    cin >> n;
    arr.push_back(n);
    insertionSort(arr);
    cout << "排序後: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;
    return 0;
}
```
Quick Sort 程式碼:

```cpp
#include <iostream>
#include <vector>
#include <random>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; ++j) {
        if (arr[j] < pivot) {
            ++i;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    vector<int> arr;
    vector<int> originalArr;
    int numCount;

    cout << "請輸入陣列大小: ";
    cin >> numCount;

    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dis(1, 100);

    for (int i = 0; i < numCount; i++) {
        int num = dis(gen);
        arr.push_back(num);
        originalArr.push_back(num);
    }

    cout << "未排序前: ";
    for (int num : originalArr) {
        cout << num << " ";
    }

    quickSort(arr, 0, arr.size() - 1);

    cout << "\n排序後: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    int newNum;
    cout << "請輸入要插入的數字: ";
    cin >> newNum;

    arr.push_back(newNum);
    quickSort(arr, 0, arr.size() - 1);

    cout << "插入後重新排序結果: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```

Merge Sort 程式碼:

```cpp
#include <iostream>
#include <vector>
#include <random>
using namespace std;

void merge(vector<int>& arr, int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;

    vector<int> L(n1), R(n2);

    for (int i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        }
        else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r)
        return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}

int main() {
    vector<int> arr;
    vector<int> originalArr;
    int numCount;

    cout << "請輸入陣列大小: ";
    cin >> numCount;

    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dis(1, 100);

    for (int i = 0; i < numCount; i++) {
        int num = dis(gen);
        arr.push_back(num);
        originalArr.push_back(num);
    }

    cout << "未排序前: ";
    for (int num : originalArr) {
        cout << num << " ";
    }

    mergeSort(arr, 0, arr.size() - 1);

    cout << "\n排序後: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    int newNum;
    cout << "請輸入要插入的數字: ";
    cin >> newNum;

    arr.push_back(newNum);
    mergeSort(arr, 0, arr.size() - 1);

    cout << "插入後重新排序結果: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```

Heap Sort 程式碼:

```cpp
#include <iostream>
#include <vector>
#include <random>
using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    vector<int> arr;
    vector<int> originalArr;
    int numCount;

    cout << "請輸入陣列大小: ";
    cin >> numCount;

    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dis(1, 100);

    for (int i = 0; i < numCount; i++) {
        int num = dis(gen);
        arr.push_back(num);
        originalArr.push_back(num);
    }

    cout << "未排序前: ";
    for (int num : originalArr) {
        cout << num << " ";
    }

    heapSort(arr);

    cout << "\n排序後: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    int newNum;
    cout << "請輸入要插入的數字: ";
    cin >> newNum;

    arr.push_back(newNum);
    heapSort(arr);

    cout << "插入後重新排序結果: ";
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```

## 效能分析

時間複雜度：
1.	Insertion Sort：O(n)，n為陣列長度(最壞的情況下則是O(n2))。
2.	Quick Sort：正常快速排序下為O(nlogn)，n為陣列長度(最壞的情況下則是O(n2))。
3.	Merge Sort：O(nlogn)，n為陣列長度
4.	Heap Sort：O(nlogn)，n為陣列長度
   
空間複雜度：
1.	Insertion Sort：O(1)，原地排序算法
2.	Quick Sort：O(logn)，遞迴的最大深度
3.	Merge Sort：O(n)，n為陣列長度，需要額外的空間來存儲合併過程中的暫時數據
4.	Heap Sort：O(1)，原地排序算法


## 測試與驗證

### 測試案例

| 測試案例 | 輸入參數 $n$ | 預期輸出 | 實際輸出 |
|----------|--------------|----------|----------|
| 測試一   | $n = 0$      | 0        | 0        |
| 測試二   | $n = 1$      | 1        | 1        |
| 測試三   | $n = 3$      | 6        | 6        |
| 測試四   | $n = 5$      | 15       | 15       |
| 測試五   | $n = -1$     | 異常拋出 | 異常拋出 |

### 編譯與執行指令

```shell
$ g++ -std=c++17 -o sigma sigma.cpp
$ ./sigma
6
```

### 結論

1. 程式能正確計算 $n$ 到 $1$ 的連加總和。  
2. 在 $n < 0$ 的情況下，程式會成功拋出異常，符合設計預期。  
3. 測試案例涵蓋了多種邊界情況（$n = 0$、$n = 1$、$n > 1$、$n < 0$），驗證程式的正確性。

## 申論及開發報告

### 選擇遞迴的原因

在本程式中，使用遞迴來計算連加總和的主要原因如下：

1. **程式邏輯簡單直觀**  
   遞迴的寫法能夠清楚表達「將問題拆解為更小的子問題」的核心概念。  
   例如，計算 $\Sigma(n)$ 的過程可分解為：  

   $$
   \Sigma(n) = n + \Sigma(n-1)
   $$

   當 $n$ 等於 1 或 0 時，直接返回結果，結束遞迴。

2. **易於理解與實現**  
   遞迴的程式碼更接近數學公式的表示方式，特別適合新手學習遞迴的基本概念。  
   以本程式為例：  

   ```cpp
   int sigma(int n) {
       if (n < 0)
           throw "n < 0";
       else if (n <= 1)
           return n;
       return n + sigma(n - 1);
   }
   ```

3. **遞迴的語意清楚**  
   在程式中，每次遞迴呼叫都代表一個「子問題的解」，而最終遞迴的返回結果會逐層相加，完成整體問題的求解。  
   這種設計簡化了邏輯，不需要額外變數來維護中間狀態。

透過遞迴實作 Sigma 計算，程式邏輯簡單且易於理解，特別適合展示遞迴的核心思想。然而，遞迴會因堆疊深度受到限制，當 $n$ 值過大時，應考慮使用迭代版本來避免 Stack Overflow 問題。
