## 题目：无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例 1:**

    输入: "abcabcbb"

    输出: 3 

    解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

**示例 2:**

    输入: "bbbbb"

    输出: 1

    解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

**示例 3:**

    输入: "pwwkew"

    输出: 3

    解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
    请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

## 解答

题目的要求是不含重复字符的字串，所以可以使用Map来存储已经扫描过的字符。假设目前已知索引i到索引j之间的字串最长，那么考察索引为j+1的字符。如果在Map中有这个字符，说明i到j之间有一个字符和它相同，假设其索引为k，那么我们只要考虑k+1到j+1之间的字符，因为k和j+1位置上的字符是相同的。

我们可以使用一个滑动窗口来扫描字符串。使用一个变量start记录窗口左边的索引位置，一个变量end来记录窗口右边的索引位置。让end从0到n-1依次考察每个字符。
* 如果这个字符在Map中不存在，就把它加入Map，同时比较最长字串长度maxLength和end-start+1（当前长度）的大小，保留大的那一个；
* 如果Map中有这个字符，那么把start设置为start和这个字符在前一个索引中的位置中大的一个（有可能前一个字符在第i个位置，但是start已经到了第5个位置，这时start不能设置为i+1，应该继续保留在5这个位置）

```C#
public int LengthOfLongestSubstring(string s)
{
    Dictionary<char, int> map4Char = new Dictionary<char, int>();
    int start = 0, end = 0;
    int maxLength = 0;
    while (end < s.Length)
    {
        if (map4Char.TryGetValue(s[end], out int index))
        {
            start = Math.Max(index + 1, start);
        }
        maxLength = Math.Max(maxLength, end - start + 1);
        map4Char[s[end]] = end;
        end++;
    }

    return maxLength;
}
```