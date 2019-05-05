## 题目：字符串转换整数 (atoi)

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明：**

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

**示例 1:**

    输入: "42"

    输出: 42
    
**示例 2:**

    输入: "   -42"

    输出: -42

    解释: 第一个非空白字符为 '-', 它是一个负号。我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

**示例 3:**

    输入: "4193 with words"

    输出: 4193

    解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

**示例 4:**

    输入: "words and 987"

    输出: 0

    解释: 第一个非空字符是 'w', 但它不是数字或正、负号。因此无法执行有效的转换。

**示例 5:**

    输入: "-91283472332"

    输出: -2147483648

    解释: 数字 "-91283472332" 超过 32 位有符号整数范围。因此返回 INT_MIN (−231) 。

## 解答
这个题目跟前一个题目[整数反转](https://github.com/xuqingxin/leetcode/blob/master/Algorithms/0007-Reverse.Integer.md)很类似，把字符串中的数字字符从左到右依次取出来，跟‘0’的差值就是字符的数值，结果乘以10后加上这个数值就可以了。需要注意的部分是略过字符串前后的空格，处理下正负号，遇到非数字字符直接返回，最后还有一个跟前一题一样的范围问题。

```C#
public int MyAtoi(string str)
{
    int rtVal = 0;
    if (string.IsNullOrEmpty(str))
    {
        return rtVal;
    }
    str = str.Trim();
    if (string.IsNullOrEmpty(str))
    {
        return rtVal;
    }
    int p1 = Int32.MaxValue / 10;
    int p2 = Int32.MinValue / 10;
    char[] charsArray = str.ToCharArray();
    int i = 0;
    char ch = charsArray[i];
    bool negative = ch == '-';
    if (ch == '+' || ch == '-')
    {
        i++;
    }
    while (i < charsArray.Length)
    {
        ch = charsArray[i++];
        if (!IsDigitalNumber(ch))
        {
            return rtVal;
        }

        int n = ch - '0';
        if (rtVal > p1 || (rtVal == p1 && n > 7))
        {
            return Int32.MaxValue;
        }
        if (rtVal < p2 || (rtVal == p2 && n > 8))
        {
            return Int32.MinValue;
        }
        rtVal = rtVal * 10 + n * (negative ? -1 : 1);
    }

    return rtVal;
}

public bool IsDigitalNumber(char ch)
{
    return ch >= '0' && ch <= '9';
}
```