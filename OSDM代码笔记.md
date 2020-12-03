```python
        
    #from document.py
    for w in self.widFreq:#widFreq结构是dict,键是wid，值是频率。
            wFreq = self.widFreq[w]
            self.widToWidFreq[w]={}  
            for w2 in self.widFreq:
                if w!=w2:
                    w2Freq = self.widFreq[w2]
                    total = wFreq+w2Freq
                    score = wFreq/total
                    self.widToWidFreq[w][w2] = score#widtoWidFreq是一个数组，数组的下标代表wid，每个wid又对应一个dict，dict结构是，key为wid（此wid与后面的wid不同），value为score.
                    
                    
    #from model.py
        def createNewCluster(self,document):
        self.cluster_counter[0] = self.cluster_counter[0]+1
        newIndexOfClus = self.cluster_counter[0] # = {}   clusterID -> [ cn, cw, cwf, cww, cd, csw]

        self.clusters[newIndexOfClus]={}
        self.clusters[newIndexOfClus][con.I_cn]=[]
        self.clusters[newIndexOfClus][con.I_cwf] = {}
        self.clusters[newIndexOfClus][con.I_cww] = {}
        self.clusters[newIndexOfClus][con.I_cd] = 1.0
        self.clusters[newIndexOfClus][con.I_csw] = 0
        self.addDocumentIntoClusterFeature(document,newIndexOfClus)
    #代码中的代号和论文中的一个对应关系如下：
    
```

**代码中的符号和论文中的符号中的对应关系**：

1. $m_z$，簇z中包含的文档数目，$CF[I\_cn].len()$，CF[I_cn]代表的是所有在簇z中的document id的列表
2. $n_z^w$，簇z中单词w出现的频率，$CF[I\_cwf][w]$
3. $cw_z$，簇z的单词共现矩阵，$CF[I_cww]$
4. $len_z$，簇z中包含的单词总数，$CF[I\_csw]$
5. $u_z$，最后一次更新的时间戳，$CF[I\_cl]$

值得一提的是簇共现矩阵的更新：

```python
        for w in document.widFreq:
            ...

            for w2 in document.widFreq: 
                if w!=w2:
                    if w2 not in CF[con.I_cww][w]:
                        CF[con.I_cww][w][w2] = document.widToWidFreq[w][w2]
                    else:
                        CF[con.I_cww][w][w2] = CF[con.I_cww][w][w2]+document.widToWidFreq[w][w2]
```

