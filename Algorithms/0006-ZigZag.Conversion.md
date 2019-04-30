## 题目：Z 字形变换

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
<pre><code>
L   C   I   R
E T O E S I I G
E   D   H   N
</code></pre>
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

**示例 1:**

    输入: s = "LEETCODEISHIRING", numRows = 3

    输出: "LCIRETOESIIGEDHN"

**示例 2:**

    输入: s = "LEETCODEISHIRING", numRows = 4

    输出: "LDREOEIIECIHNTSG"

    解释:
<pre><code>
L     D     R
E   O E   I I
E C   I H   N
T     S     G
</code></pre>

## 解答
### 解法1
我们可以构造出一个矩阵，把字符按照Z字形状的填入相应的位置，最后逐行扫描矩阵，把非空字符链接起来即可。问题是怎么填这个矩阵。

我们观察下就可以发现，除了第一个字符，从第二个字符开始，每过numRows - 1个字符，方向就变一下。先下，然后斜上。方向向下，就是x += 0，y += 1；方向斜上，就是x += 1，y += -1；我们可以定义一个数组，里面存放x，y的增量值{0，1}和{1，-1}。

接下来要计算出矩阵的大小。行数已经固定了numRows，列数怎么计算？
首先考虑一个重复模式，把先竖再斜上作为一个单位，
<pre><code>
单位0   |单位1   |单位2   |单位3
  L     |  C    |  I     |  R
  E T   |  O E  |  S I   |  I G
  E     |  D    |  H     |  N
</code></pre>
每个单位字符数共为len = 2 * numRows - 2。对于一个固定的字符串，一共有(s.Length - 1) / (2 * numRows - 2) + 1个这样的单位。每个单位一共有numRows - 1列。所以矩阵的列数为((s.Length - 1) / (2 * numRows - 2) + 1) * (numRows - 1)。

```C#
public string convert(string s, int numRows)
{
    if (numRows == 1)
    {
        return s;
    }
    else
    {
        string rtVal = "";
        int numCols = ((s.Length - 1) / (2 * numRows - 2) + 1) * (numRows - 1);
        char[,] matrix = new char[numRows, numCols];
        int[,] dirs = new int[2, 2] { { 0, 1 }, { 1, -1 } };
        int dir = 0;
        int x = 0, y = 0;
        matrix[y, x] = s[0];
        for (int i = 1; i < s.Length; i++)
        {
            x += dirs[dir, 0];
            y += dirs[dir, 1];
            matrix[y,x] = s[i];
            if (i % (numRows - 1) == 0)
            {
                dir++;
                dir = dir % 2;
            }
        }

        for (int i = 0; i < numRows; i++)
        {
            for (int j = 0; j < numCols; j++)
            {
                if (matrix[i, j] != 0)
                {
                    rtVal += matrix[i, j];
                }
            }
        }
        return rtVal;
    }
}
```

### 解法2
这个题目只要求输出一个字符串，不需要整个Z字形状，所以我们只要依次把字符从上到下，再从下往上追加到每一行的结尾就可以了。
<pre><code>
L   C   I   R       LCIR
E T O E S I I G =>  ETOESIIG => LCIR ETOESIIG EDHN
E   D   H   N       EDHN
</code></pre>
```C#
public string convert2(string s, int numRows)
{
    if (numRows == 1)
    {
        return s;
    }
    else
    {
        StringBuilder rtVal = new StringBuilder();
        List<StringBuilder> lstBuilders = new List<StringBuilder>();
        for (int i = 0; i < numRows; i++)
        {
            lstBuilders.Add(new StringBuilder());
        }

        int row = 0;
        int dir = -1;
        for (int i = 0; i < s.Length; i++)
        {
            lstBuilders[row].Append(s[i]);
            if (row == 0 || row == numRows - 1)
            {
                dir = -dir;
            }
            row += dir;
        }

        for (int i = 0; i < numRows; i++)
        {
            rtVal.Append(lstBuilders[i].ToString());
        }
        return rtVal.ToString();
    }
}
```

### 解法3
这个其实纯粹是一个数学问题，对于一个确定的字符串和确定的行数，每一行上的字符在字符串中的位置都可以直接计算出来。

<pre><code>
单位0   |单位1   |单位2   |单位3
  L     |  C    |  I     |  R
  E T   |  O E  |  S I   |  I G
  E     |  D    |  H     |  N
</code></pre>
每个单位字符数共为len = 2 * numRows - 2。
对于第j个单位，第0行上的字符，在字符串中的位置是j * len，最后一行(numRows - 1)的字符在字符串中的位置是j * len + numRows - 1，中间的每一行有1个字符或2个字符，它们在字符串中的位置是j * len + i和j * len + len - i

```C#
public string convert3(string s, int numRows)
{
    if (numRows == 1)
    {
        return s;
    }
    else
    {
        int len = 2 * numRows - 2;
        StringBuilder rtVal = new StringBuilder();
        for (int i = 0; i < numRows; i++)
        {
            for (int j = 0; j + i < s.Length ; j += len)
            {
                rtVal.Append(s[j + i]);
                if (i != 0 && i != numRows - 1 && j + len - i < s.Length)
                {
                    rtVal.Append(s[j + len - i]);
                }
            }
        }
        return rtVal.ToString();
    }
}
```