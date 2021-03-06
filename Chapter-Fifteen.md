## 第十五章 字符串

**<h4 id = "1">1. 本章通篇对单词采用如下的简单定义：单词由空白字符隔开。但 HTML 或 RTF 等格式的许多实际文档包含格式命令。如何处理这种命令？是否还需要进行其他处理？</h4>**

许多文档系统都提供了去除所有格式命令并查看输入的原始文本表示的方法。我在长文本上运行 15.2 节的字符串重复程序时发现，该程序对文本的格式非常敏感。程序处理詹姆斯一世钦定版《圣经》中的 4 460 056 个字符需要 36 秒，且最长的重复字符串为 269 个字符。如果删除每行的行号以标准化输入文本，那么长字符串就可以跨越行边界，从而最长的重复字符串达到了 563 个字符，但是程序找到它的时间几乎没有变。 

**<h4 id = "2">2. 在内存很大的机器上如何使用 C++ 标准模版库的 set 或 map 来解决 13.8 节中的搜索问题？与 McIlroy 的结构进行比较，它需要多少内存？</h4>**

**<h4 id = "3">3. 在 15.1 节的散列函数中采用[答案 9.2](Chapter-Nine.md#2) 中的专用 malloc，能使速度提升多少？</h4>**

由于该程序每次插入都需要执行很多次搜索，因此只有很少的时间用于内存分配。采用专用的存储分配器能使处理时间减少约 0.06 秒，能使插入程序的速度提高 10%，但是对整个程序的提速只有 2%。

**<h4 id = "4">4. 当散列表较大，且散列函数能够均匀分布数据时，表中每个链表的元素都不多。如果这两个条件都满足，那么查找所需的时间就会很多。如果 15.1 节的散列表中没有找到某个新的字符串，就将它放到链表的最前面。为了模拟散列存在的问题，将 NHASH 设置为 1，并用 15.1 节的链表策略和其他的链表策略（例如添加到链表的最后面，或者将最近找到的元素放置到链表的最前面）进行实验。</h4>**

**<h4 id = "5">5. 在观察 15.1 节词频程序的输出时，将单词按频率递减的顺序输出是最合适的。如何修改 C 和 C++ 程序以完成这一任务？如何仅输出 M 个最常见的单词（其中 M 是常数，例如 10 或者 1000）？</h4>**

可以在 C++ 程序中添加另一个映射，将一组单词跟它们的计数联系起来。在 C 程序中我们可以根据计数对数组排序，然后对其迭代（由于一些单词的计数会比较大，数组应该比输入文件小得多）。对于常见的文档，我们可以用关键字索引，并保存一个在一定范围（如 1~1 000）内计数的链表数组。

**<h4 id = "6">6. 给定一个新的输入字符串，如何搜索后缀数组，以找到所存储文本中的最长匹配？如何建立一个图形用户界面来完成该任务？</h4>**

**<h4 id = "7">7. 我们的程序对于“常见”的输入能够快速找到重复的字符串，但是在某些输入下速度很慢（超过平方复杂度）。计算这类输入下程序运行的时间。实际应用中曾出现过这类输入吗？</h4>**

算法教材多次提醒我们注意类似于 "aaaaaaaa" 的输入。我发现对由换行符组成的文件计时要更容易一些。程序处理 5 000 个换行符需要 2.09 秒，处理 10 000 个换行符需要 8.90 秒，处理 20 000 个换行符需要 37.90 秒。这一增长速度要比平方快一些，也许正比于大约 n log<sub>2</sub><sup>n</sup> 次比较，其中每次比较的平均开销都正比于 n。把一个大输入文件的两份拷贝拼接在一起产生的不良输入可能更接近实际生活。

**<h4 id = "8">8. 如何修改查找重复字符串的程序，以找出出现超过 M 次的最长的字符串？</h4>**

子数组 a[i..i+M] 表示 M+1 个字符串。由于数组是有序的，我们可以通过调用在第一个和最后一个字符串上调用 comlen 函数来快速确定这 M+1 个字符串共有的字符数： `comlen(a[i], a[i+M])` 本书网站提供了实现这一算法的代码。

**<h4 id = "9">9. 给定两个输入文本，找出它们共有的最长字符串。</h4>**

把第一个字符串读入数组 c，记录其结束的位置并在其最后填入空字符；然后读入第二个字符串并进行同样的处理。跟以前一样进行排序。扫描数组时，使用"异或"操作来确保恰有一个字符串是从过滤点前面开始的。

**<h4 id = "10">10. 说明在查找重复字符串的程序中，如何通过仅指向从单词边界开始的后缀来减少指针的数目。这对程序的输出有何影响？</h4>**

**<h4 id = "11">11. 实现一个程序，生成字母级别的马尔可夫文本。</h4>**

**<h4 id = "12">12. 如何使用 15.1 节中的工具和方法生成（零阶或非马尔可夫）随机文本？</h4>**

**<h4 id = "13">13. 本书网站上提供了生成单词级别的马尔可夫文本的程序，用自己的一些文档测试该程序。</h4>**

**<h4 id = "14">14. 如何使用散列对马尔可夫程序提速？</h4>**

下面的函数对 k 个单词组成的序列进行了散列，其中每个单词都以空字符结束：
```
unsigned int hash(char *p)
    unsigned int h = 0
    int n
    for (n = k; n > 0; p++)
        h = MULT * h + *p
        if (*p == 0)
            n-
    return h % NHASH
```
本书网站上的一个程序使用这个散列函数取代了马尔可夫文本生成算法中的二分搜索，使平均运行时间从 O(n log n) 降到了 O(n)。该程序在散列表中为元素使用了链表表示法，只增加了 nwords 个 32 位整数的额外空间，其中 nwords 是输入中的单词个数。

**<h4 id = "15">15. 15.3 节中对香农的引用描述了他用来构建马尔可夫文本的算法，编写程序实现该算法。它给出了马尔可夫频率的很好的近似，但不是精确的形式。解释为什么不是精确的形式。编写程序从头开始扫描整个字符串（从而可以使用真实的频率）以生成每个单词。</h4>**

假设我们正从一个有 100 万个单词的文档中生成 1 阶马尔可夫文本，该文档只在短语 "x y x z" 中包含单词 x、y 和 z。x 后面跟 y 的可能性应为 1/2，后面跟 z 的可能性也应为 1/2。在香农的算法中有什么差别？

**<h4 id = "16">16. 如何使用本章的方法形成字典的单词列表（这是 13.8 节中 Doug McIloy 面临的问题）？如何在不使用字典的前提下建立拼写检查器？如何在不使用语法规则的前提下建立语法检查器？</h4>**

如何利用 k 连字母或 k 连单词的计数？

**<h4 id = "17">17. 研究一下在语音识别和数据压缩等应用中，与 k 连字母分析有关的方法是如何使用的。</h4>**

一些商业语音识别器是基于三连统计的。