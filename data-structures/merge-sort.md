# 🐳 Merge Sort

Merge Sort is a divide-and-conquer sorting algorithm. The intuition behind it is to divide the data set into smaller and smaller sub-arrays until it is easy to sort, and then merge the sorted sub-arrays back into a larger sorted array.

The steps for implementing Merge Sort are as follows:

* **Divide the data set into two equal parts:** The first step in the Merge Sort algorithm is to divide the data set into two equal halves. This is done by finding the middle point of the data set and splitting the data into two parts.
* **Recursively sort each half:** Once the data set is divided into two halves, the Merge Sort function is called recursively on each half. The recursive calls continue until each half of the data is sorted into single-element arrays.
* **Merge the sorted halves:** Once each half of the data is sorted, the two halves are merged back into one final sorted array. The merging process involves comparing the first elements of each half and inserting the smaller element into the final array. This process continues until one of the halves is empty. The remaining elements of the other half are then inserted into the final array.
* **Repeat the process until the entire data is sorted:** The Merge Sort function is called recursively until the entire data set is sorted.

<figure><img src="https://leetcode.com/problems/sort-an-array/Figures/912/Slide2.PNG" alt=""><figcaption></figcaption></figure>

## **Algorithm**

1. Create a helper function called `merge` which takes in the original array `arr`, indices `left`, `mid`, `right`, and a temporary array `tempArr` as parameters.
   * Calculate the start indices and sizes of the two halves of the array. The first half starts from the `left` index and the second half starts from `mid+1`.
   * Copy elements of both halves into the temporary array.
   * Merge the sub-arrays from the temporary array `tempArr` back into the original array `arr` in a sorted order using a while loop. The loop runs until either the first half or second half is completely merged. In each iteration, the smaller of the two elements from the first and second half is copied into the original array "arr".
   * Copy any remaining elements from the first half or second half into the original array.
2. Create a recursive function called `mergeSort` which takes in the original array `arr`, indices `left`, `right`, and a temporary array `tempArr` as parameters.
   * Check if the `left` index is greater than or equal to the `right` index. If it is, we return from the function.
   * Calculate the `mid` index.
   * Sort the first and second halves of the array recursively by calling the `mergeSort` function.
   * Merge the sorted halves by calling the `merge` function.
3. Create a temporary array `temporaryArray` with the same size as the `nums` array.
4. Call the `mergeSort` function on the `nums` array with boundary, `0`, and `nums.size()-1`.
5. Return the sorted array `nums`.

```cpp
class Solution {
    // Function to merge two sub-arrays in sorted order.
    void merge(vector<int> &arr, int left, int mid, int right, vector<int> &tempArr) {
        // Calculate the start and sizes of two halves.
        int start1 = left;
        int start2 = mid + 1;
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        // Copy elements of both halves into a temporary array.
        for (int i = 0; i < n1; i++) {
            tempArr[start1 + i] = arr[start1 + i];
        }
        for (int i = 0; i < n2; i++) {
            tempArr[start2 + i] = arr[start2 + i];
        }

        // Merge the sub-arrays 'in tempArray' back into the original array 'arr' in sorted order.
        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (tempArr[start1 + i] <= tempArr[start2 + j]) {
                arr[k] = tempArr[start1 + i];
                i += 1;
            } else {
                arr[k] = tempArr[start2 + j];
                j += 1;
            }
            k += 1;
        }

        // Copy remaining elements
        while (i < n1) {
            arr[k] = tempArr[start1 + i];
            i += 1;
            k += 1;
        }
        while (j < n2) {
            arr[k] = tempArr[start2 + j];
            j += 1;
            k += 1;
        }
    }

    // Recursive function to sort an array using merge sort
    void mergeSort(vector<int> &arr, int left, int right, vector<int> &tempArr) {
        if (left >= right) {
            return;
        }
        int mid = (left + right) / 2;
        // Sort first and second halves recursively.
        mergeSort(arr, left, mid, tempArr);
        mergeSort(arr, mid + 1, right, tempArr);
        // Merge the sorted halves.
        merge(arr, left, mid, right, tempArr);
    }

public:
    vector<int> sortArray(vector<int>& nums) {
        vector<int> temporaryArray(nums.size());
        mergeSort(nums, 0, nums.size() - 1, temporaryArray);
        return nums;
    }
};
```

**Complexity Analysis**

Here, $$nnn$$ is the number of elements in the `nums` array.

* Time complexity: $$O(nlog⁡n)O(n \log n)O(nlogn)$$
  * We divide the `nums` array into two halves till there is only one element in the array, which will lead to $$O(log⁡n)O(\log n)O(logn)$$ steps.\
    $$n→n/2→n/4→...→1 (k steps)n \rarr n/2 \rarr n/4 \rarr ... \rarr 1 \space (\text{k steps}) n→n/2→n/4→...→1 (k steps)$$\
    $$n/2(k−1)=1  ⟹  n / 2^{(k-1)} = 1 \impliesn/2(k−1)=1⟹$$ $$k≈log⁡nk \approx \log nk≈logn$$
  * And after each division, we merge those respective halves which will take $$O(n)O(n)O(n)$$ time each.
  * Thus, overall it takes $$O(nlog⁡n)O(n \log n)O(nlogn)$$ time.
* Space complexity: $$O(n)O(n)O(n)$$
  * The recursive stack will take $$O(log⁡n)O(\log n)O(logn)$$ space and we used an additional array `temporaryArray` of size $$nnn$$.
  * Thus, overall we use $$O(log⁡n+n)=O(n)O(\log n + n) = O(n)O(logn+n)=O(n)$$ space.
