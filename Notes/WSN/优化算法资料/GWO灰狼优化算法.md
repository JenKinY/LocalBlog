## 1.算法原理

灰狼属于犬科动物，被认为是顶级的掠食者，它们处于生物圈食物链的顶端。灰狼大多喜欢群居，每个群体中平均有５-12只狼。特别令人感兴趣的是，它们具有非常严格的社会等级层次制度，如图1,金字塔第一层为种群中的领导者，称为 α 。  
在狼群中 α 是具有管理能力的个体，主要负责关于狩猎、睡觉的时间和地方、食物分配等群体中各项决策的事务。金字塔第二层是 α 的智囊团队，称为 β 。  
β 主要负责协助α 进行决策。当整个狼群的 α 出现空缺时，β 将接替 α 的位置。  
β 在狼群中的支配权仅次于 α，它将 α 的命令下达给其他成员，并将其他成员的执行情况反馈给 α 起着桥梁的作用。  
金字塔第三层是 δ ，δ 听从 α 和 β 的决策命令，主要负责侦查、放哨、看护等事务。  
适应度不好的 α 和 β 也会降为 δ 。金字塔最底层是 ω ，主要负责种群内部关系的平衡。

![](https://upload-images.jianshu.io/upload_images/6881750-af85f6e16673e041.png?imageMogr2/auto-orient/strip|imageView2/2/w/693/format/webp)

灰狼的社会等级制度

此外，集体狩猎是灰狼的另一个迷人的社会行为。灰狼的社会等级在群体狩猎过程中发挥着重要的作用，捕食的过程在 α 的带领下完成。灰狼的狩猎包括以下 3个主要部分：  
1）跟踪、追逐和接近猎物；  
2）追捕、包围和骚扰猎物，直到它停止移动；  
3）攻击猎物

#### 1.1 包围猎物

在狩猎过程中，将灰狼围捕猎物的行为定义如下：

式（1） ![\vec D = |\vec C.\vec X_p(t)-\vec X(t)](https://math.jianshu.com/math?formula=%5Cvec%20D%20%3D%20%7C%5Cvec%20C.%5Cvec%20X_p(t)-%5Cvec%20X(t))  
式（2） ![\vec X(t+1) = \vec X_\rho(t)-\vec A.\vec D](https://math.jianshu.com/math?formula=%5Cvec%20X(t%2B1)%20%3D%20%5Cvec%20X_%5Crho(t)-%5Cvec%20A.%5Cvec%20D)

式（1）表示个体与猎物的距离  
式（2）是灰狼的位置更新公式

其中，![t](https://math.jianshu.com/math?formula=t) 是目前迭代代数，![\vec A](https://math.jianshu.com/math?formula=%5Cvec%20A) 和 ![\vec C](https://math.jianshu.com/math?formula=%5Cvec%20C) 是系数向量，![\vec X_p](https://math.jianshu.com/math?formula=%5Cvec%20X_p) 和 ![\vec X](https://math.jianshu.com/math?formula=%5Cvec%20X) 分别是猎物的位置向量和灰狼的位置向量。  
![\vec A](https://math.jianshu.com/math?formula=%5Cvec%20A) 和 ![\vec C](https://math.jianshu.com/math?formula=%5Cvec%20C) 的计算公式如下

式（3） ![\vec A = 2\vec a.\vec r_1 - \vec a](https://math.jianshu.com/math?formula=%5Cvec%20A%20%3D%202%5Cvec%20a.%5Cvec%20r_1%20-%20%5Cvec%20a)  
式（4） ![\vec C = 2.\vec r_2](https://math.jianshu.com/math?formula=%5Cvec%20C%20%3D%202.%5Cvec%20r_2)

其中，![\vec a](https://math.jianshu.com/math?formula=%5Cvec%20a) 是收敛因子，随着迭代次数从 2 线性减小到 0，![\vec r_1](https://math.jianshu.com/math?formula=%5Cvec%20r_1) 和 ![\vec r_2](https://math.jianshu.com/math?formula=%5Cvec%20r_2) 的取模 \[0 ,1\] 之间的随机数。

#### 1.2 狩猎

灰狼能够识别猎物的位置并包围它们,当灰狼识别出猎物的位置后，β 和 δ 在 α 的带领下指导狼群包围猎物。  
在优化问题的决策空间中，我们对最佳解决方案（猎物的位置）并不了解。  
因此，为了模拟灰狼的狩猎行为，我们假设 α ，β 和 δ 更了解猎物的潜在位置。  
我们保存迄今为止取得的３个最优解决方案，并利用这三者的位置来判断猎物所在的位置，同时强迫其他灰狼个体（包括 ω ）依据最优灰狼个体的位置来更新其位置，逐渐逼近猎物。

![](https://upload-images.jianshu.io/upload_images/6881750-764746ad226b601f.png?imageMogr2/auto-orient/strip|imageView2/2/w/578/format/webp)

GWO 算法中灰狼位置更新示意图

灰狼个体跟踪猎物位置的数学模型描述如下:

式（5） ![\begin{cases} \vec D_\alpha = \begin{vmatrix} \vec C_1. \vec X_\alpha - \vec X \end{vmatrix} \\ \vec D_\beta = \begin{vmatrix} \vec C_2. \vec X_\beta - \vec X \end{vmatrix} \\ \vec D_\delta = \begin{vmatrix} \vec C_3. \vec X_\delta - \vec X \end{vmatrix} \end{cases}](https://math.jianshu.com/math?formula=%5Cbegin%7Bcases%7D%20%5Cvec%20D_%5Calpha%20%3D%20%5Cbegin%7Bvmatrix%7D%20%5Cvec%20C_1.%20%5Cvec%20X_%5Calpha%20-%20%5Cvec%20X%20%5Cend%7Bvmatrix%7D%20%5C%5C%20%5Cvec%20D_%5Cbeta%20%3D%20%5Cbegin%7Bvmatrix%7D%20%5Cvec%20C_2.%20%5Cvec%20X_%5Cbeta%20-%20%5Cvec%20X%20%5Cend%7Bvmatrix%7D%20%5C%5C%20%5Cvec%20D_%5Cdelta%20%3D%20%5Cbegin%7Bvmatrix%7D%20%5Cvec%20C_3.%20%5Cvec%20X_%5Cdelta%20-%20%5Cvec%20X%20%5Cend%7Bvmatrix%7D%20%5Cend%7Bcases%7D)

其中，  
![\vec D_\alpha, \vec D_\beta, \vec D_\delta](https://math.jianshu.com/math?formula=%5Cvec%20D_%5Calpha%2C%20%5Cvec%20D_%5Cbeta%2C%20%5Cvec%20D_%5Cdelta) 分别表示![\alpha , \beta , \delta](https://math.jianshu.com/math?formula=%5Calpha%20%2C%20%5Cbeta%20%2C%20%5Cdelta) 与其它个体间的距离。  
![\vec X_\alpha, \vec X_\beta, \vec X_\delta](https://math.jianshu.com/math?formula=%5Cvec%20X_%5Calpha%2C%20%5Cvec%20X_%5Cbeta%2C%20%5Cvec%20X_%5Cdelta) 分别代表 ![\alpha , \beta , \delta](https://math.jianshu.com/math?formula=%5Calpha%20%2C%20%5Cbeta%20%2C%20%5Cdelta) 的当前位置。  
![\vec C_1, \vec C_2, \vec C_3](https://math.jianshu.com/math?formula=%5Cvec%20C_1%2C%20%5Cvec%20C_2%2C%20%5Cvec%20C_3) 是随机向量，![\vec X](https://math.jianshu.com/math?formula=%5Cvec%20X) 是当前灰狼的位置。

式（6） ![\begin{cases} \vec X_1 = \vec X_\alpha - A_1. \vec D_\alpha \\ \vec X_2 = \vec X_\beta - A_2. \vec D_\beta \\ \vec X_3 = \vec X_\delta - A_3. \vec D_\delta \end{cases}](https://math.jianshu.com/math?formula=%5Cbegin%7Bcases%7D%20%5Cvec%20X_1%20%3D%20%5Cvec%20X_%5Calpha%20-%20A_1.%20%5Cvec%20D_%5Calpha%20%5C%5C%20%5Cvec%20X_2%20%3D%20%5Cvec%20X_%5Cbeta%20-%20A_2.%20%5Cvec%20D_%5Cbeta%20%5C%5C%20%5Cvec%20X_3%20%3D%20%5Cvec%20X_%5Cdelta%20-%20A_3.%20%5Cvec%20D_%5Cdelta%20%5Cend%7Bcases%7D)

式（7） ![\vec X(t+1) =\frac {\vec X_1+\vec X_2+\vec X_3} {3}](https://math.jianshu.com/math?formula=%5Cvec%20X(t%2B1)%20%3D%5Cfrac%20%7B%5Cvec%20X_1%2B%5Cvec%20X_2%2B%5Cvec%20X_3%7D%20%7B3%7D)

式（6）分别定义了狼群中 ω 的个体朝向 α ，β 和 δ 前进的步长和方向  
式（7）定义了 ω 的最终位置

#### 1.3 攻击猎物（开发）

当猎物停止移动时，灰狼通过攻击来完成狩猎过程。为了模拟逼近猎物，![\vec a](https://math.jianshu.com/math?formula=%5Cvec%20a) 的值被逐渐减小。因此![\vec A](https://math.jianshu.com/math?formula=%5Cvec%20A) 的波动范围也随之减小。换句话说，在迭代过程中，当![\vec a](https://math.jianshu.com/math?formula=%5Cvec%20a) 的值从2线性下降到0时，其对应的![\vec A](https://math.jianshu.com/math?formula=%5Cvec%20A) 的值也在区间 \[-a , a\]内变化。  
当![\vec A](https://math.jianshu.com/math?formula=%5Cvec%20A)的值位于区间内时，灰狼的下一位置可以位于其当前位置和猎物位置之间的任意位置。  
当 ![\begin{vmatrix} \vec A \end{vmatrix} < 1](https://math.jianshu.com/math?formula=%5Cbegin%7Bvmatrix%7D%20%5Cvec%20A%20%5Cend%7Bvmatrix%7D%20%3C%201) 时，狼群向猎物发起攻击（陷入局部最优）

![](https://upload-images.jianshu.io/upload_images/6881750-c3ceaf005a06f8d9.png?imageMogr2/auto-orient/strip|imageView2/2/w/620/format/webp)

攻击猎物和寻找猎物

#### 1.4 搜索猎物（勘探）

灰狼根据α ，β 和 δ 的位置和搜索猎物。灰狼在寻找猎物时彼此分开，然后聚集在一起攻击猎物。  
基于数据建模的散度，可以用 ![\vec A > 1](https://math.jianshu.com/math?formula=%5Cvec%20A%20%3E%201) 或 ![\vec A < -1](https://math.jianshu.com/math?formula=%5Cvec%20A%20%3C%20-1) 的随机值来迫使灰狼与猎物分离，这强调了探索并允许GWO算法全局搜索是解。  
GWO算法还有另一个组件![\vec C](https://math.jianshu.com/math?formula=%5Cvec%20C)来帮助发现新的解决方案。  
由式（4）可知，![\vec C](https://math.jianshu.com/math?formula=%5Cvec%20C) 是 \[0,2\] 之间的随机值。![C](https://math.jianshu.com/math?formula=C) 表示狼所在的位置对猎物影响的随机权重，![C> 1](https://math.jianshu.com/math?formula=C%3E%201)表示影响权重大，反之表示影响权重小。这有助于GWO算法更随机地表现并支持探索，同时可在优化过程中避免陷入局部最优。  
另外，![A](https://math.jianshu.com/math?formula=A) 不同 ， ![C](https://math.jianshu.com/math?formula=C) 是非线性减小的，这样从最初的迭代到最终的迭代中，它都提供了决策空间中的全局搜索。  
在算法陷入了局部最优并且不易跳出时，![C](https://math.jianshu.com/math?formula=C) 的随机性在避免局部最优方面发挥了非常重要的作用，尤其是在最后需要获得全局最优解的迭代中。

## 2.算法流程图

![](https://upload-images.jianshu.io/upload_images/6881750-1b9c918edab4dc4c.png?imageMogr2/auto-orient/strip|imageView2/2/w/628/format/webp)

流程图

## 3.算法结果

![](https://upload-images.jianshu.io/upload_images/6881750-90e1891b1d8b6ec6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1070/format/webp)

结果

  

作者：\_\_\_n  
链接：https://www.jianshu.com/p/1127c19852c8  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。