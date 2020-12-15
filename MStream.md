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





