## 1. 简介
QModelIndex用于定位数据模型中的数据。QModelIndex数据三要素：行row，列column，父节点索引parent  

判断模型是否有效bool QModelIndex::inValid

## 2. 
在model/view架构中，model为view和delegates提供了标准接口，在Qt中，标准接口QAbstractItemModel类被定义。不管底层数据以哪种方式存储，QAbstractItemModel的子类都以层次结构的形式表示数据，结构中包含了数据项表。我们按照这种约定访问model中的数据项

## 3. 

概念总结：  
1，Model indexes为views与delegages提供model中数据项定位的信息，它与底层的数据结构无关。  
2，通过指定行，列数，父项的model index来引用数据项。  
3，依照别的组件的要求，model indexes被model构建。  
4，使用index()时，如果指定了有效的父项的model index,那么返回得到的model index对应于父项的某个孩子。  
5，使用index()时，如果指定了无效的父项的model index,那么返回得到的model index对应于顶层项的某个孩子。  
6, 角色对一个数据项包含的不同类型的数据给出了区分。 

## 参考链接
1. https://my.oschina.net/itissue/blog/112512