假设数组有序，获取数组中的第 k 个数

示例题目为，提供两个有序数组，求两个数组的中位数

- java方法

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int k1 = (nums1.length + nums2.length + 1) / 2;
        int k2 = (nums1.length + nums2.lenght + 2) / 2;
        double ans = (getKthElement(nums1, nums2, k1) + getKthElement(nums1, nums2, k2)) / 2.0;
        return ans;
    }

    public int getKthElement(int[] nums1, int[] nums2, int k) {
        int length1 = nums1.length, length2 = nums2.length;
        int index1 = 0, index2 = 0;

        while (true) {
            //边界情况
            if (index1 == length1) {
                return nums2[index2 + k - 1];
            }
            if (index2 == length2) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return Math.max(nums1[index1], nums2[index2]);
            }
            //正常情况
            int newIndex1 = Math.min(index1 + k / 2, length1) - 1;
            int newIndex2 = Math.min(index2 + k / 2, length2) - 1;
            if (nums1[newIndex1] > nums2[newIndex2]) {
                k -= (newIndex2 - index2 + 1);
                index2 = newIndex2 + 1;
            } else {
                k -= (newIndex1 - index1 + 1);
                index1 = newIndex1 + 1;
            }
        }
    }
}
```


- python实现

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def findKthElement(nums1, nums2, k):
            l1, l2 = len(nums1), len(nums2)
            idx1, idx2 = 0, 0
            while 1:
                if idx1 == l1:
                    return nums2[idx2 + k - 1]
                if idx2 == l2:
                    return nums1[idx1 + k - 1]
                if k == 1:
                    return min(nums1[idx1], nums2[idx2])

                newIdx1 = min(idx1 + k // 2, l1) - 1
                newIdx2 = min(idx2 + k // 2, l2) - 1
                if nums1[newIdx1] > nums2[newIdx2]:
                    k -= newIdx1 - idx1 + 1
                    idx1 = newIdx1 + 1
                else:
                    k -= newIdx2 - idx2 + 1
                    idx2 = newIdx2 + 1
        k1 = (len(nums1) + len(nums2) + 1) // 2
        k2 = (len(nums1) + len(nums2) + 1) // 2
        return (findKthElement(nums1, nums2, k1) + findKthElement(nums1, nums2, k2)) / 2
```