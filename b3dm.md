# b3dm编写指南（1.0）

*说明：本文档适用于对b3dm有一定了解的人群，也可以用于只需要编程实现的人群。*

## 整体数据结构概览

<div align="center"><img src="https://camo.githubusercontent.com/793823b7de7ab4bc8e6c7af97c19531de0a026c1610317d9b4b24226cb874cfd/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f3366425156764168745a6e5238714b2e706e67"></div>[![image-20220307160210309]()](https://camo.githubusercontent.com/793823b7de7ab4bc8e6c7af97c19531de0a026c1610317d9b4b24226cb874cfd/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f3366425156764168745a6e5238714b2e706e67)

无论是哪一种格式，其文件类型都如上图所示。此处只针对**b3dm**进行说明。

# b3dm结构

<div align="center"><img src="https://camo.githubusercontent.com/8e22313212720451572967a8edf22c8ec37a304662c893c2d57aea4b62366b04/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f32784953575a754162553374584a732e706e67"></div>

------

### Header部分

- 模型类型（1-4字节）（uchar）: b3dm ——4字节的ANSI string，指示模型类型
- 版本号（5-8字节）（uint32）：1.0 ——当前版本号
- 总字节长度（9-12字节）（uint32）: 文件的总字节数 ——包含开头的28字节+后续的各块长度
- featureTable的json文本字节长度 (13-16字节)（uint32）
- featureTable的二进制字节长度（17-20字节）（uint32）: 可能为0
- batchTable的json文本字节长度（21-24字节）（uint32）
- batchTable的二进制字节长度（25-28字节）（uint32）: 可能为0

------

### Body部分

<div align="center"><img src="https://camo.githubusercontent.com/50bec3a81bd7fea473ba2253ee6ed7522c07abb96f4e96a8bfe7770798c9926c/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f337079774151736d677455525769622e706e67"></div>

[![image-20220307163845721]()](https://camo.githubusercontent.com/50bec3a81bd7fea473ba2253ee6ed7522c07abb96f4e96a8bfe7770798c9926c/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f337079774151736d677455525769622e706e67)

<div align="center"><img src="https://camo.githubusercontent.com/793823b7de7ab4bc8e6c7af97c19531de0a026c1610317d9b4b24226cb874cfd/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f3366425156764168745a6e5238714b2e706e67"></div>

#### feature table部分

<div align="center"><img src="https://camo.githubusercontent.com/fb3753f4e77dbaf412339320b31497f190dc50ddd01c5c36120ce9b93824ed39/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f5766313533587948426d61504f76722e706e67"></div>

① json数据

<div align="center"><img src="https://camo.githubusercontent.com/d49a5bb011bb48f31783420a1735bfd453c36ced68aa1c22e5a337e539f8b2e9/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f337536396250687a787756633270642e706e67"></div>

② 二进制数据（暂无）

------

#### batch table部分

<div align="center"><img src="https://camo.githubusercontent.com/4d6db4693ea39b0218885f3811878ced784b966a7aa7dd9f79c7d835525478d7/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f536141344859777339576d6e7649312e706e67"></div>

[![image-20220307164025870]()](https://camo.githubusercontent.com/4d6db4693ea39b0218885f3811878ced784b966a7aa7dd9f79c7d835525478d7/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f536141344859777339576d6e7649312e706e67)

①json数据

<div align="center"><img src="https://camo.githubusercontent.com/f10488f778c0fb9ff41693565bc7f8ec613343031309fb65306cb36bb24ca2e1/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f463969507a6b5a36357451563279632e706e67"></div>

[![image-20220307164057495]()](https://camo.githubusercontent.com/f10488f778c0fb9ff41693565bc7f8ec613343031309fb65306cb36bb24ca2e1/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f30372f463969507a6b5a36357451563279632e706e67)

②二进制数据（暂无）

------

#### 二进制glTF部分

一个b3dm中都内嵌了一个二进制的glTF模型。其总体架构如下：**头部**+**JSON**数据块部分+**二进制**数据块部分

<div align="center"><img src="https://camo.githubusercontent.com/005cce5a8e4781633678311eaf3bd784acc132b94af77d92dec35eee30c091ab/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f4a78584c3946625a776e4d5971434f2e706e67"></div>

- glb模型要求是**小端模式**，而**C++默认是小端模式**，**Java默认是大端模式**，因此**利用C++写入不用考虑大小端问题**

------

#### header部分

- 模型类型（1-4字节）（uchar）——ASCII 字符串 glTF
- 版本号（5-8字节）（uint32）——版本号为2.0
- glb模型总字节长度（uint32）

------

#### chunk（数据块）部分

##### json部分

- json部分字节长度（uint32）
- json部分对应的数据块类型（uint32）16进制：0x4E4F534A
- json部分数据块 （uchar） length() = json部分字节长度

**Binary部分**

- Binary部分字节长度（uint32）
- Binary部分对应的数据块类型（uint32）16进制：0x004E4942
- Binary部分数据块 （uchar） length() = Binary部分字节长度

------

##### json部分数据块详解

**① asset、scenes、nodes部分**

[![img](https://camo.githubusercontent.com/6d935886842ffa42fd299a1b5b5adf9ccb719da4f2b00aedd3924d1c33be840e/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f3248576979754f516f76654d3441392e706e67)](https://camo.githubusercontent.com/6d935886842ffa42fd299a1b5b5adf9ccb719da4f2b00aedd3924d1c33be840e/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f3248576979754f516f76654d3441392e706e67)

**②mesh、accessor部分**

[![img](https://camo.githubusercontent.com/344bd154b528dc88bb9a75c27e2db4feee7dba031b18a70d080105eada135f71/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f55497064315a4b6c54327747674d522e706e67)](https://camo.githubusercontent.com/344bd154b528dc88bb9a75c27e2db4feee7dba031b18a70d080105eada135f71/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f55497064315a4b6c54327747674d522e706e67)

**在写入的时候，按照accessors中的类别分类，按对应的数据类型写入**

**②bufferView、buffer部分**

[![img](https://camo.githubusercontent.com/126a9bcd6d964448347c22f29ebb4e68d535ee41e7b3059e964bddfc57786d73/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f4a346b564d6a5568644e57726c47462e706e67)](https://camo.githubusercontent.com/126a9bcd6d964448347c22f29ebb4e68d535ee41e7b3059e964bddfc57786d73/68747470733a2f2f73322e6c6f6c692e6e65742f323032322f30332f31322f4a346b564d6a5568644e57726c47462e706e67)

------

## 编写步骤

### 1.输入

------

#### ①读取源数据obj模型信息函数

**输入**：obj模型路径：string

**从obj模型读取：**

1、**顶点坐标**   ：Vertex类

2、**法向量** ： Normal类

3、**纹理坐标**（如果有的话）：Texture类

4、**面片索引** ![image-20220318101916796](https://s2.loli.net/2022/03/18/MG2mbTUH6nipcR7.png)   对应 **顶点坐标索引//法向量索引**，采用**三角面片形式**组织  

​	：vector<vector\<int\>>或vector<double[2]>

**额外读取**：

- **属性信息**   ——存放于batchTable中 ：Json::Value类   基于C++解析Json类库——JsonCpp库

**输出**：封装好的obj对象 ：ObjClass



#### ②计算顶点、法向量等的数量的函数 

对应glb模型accessors中的**count属性**

**输入**：obj模型对象 ：ObjClass 或 vector\<ObjClass>

**输出**：顶点、法向量等的个数



#### ③计算顶点、法向量等所占字节数（byteLength与byteOffset）的函数

对应glb模型accessors中的**byteLength与byteOffset属性**

**输入**：obj模型对象 ：ObjClass 或 vector\<ObjClass>

例：顶点所占字节数 =  顶点数量（②获得）* 维数 (3) *  sizeof(数据类型) (sizeof(float) = 4)

例如，我们指定存储顺序为：顶点-float、法向量-float、面片索引-unsigned short，

则 ——顶点部分的byteOffset = **0**

​    ——法向量部分的byteOffset = **顶点部分所占字节长度**

​    ——索引部分的byteOffset = **顶点部分所占字节长度 + 法向量部分所占字节长度**

**输出**：byteLength、byteOffset



#### ④计算顶点、法向量等的最值的函数 

对应glb模型accessors中的**max、min属性**

**输入**：obj模型对象 ：ObjClass 或 vector\<ObjClass>

**输出**：顶点、法向量等的最值

- 注意：此处针对多维向量（例如顶点、法向量是三维向量）的最值对应的点**并非真实存在**，**只是在各个方向上的最值的组合**

  例如：三个顶点(1,0,0), (0,1,0), (0,0,1)，最后生成的："max": [1,1,1], "min": [0,0,0]



- **当然，②、③、④部分可以边读取边计算边更新，以引用传递的方式在函数执行完成后获取对应的结果**



------

#### 针对一个b3dm文件中存在多个obj模型的情况，还需：

#### **⑤生成模型唯一ID号函数**

**输入**：所有读取的obj对象数组：vector\<ObjClass\>

在读取各个obj模型时，顺带生成各个obj模型的**ID号**，对应b3dm文件**featureTable中的Batch_Length属性值**以及**batchTable中的Batch_Id数组**

**输出**：obj对象 ：ObjClass

- **也可在读取模型时直接赋予ID号**



#### **⑥合并顶点、法向量、索引函数：**

**输入**：所有读取的obj对象数组 ：vector\<ObjClass\>

在b3dm模型中内嵌的glb模型的二进制数据部分，肯定存放的是一个**连续**的数据块，不可能类似数组一样分块存储，因此需要对多个模型的顶点、法向量等信息进行合并

- 将各个obj模型的顶点（法向量）数组合并成一个包含**所有**模型顶点（法向量）信息的数组，合并时注意：

  1、需要一个**判断函数**，多模型重复的顶点（法向量）**不重复出现**，只出现一次，使用一个索引值

  2、调整顶点（法向量）的索引值  ——合并之前，在obj-1中存在索引值为1的顶点，在obj-2中也存在索引值为1的顶点，合并后需要将只存在于obj-2中的索引值修改为**全局索引值**

  3、且需要在面片索引中进行调整 —— **f 顶点索引//法向量索引**
  
  - **不建议使用set<>进行去重，set在去重时会自动排序，破坏原有顶点顺序，可能造成不必要的麻烦**

**输出**：调整后的obj对象数组 ：vector\<ObjClass\>



#### ⑦合并属性函数：

**输入**： 所有读取obj数组 ：vector\<ObjClass\>

取出所有obj模型属性，按b3dm模型batchTable的格式，组成”**属性名": [obj-1属性值, obj-2属性值, ..., obj-n属性值]**“的格式, 其中n=vector\<ObjClass\>.size()，空值以**空字符串**填充

**输出**：合并后的属性对象 Json::Value对象，或map<string, vector\<string>>格式

- C++中map\<\>存储”键:键值"的格式，但与python中的字典数据类型不同，C++中**不支持键值的多数据类型**，在声明时必须把键值的类型定死，因此，统一先存储为string类型，之后在按需进行转化 



------

#### **输入格式**

##### obj对象

- 将单个模型的**顶点坐标集**、**法向量集**、**纹理坐标集**、**面片索引**、**模型ID**、**属性信息** class封装成一个对象
- 属性信息可能需要从数据库读出，再转为string进行存储
- 多个模型使用 链表或vector<> 存储

![image-20220318203914315](https://s2.loli.net/2022/03/18/BNnC94aZyWTROKF.png)

### 2.计算

#### ①featureTable的函数

该函数生成featureTable json部分、该部分字节长度

**输入**：id的个数（int 目前认为不会有超限情况）

需要计算{"BATCH_LENGTH": id_num}的字节长度，若(**长度+28**)不能整除8 补充空格

**输出**：①  {"BATCH_LENGTH": id_num}  ：string

​            ②  字节总长度 ：unsigned int



#### ②batchTable的函数

该函数生成batchTable json部分、该部分字节长度

**输入**：map<> 或 Json::Value

将输入转化为字符串，计算字符串的长度，得到最终的batchTable总长度，若不能整除8 补充空格

**输出**：① {...} ：string

​		    ② 字节总长度 ：unsigned int



#### ③glb生成Json的函数

输入：buffer内容



### 3.输出

#### ①输出二进制函数

**输入**：各部分内容

**输出**：二进制文件（文件后缀是.b3dm）