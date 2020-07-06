

```python
from nltk.book import *
from nltk import *
#词语索引视图显示一个指定单词的每一次出现，连同一些上下文一起显示。
text1.concordance("monstrous")

#还有哪些词出现在相似的上下文中？
text1.similar("monstrous")

#函数common_contexts允许我们研究两个或两个以上的词共同的上下文
text2.common_contexts(["monstrous", "very"])

#我们也可以判断词在文本中的位置：从文本开头算起在它前面有多少词。这个位置信息可以用 离散图表示。每一个竖线代表一个单词，每一行代表整个文本。
#text4.dispersion_plot(["citizens", "democracy", "freedom", "duties", "America"])

#我们刚才看到的不同风格产生一些随机文本,库待完善
#text3.generate()

#计数与统计
text3.count("smote")
def lexical_diversity(text):
	return len(text) / len(set(text))

def percentage(count, total):
	return 100 * count / total

#频率分布
fdist1 = FreqDist(text1)
vocabulary1 = list(fdist1.keys())
vocabulary1[:50]
fdist1['whale']
print()

#fdist1.plot(50, cumulative=True)
#一次
fdist1.hapaxes()

V = set(text1)
long_words = [w for w in V if len(w) > 15]
sorted(long_words)
print()

fdist5 = FreqDist(text5)
sorted([w for w in set(text5) if len(w) > 7 and fdist5[w] > 7])
print()

#要获取搭配，我们先从提取文本词汇中的词对也就是双连词开始
list(bigrams(['more','is', 'said', 'than', 'done']))
text4.collocations()
print()

import nltk
nltk.corpus.gutenberg.fileids()
emma = nltk.corpus.gutenberg.words('austen-emma.txt')
emma = nltk.Text(nltk.corpus.gutenberg.words('austen-emma.txt'))
from nltk.corpus import gutenberg
gutenberg.fileids()

#raw()函数给我们没有进行过任何语言学处理的文件的内容
#sents()函数把文本划分成句子，其中每一个句子是一个词链表。
for fileid in gutenberg.fileids():
	num_chars = len(gutenberg.raw(fileid)) 
	num_words = len(gutenberg.words(fileid))
	num_sents = len(gutenberg.sents(fileid))
	num_vocab = len(set([w.lower() for w in gutenberg.words(fileid)]))
 	print int(num_chars/num_words), int(num_words/num_sents), int(num_words/num_vocab), fileid

#各种语料库
from nltk.corpus import *
chatroom = nps_chat.posts('10-19-20s_706posts.xml')
brown.words(categories='news')
brown.words(fileids=['cg22'])
```

