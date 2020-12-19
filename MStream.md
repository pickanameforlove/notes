## document文件

* documentID文档的唯一标识
* wordIdArray包含的wordID数组
* wordFreArray包含的word的频率数组。
* 初始化方法，同时更新全局的wordToIdMap和wordList这两个属性，同时为文档的三个属性赋值。

```python
            if wId not in wordFreMap:
                wordFreMap[wId] = 1
            else:
                # wordFreMap[wId] = wordFreMap[wId] + 1
                wordFreMap[wId] = 1
```

## documentSet文件

* 此文件有两个属性，文件的数量和文件数组。通过读取一个训练集文件来对这两个属性进行初始化。



## MStream文件

* 属性有，wordList，wordToIdMap，alpha，β，dataset，V（读取完一个文件后的所有单词的数量）...
* 然后还有几个函数，getDocuments函数，runMStream函数和runMStreamF函数。



## model文件

### 属性

* dataset
* α0，是一个超参数，α是根据α0和当前处理的文档数目（self.D）来进行计算的。
* β
* K
* V，这个算法的V是前后大约五个Batch的单词数目。
* batchNum2tweetID，这里是由于将documentSet分为几个batch之后，我们需要设置第几个batch的结束document是哪一个document。
* D_ALL，这个属性是documentSet中包含的文档总数。而D代表的是当前处理的文档的数量。
* m_Z 存的是the number of words in the cluster z.
* n_zv 存的是the number of occurrences of word v in cluster z
* β0
* batchNum当前batch的编号。
* 重要的参数，K，iterNum，SampleNum，alpha，beta，BatchNum，BatchSaved，
* 此实验是将若干个文档集合为一个batch，随后对batch进行处理。batchNum就是第几个batch的意思了。
* 而后对每个batch进行迭代处理。iterNum就是每个batch的迭代次数。
* currentDoc，当前指向哪一个文档进行处理。

### 方法

* getAveBatch，初始化batchNum2tweetID这个属性。注意好像只是batchNUM和其中的一个文档有记录。
* gibbsSampling，一个抽样方法，这个方法好像是完成一个batch里面的迭代更新的。将一个文档从所属的cluster删除，而后再加入到一个合适的cluster之中。
* sampleCluster，输入是document，documentID，islast，计算两个概率，为什么这两个概率第一部分的分母(D+1+αD)不参与计算。计算方式是两个循环。如果是最后一次迭代，那么采用概率最大的那一个簇，如果不是最后一次迭代，采用的是随机选取的形式！！
* getBeta0函数，应该计算的是Vβ的值。
* V的计算方式。

```python
for w in range(document.wordNum):
    wordNo = document.wordIdArray[w]
    wordFre = document.wordFreArray[w]
    for j in range(wordFre):
        if wordNo not in self.n_zv[cluster]:
             self.n_zv[cluster][wordNo] = 0
        valueOfRule2*=(self.n_zv[cluster[wordNo]+self.beta+j)
                       /(self.n_z[cluster]+self.beta0 + i)  
        i += 1
prob[cluster] *= valueOfRule2
for w in range(document.wordNum):
    wordFre = document.wordFreArray[w]
    for j in range(wordFre):
        valueOfRule2 *= (self.beta + j) / (self.beta0 + i)
        i += 1
prob[self.K] *= valueOfRule2
                                 
#计算Vβ
Words = []
        if self.batchNum < 5:
            for i in range(1, self.batchNum + 1):
                Words = list(set(Words + self.word_current[i]))
        if self.batchNum >= 5:
            for i in range(self.batchNum - 4, self.batchNum + 1):
                Words = list(set(Words + self.word_current[i]))
        return (float(len(list(set(Words)))) * float(self.beta))
```

### 函数的调用过程

* main函数调用runMStream(K, AllBatchNum, AllBatchNum, alpha, beta, iterNum, sampleNum, dataset, timefil, wordsInTopicNum)
* runMStream里面利用这些参数新建一个MStream对象，MStream执行getDocuments函数初始化Documents的相关属性。
* 执行SampleNum次循环，循环体是mstream.runMStream函数。
* runMStream方法里面，利用参数新建一个model对象，model对象执行run_MStream方法。
* 对batch中的文档处理的方法是，先初始化，使用两个概率公式，计算是否新建一个簇。得到初始化的簇集之后，就进行iterNum次的更新迭代，迭代方法就是先将文档从所属cluster删除，而后再进行一次概率的计算。
* K这个属性代表当前簇的数目，初始的时候，簇的数目确实是0，所以初始参数是0，但是随着后面簇的更新，K这个属性会更新的。
* 各个batch之间的簇是共用的。

```
def run_MStream(self, documentSet, outputPath, wordList, AllBatchNum):
        self.D_All = documentSet.D  # The whole number of documents
        self.z = {}  # (documentID -> clusterID)
        self.m_z = {}  # (clusterID -> number of documents)
        self.n_z = {}  # (clusterID -> number of words)
        self.n_zv = {}  # (n_zv[clusterID][wordID] = number)
        self.currentDoc = 0  # Store start point of next batch
        self.startDoc = 0  # Store start point of this batch
        self.D = 0  # The number of documents currently
        self.K_current = copy.deepcopy(self.K) # the number of cluster containing documents currently
        # self.BatchSet = {} # No need to store information of each batch
        self.word_current = {} # Store word-IDs' list of each batch

        # Get batchNum2tweetID by AllBatchNum
        self.getAveBatch(documentSet, AllBatchNum)
        print("batchNum2tweetID is ", self.batchNum2tweetID)

        while self.currentDoc < self.D_All:
            print("Batch", self.batchNum)
            if self.batchNum not in self.batchNum2tweetID:
                break
            self.intialize(documentSet)
            self.gibbsSampling(documentSet)
            print("\tGibbs sampling successful! Start to saving results.")
            self.output(documentSet, outputPath, wordList, self.batchNum - 1)
            print("\tSaving successful!")
```

**run_MStreamF**

1. deepcopy自己进行了一份内存的复制，而不是简单的指针引用。
2. self.BatchSet是存储每个batch属性的一个属性，每个batch里面深拷贝了一些属性，比如D,z,m_z,n_z,n_zv
3. intialize初始化函数，对~~所有的文档~~（应该是一个batch里面的文档）进行一个初始化，将文档的统计信息放入对应的batch中，并且对文档进行聚簇，对对应的簇的信息进行更新。
4. word_current[i]表示在第i个batch中的wordno的列表。
5. m_z表示cluster和簇中文档的数目，
6. n_zv[cluster] = {},n_zv\[cluster\][wordNo] = wordFreq，簇中单词编号和单词出现频率的一个字典。
7. n_z[cluster]代表簇中出现的单词总数。
8. gibbsSampling是在初始化一个簇的文档之后，又进行的迭代更新的操作。
9. **batchNum**是一个全局变量
10. **为什么batchNum可以大于max_Batch?**
11. 