# 基于麻雀搜索算法3D无线传感器网络(WSN)覆盖优化
## 一、简介

麻雀搜索算法(Sparrow Search Algorithm, SSA)是于2020年提出的。SSA 主要是受麻雀的觅食行为和反捕食行为的启发而提出的。该算法比较新颖，具有寻优能力强，收敛速度快的优点。  
1 算法原理  
建立麻雀搜索算法的数学模型，主要规则如下所述：  
（1）发现者通常拥有较高的能源储备并且在整个种群中负责搜索到具有丰富食物的区域，为所有的加入者提供觅食的区域和方向。在模型建立中能量储备的高低取决于麻雀个体所对应的适应度值(Fitness Value)的好坏。  
（2）一旦麻雀发现了捕食者，个体开始发出鸣叫作为报警信号。当报警值大于安全值时，发现者会将加入者带到其它安全区域进行觅食。  
（3）发现者和加入者的身份是动态变化的。只要能够寻找到更好的食物来源，每只麻雀都可以成为发现者，但是发现者和加入者所占整个种群数量的比重是不变的。也就是说，有一只麻雀变成发现者必然有另一只麻雀变成加入者。  
（4）加入者的能量越低，它们在整个种群中所处的觅食位置就越差。一些饥肠辘辘的加入者更有可能飞往其它地方觅食，以获得更多的能量。  
（5）在觅食过程中，加入者总是能够搜索到提供最好食物的发现者，然后从最好的食物中获取食物或者在该发现者周围觅食。与此同时，一些加入者为了增加自己的捕食率可能会不断地监控发现者进而去争夺食物资源。  
（6）当意识到危险时，群体边缘的麻雀会迅速向安全区域移动，以获得更好的位置，位于种群中间的麻雀则会随机走动，以靠近其它麻雀。  
在模拟实验中，我们需要使用虚拟麻雀进行食物的寻找，由n只麻雀组成的种群可表示为如下形式：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322094852802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RJUUNtYXRsYWI=,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322094907618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RJUUNtYXRsYWI=,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322094918132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RJUUNtYXRsYWI=,size_16,color_FFFFFF,t_70)  
2 算法流程

Step1： 初始化种群，迭代次数，初始化捕食者和加入者比列。

Step2：计算适应度值，并排序。

Step3：利用式（3）更新捕食者位置。

Step4：利用式（4）更新加入者位置。

Step5：利用式（5）更新警戒者位置。

Step6：计算适应度值并更新麻雀位置。

Step7：是否满足停止条件，满足则退出，输出结果，否则，重复执行Step2-6；

## 二、源代码

```c
clear all 
clc
rng('default');
%% 设定WNS覆盖参数,
%% 默认输入参数都是整数，如果想定义小数，请自行乘以系数变为整数再做转换。
%% 比如范围1*1，R=0.03可以转换为100*100，R=3；
%区域范围为AreaX*AreaY*AreaZ
AreaX = 100;
AreaY = 100;
AreaZ = 100;
N = 20 ;%覆盖节点数
R = 15;%通信半径

%% 设定麻雀优化参数
pop=30; % 种群数量
Max_iteration=30; %设定最大迭代次数
lb = ones(1,3*N);
ub = [AreaX.*ones(1,N),AreaY.*ones(1,N),AreaZ.*ones(1,N)];
dim = 3*N;%维度为3N，N个坐标点
fobj = @(X)fun(X,N,R,AreaX,AreaY,AreaZ);%适应度函数
[Best_pos,Best_score,SSA_curve]=SSA(pop,Max_iteration,lb,ub,dim,fobj); %开始优化
%_________________________________________________________________________%
% 麻雀优化算法 %
%_________________________________________________________________________%
function [Best_pos,Best_score,curve]=SSA(pop,Max_iter,lb,ub,dim,fobj)

ST = 0.6;%预警值
PD = 0.7;%发现者的比列，剩下的是加入者
SD = 0.2;%意识到有危险麻雀的比重

PDNumber = round(pop*PD); %发现者数量
SDNumber = round(SD*PD);%意识到有危险麻雀数量
if(max(size(ub)) == 1) ub = ub.*ones(1,dim); lb = lb.*ones(1,dim);  
end

%种群初始化
X0=initialization(pop,dim,ub,lb);
X = X0;
%计算初始适应度值
fitness = zeros(1,pop);
for i = 1:pop fitness(i) =  fobj(X(i,:));
end
 [fitness, index]= sort(fitness);%排序
BestF = fitness(1);
WorstF = fitness(end);
GBestF = fitness(1);%全局最优适应度值
for i = 1:pop X(i,:) = X0(index(i),:);
end
curve=zeros(1,Max_iter);
GBestX = X(1,:);%全局最优位置
X_new = X;
for i = 1: Max_iter disp(['第',num2str(i),'次迭代']); BestF = fitness(1); WorstF = fitness(end); R2 = rand(1); for j = 1:PDNumber if(R2<ST) X_new(j,:) = X(j,:).*exp(-j/(rand(1)*Max_iter)); else X_new(j,:) = X(j,:) + randn()*ones(1,dim); end end for j = PDNumber+1:pop
% if(j>(pop/2)) if(j>(pop - PDNumber)/2 + PDNumber) X_new(j,:)= randn().*exp((X(end,:) - X(j,:))/j^2); else %产生-1，1的随机数 A = ones(1,dim); for a = 1:dim if(rand()>0.5) A(a) = -1; end end AA = A'*inv(A*A'); X_new(j,:)= X(1,:) + abs(X(j,:) - X(1,:)).*AA'; end end Temp = randperm(pop); SDchooseIndex = Temp(1:SDNumber); for j = 1:SDNumber if(fitness(SDchooseIndex(j))>BestF) X_new(SDchooseIndex(j),:) = X(1,:) + randn().*abs(X(SDchooseIndex(j),:) - X(1,:)); elseif(fitness(SDchooseIndex(j))== BestF) K = 2*rand() -1; X_new(SDchooseIndex(j),:) = X(SDchooseIndex(j),:) + K.*(abs( X(SDchooseIndex(j),:) - X(end,:))./(fitness(SDchooseIndex(j)) - fitness(end) + 10^-8)); end end %边界控制 for j = 1:pop for a = 1: dim if(X_new(j,a)>ub) X_new(j,a) =ub(a); end if(X_new(j,a)<lb) X_new(j,a) =lb(a); end end end %更新位置 for j = 1:pop if(fitness_new(j) < GBestF) GBestF = fitness_new(j); GBestX = X_new(j,:); end end X = X_new; fitness = fitness_new; %排序更新 [fitness, index]= sort(fitness);%排序 BestF = fitness(1); WorstF = fitness(end); for j = 1:pop X(j,:) = X(index(j),:); end

  
```

## 三、运行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322094706651.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RJUUNtYXRsYWI=,size_16,color_FFFFFF,t_70#pic_center)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210322094706638.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RJUUNtYXRsYWI=,size_16,color_FFFFFF,t_70#pic_center)

## 四、备注