## 题目：寻找两个有序数组的中位数

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5

## 解答

要解答这个问题，首先我们要知道什么是中位数。
> 中位数（又称中值，英语：Median），统计学中的专有名词，代表一个样本、种群或概率分布中的一个数值，其可将数值集合划分为相等的上下两部分。
对于有限的数集，可以通过把所有观察值高低排序后找出正中间的一个作为中位数。如果观察值有偶数个，通常取最中间的两个数值的平均数作为中位数。

假设我们将nums1 A 和nums2 B 各自划分为2个部分。

          left_A             |        right_A
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]


          left_B             |        right_B
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]

其中len(left_A)=i, len(right_A)=m−i; len(left_B)=j, len(right_B)=n-j。

将left_A 和left_B 放入一个集合，并将right_A 和right_B 放入另一个集合。 再把这两个新的集合分别命名为left_part 和right_part：

          left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]

如果我们可以确认：

1. len(left_part)=len(right_part)
2. max(left_part)≤min(right_part)
 
那么，我们已经将{A,B} 中的所有元素划分为相同长度的两个部分，且其中一部分中的元素总是大于另一部分中的元素。此时中值就是：

median=(max(left_part)+min(right_part)) / 2
​
为此我们只要保证：
1. i+j=m−i+n−j（或：m−i+n−j+1）(假如：n≥m，只需要使i=0∼m,j=(m+n+1)/2−i）
2. B[j−1]≤A[i] 以及 A[i−1]≤B[j]
   
```C#
public double FindMedianSortedArrays(int[] nums1, int[] nums2)
{
    if (nums1.Length > nums2.Length)
    {
        int[] tmp = nums1;
        nums1 = nums2;
        nums2 = tmp;
    }

    int iMin = 0;
    int iMax = nums1.Length;
    while (iMin <= iMax)
    {
        int i = (iMin + iMax) / 2;
        int j = (nums1.Length + nums2.Length + 1) / 2 - i;
        if (i > iMin && nums1[i - 1] > nums2[j])
        {
            iMax = i - 1;
        }
        else if (i < iMax && nums2[j - 1] > nums1[i])
        {
            iMin = i + 1;
        }
        else
        {
            int maxLeft = 0;
            if (i == 0)
            {
                maxLeft = nums2[j - 1];
            }
            else if (j == 0)
            {
                maxLeft = nums1[i - 1];
            }
            else
            {
                maxLeft = Math.Max(nums1[i - 1], nums2[j - 1]);
            }

            if ((nums1.Length + nums2.Length + 1) % 2 == 0)
            {
                return maxLeft;
            }

            int minRight = 0;
            if (i == nums1.Length)
            {
                minRight = nums2[j];
            }
            else if (j == nums2.Length)
            {
                minRight = nums1[i];
            }
            else
            {
                minRight = Math.Min(nums1[i], nums2[j]);
            }

            return (maxLeft + minRight) / 2.0;
        }
    }

    return 0.0;
}
```