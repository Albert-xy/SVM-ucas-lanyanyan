# SVM-ucas-lanyanyan
2018年国科大模式识别与机器学习兰艳艳-SVM  
@2018/15  
@authon xy  

鉴于本人时间精力和能力有限，若有错误，请不吝告知

# 1.History
创始人及主要的贡献人Vapnik  
Linear SVM-> Kernelized SVM-> SVR...SMO...  
# 2.从Linear线性分类器说起
对于二维平面上二类划分问题  
![图1](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/linear-clasifier.png)  
- 选择分类面的标准：**泛化误差最小**，可以简单理解为要使得两类之间间隔足够大   
- 黑色那条就是最优的分类线，Q1间隔怎么定义？Q2如何以数学的形式刻画这个最优的分类超平面？  
#  3.Margin(边际或者边距)  
以logistic回归问题为例  
![图2](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LR-1.png)    
- P(y=1|x)越大，我们越有**自信**说x对应y的lable为1，这里是基于概率上的**自信**  
- 在一个没有概率的分类器上应该怎么定义**Q1**，从图1上来看**自信**就对应点到分类面的距离   
- **自信**其实的就是分类面的边距即margin
  
## 函数间距  
- 回到2.线性分类器上：*f(x)=wx+b,f(x)>0,lable=1(正例);f(x)<0,lable=-1*(反例)   
- 分类的超平面(实际是条线):*wx+b=0*  
- *γ=min y(wx+b)*,点到分类面的距离**最小**的那个，这些点也称为支持向量，后面说  

![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LR-2.png)  
## 几何间距  
### 简单的推导（徐君）：  

