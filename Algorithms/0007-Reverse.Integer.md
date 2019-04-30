## 题目：整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**

    输入: 123

    输出: 321

**示例 2:**

    输入: -123

    输出: -321

**示例 3:**

    输入: 120
    
    输出: 21

注意：假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

## 解答
这个题目相对简单，只要对x不断通过模10把低位上的数字取出来，然后把它加到新数result上（加之前把新数乘以10），再把数字x整除10，直到x为0即可。

n = x % 10; // 获取个位数

x /= 10;    // 准备取下一位数字

result = result * 10 + n // 新的数字乘以10后再加上刚才取到的个位数

注意：因为存在数字溢出问题，所以新数乘以10之前要先判断是否会溢出。溢出的条件是result > Int32.MaxValue 或 result < Int32.MinValue。

因为Int32.MaxValue = 2147483647，Int32.MinValue = -2147483648，所以

* result > Int32.MaxValue / 10 或者 result == Int32.MaxValue / 10 且 n > 7
* result < Int32.MinValue / 10 或者 result == Int32.MinValue / 10 且 n < -8,

则result = result * 10 + n 溢出。

```C#
public int Reverse(int x) 
{
    int result = 0;
    int p1 = Int32.MaxValue / 10;
    int p2 = Int32.MinValue / 10;
    while (x != 0)
    {
        int n = x % 10;
        x /= 10;
        if (result > p1 || (result == p1 && n > 7))
        {
            return 0;
        }
        if (result < p2 || (result == p2 && n < -8))
        {
            return 0;
        }
        result = result * 10 + n;
    }

    return result;
}
```