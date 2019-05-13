## 题目：回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

    输入: 121
    输出: true

**示例 2:**

    输入: -121
    输出: false
    解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例 3:**

    输入: 10
    输出: false
    解释: 从右向左读, 为 01 。因此它不是一个回文数。

**进阶:**

你能不将整数转为字符串来解决这个问题吗？

## 解答
看到这个题目，首先想到的是把数字转化为字符串，然后头尾比较字符是否相同。

```C#
public bool IsPalindrome(int x)
{
    char[] charArray = Convert.ToString(x).ToCharArray();
    int start = 0;
    int end = charArray.Length - 1;
    while (start < end)
    {
        if (charArray[start] == charArray[end])
        {
            start++;
            end--;
        }
        else
        {
            break;
        }
    }

    return start == end || start == end + 1;
}
```

题目中也提到了是否可以不把数字转为化字符串来解这个题目？
如果数字是回文的，那么从中间把数字分成两个部分，把其中的一部分前后换下，这两个部分就应该是一样的。

如果数字是偶数，比如123321，可以分成123，321，后半部分321顺序换下就成了123。

如果数字长度为奇数，比如1234321，可以分成123，4321。把后半部分顺序换下，就成了1234。把最后一位数字4去掉，也成了123。

根据这个思路，我们可以通过不断截取数字x的个位数，用个位数来生成新的数字，直到新的数字大于或等于被截取个位数后的x。
```C#
public bool IsPalindrome(int x)
{
    if (x < 0 || x != 0 &&  x % 10 == 0)
    {
        return false;
    }

    int y = 0;
    while (y < x)
    {
        y = y * 10 + x % 10;
        x /= 10;
    }

    return y == x || y / 10 == x;
}
```