![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LR-3.png)      
- ___wx1+b = 1,wx2+b = -1 => w(x1-x2) = 2,以x1(向量)替代x+，x2替代x-<1>___
- ___x1 = x2+λw => x1-x2 = λw___ ___<2>___
- ___由1和2推出：2/w=λw => λ = 2/(w)^2___
- ___分类间隔Margin = |x1-x2| = |λw| = 2/(w)^2 * |w|= 2/|w|___  
- ___最大化Margin即max 2/|w| 等价于 min|w|^2(有的乘以1/2，常数无所谓)___
### 简单的推导（兰艳艳）：  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LR-5.png) 
- 点A（xi,yi）,A到分类面的边距γi（向量）,γi是第i个点到分类面的距离
- 点B用A表示xj=xi-γi(w/|w|2)，将其代入wx+b=0得
- ___w(xi-γi(w/|w|2))+b=0 => γi=(w/|w|2)xi+b/|w|2___  
- ___γi是个向量有方向，为了使结果为正乘以yi，γi = yi((w/|w|2)xi+b/|w|2)___
- ___最终的Margin就是γ=min γi___
#### 函数边际和几何边际的关系
- ___γ = γ^/(|w|2)当(|w|2)=1时两者相等___ 
#### 最终的基于margin线性分类器
- ___1.转换为函数边际：max γ <=> max γ^/(|w|2),s.t.yi((w/|w|2)xi+b/|w|2)≥γ^___
- ___2.消去γ^,对w,b进行放缩对最后结果没有影响：min 1/2*(|w|2)^2   s.t.yi((w/|w|2)xi+b/|w|2)≥1___
- 目标函数是凸函数，通过二次规划QP可求解  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LR-4.png)   
- 通过一个符号函数sign进行最后的分类：  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LSVM-1.png)
# 4.支持向量
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LSVM-2.png)  
- 定义：训练样本中那些最接近分类面的。几何边距中的min γ
- 对于支持向量来说：*y(wx+b)=1*  
- 函数边际：![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LSVM-3.png)  
- 几何边际：![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LSVM-4.png)
- Margin：![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/LSVM-5.png)  
- 事实上从几何边际的角度来理解，只有支持向量会对最后的分类面有影响，因为它们到分类面的margin最小，它们支撑着分类面  
# 5.目标函数求解  
由于直接对原始目标函数进行求解(w,b)太过复杂，通过拉格朗日变换，求解相对容易的对偶问题。关于原始问题和对偶问题，可以看卜东波老师算法课对偶问题一节，讲的十分详细，特别精彩，将对偶问题和原始问题的方方面面都讲的特别清楚，当时眼神中看着的都是发光的卜老师，bling bling那种~  
https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/Lec9.pdf  
https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/Lec9-Duality-SVM2.pdf  
## 简单的原始-对偶问题介绍  
**原始问题**  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-1.png)  
- 原始问题转换为拉格朗日形式（先不考虑minmize问题）  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-2.png)  
- α和β称作拉格朗日乘子，在对偶问题中实际上对应的是y，或者影子价格之类的  
- **考虑拉格朗日形式的性质**  
- 定义max L(w)拉格朗日形式  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-3.png)  
- 如果约束条件中g和h中有一个不满足，h和g大于0，那么乘以任意大α,β，结果会是正无穷  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-4.png)  
- 如果约束条件都满足，那么  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-5.png)  
- 对朗格朗日形式加上minimize问题就等于原始问题了  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-6.png)  
- **拉格朗日对偶**  
- 定义问题 minmize L(w,α,β)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-8.png)  
- 对偶问题  max min形式  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-7.png)  
- 可以看出来对偶问题始终小于等于原始问题，两者之间存在gap，强对偶时取等号  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-10.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-9.png)  
- 当拉格朗日对偶问题满足KKT条件，并且f和g函数都是凸，h函数仿射(拟凸？)，对偶问题解等于原始问题的解  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/pd-11.png)  
- 互补松弛性定义了**支持向量**，当g不等式严格成立时（wx+b=1）,即点在margin上，此时α>0，其余α=0，说明只有**支持向量**对margin起作用  
## 回到SVM目标函数求解上来 
- 目标函数写为拉格朗日形式  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-1.png)  
- 对拉格朗日形式求偏导w,b  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-2.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-3.png)  
- 将上两式结果代入原拉格朗日形式得  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-4.png)  
- 对偶问题的最终形式  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-5.png)  
- 可以通过固定n-2个αi，求两个αi,求得最终的α\*  
- 通过对w求偏导的等式和wx+b=y，求得w\*,b\*   
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-6.png)  
- 最终的分类面和分类函数为（关键注意到后边xi的转置乘以xj）  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-7.png)![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/svm-8.png)
- 从上面可以看出αi>0只对支持向量，其余向量不起作用，一个更好的形式是求w\*和b\*，只考虑向量的內积(=+-1)，这一点对核函数非常重要  
**SVM核心思想到这里已经结束了**  
# 5.软边距  
真实数据中也可能存在线性不可分或者存在噪声的情况，如下图  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-1.png)  
- 可以对分错的样本增加一个松弛变量**ξ**容忍这种情况，也称为软边距  
- 优化的目标函数形式为  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-2.png)  
- 通过拉格朗日对偶变换得到L，分别对w,b和ξ求偏导得到  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-3.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-4.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-5.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-6.png)  
- 将以上带回原拉格朗日形式得：  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-7.png)  
- 根据KKT条件和互补松弛性，支持变量有以下性质：  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-8.png)  
- 证明如下:  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-9.png)  
## 参数C的作用  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-10.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-11.png)  
- 从上图可以看出当C增大，边距Margin变小，称C为惩罚因子，变大意味着惩罚越大，对分错的数据更加严苛  
## 损失函数  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-13.png)  
- 可以看出和ERM很像，λ和C等价（C=2/λ）  
- 损失函数是henge形式  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-12.png)  
- 普通二分类是0-1损失，logistic回归是log损失，其实本质都是寻找0-1损失函数上界    
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/softM-14.png)  
# 6.核函数
- 在很多情况下，其实样本在二维空间上并非线性可分。比如树叶投射到地面上的影子是不可分的，但是在树上，我们可能会找到这么一个平面把叶子分开（老师的例子，哈哈哈  
## idea的来源  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-1.png)  
- 从几何上看，将x通过 z = φ(x) 映射到高维空间(特征空间)，寻找超平面  
- 从目标函数自身的特性来看，优化目标只与<x,x>內积有关。超平面w\*x+b\*=0,w\*和b\*也只与內积有关  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-2.png)  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-3.png)  
- 从非参数方法角度（基于经验的方法，比如KNN），內积计算就是计算相似度  
## Kernal  
为什么需要核函数而不是映射函数φ(x)呢？  

**因为在高维空间上计算內积非常复杂且困难，所以我们是否可以在不知道φ函数（映射空间）具体是什么的情况下直接求得內积呢（核技巧）？答案是肯定的**  
- 定义：给输入空间X和希尔伯特空间H（一个带有內积的完备空间，和欧式空间相仿），如果存在一个映射φ，使得X->H，K(x,z)=<φ(x),φ(z)>,K就是一个核函数  
- 给定K(x,z),映射函数φ和H通常不唯一  

![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-4.png)  
- 引入核函数的SVM目标函数  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-5.png)  

![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-6.png)  
- 什么样的kernal函数是合法的呢？
- Mercer定理：对任意m个样本，核函数矩阵总是半正定的。  任何一个核函数都隐式的定义了一个RKHS（再生核希尔伯特空间）  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-7.png)  
- 一些常用的核函数有多项式核、高斯核  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-8.png)  

![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/kernalsvm-9.png)  
# 7.SMO算法
出发点：由于α参数太多，直接计算复杂且慢
- 思想：每次固定两个参数，固定一个不可以，因为最优解要满足KKT条件。只选择一个参数的话是个定值，不需优化  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/smo-2.png)  
- 1.选择α1的标准：最违背kkt条件的；先选边际上的，在选择分错的样本  
- 2.选择α2的标准：使得α2改变最多（梯度下降可求）  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/smo-3.png)  
## 最终的算法  
![](https://github.com/Albert-xy/SVM-ucas-lanyanyan/blob/master/imp/smo-1.png)
