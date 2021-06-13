> 版权声明：本文为CSDN博主「~心升明月~」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_43821559/article/details/113481584

## 文章目录
**一、理论基础**
	1、LEACH协议描述
	2、改进后的LEACH协议
		（1）簇头判断
		（2）簇头选取
		（3）数据传输阶段
**二、算法流程**
**三、仿真与分析**
	1、仿真结果
	2、MATLAB程序实现
**四、参考文献**

---

# 一、理论基础
## 1、LEACH协议描述
LEACH 协议定义了“轮”的概念， 协议以轮为周期执行， 每一轮的过程包括分簇和稳定两个阶段。在分簇阶段， WSN 各个节点随机生成一个0～1之间的数字， 然后将该数字与一个设定的阈值公式得出的值进行比较， 若该随机数小于阈值， 并且该节点在前1 / p 1/p1/p(p pp为当前轮中节点成为簇头的比例)轮内未当选为簇头节点， 则该节点被选为本轮的簇头节点。其中， 阈值的算式为：

![[Pasted image 20210524122725.png]]
 
式中：`p`为成为簇头的期望百分比；`r`为当前轮数；`mod`为取模符号；`G`为在最近1/p轮中还未成为簇头的节点集合。

当簇头选取完毕后， 簇头向周围节点广播自身的簇头状态、 ID 等信息， 节点根据接收到的消息强度决定加入哪个簇， 并告知相应的簇头， 完成簇头的建立过程。然后， 簇头节点采用TDMA的方式， 为簇内成员分配传送数据的时隙。
在稳定阶段， 传感器节点价格采集的数据传送到簇头节点。簇头节点对采集的数据进行数据融合后再将信息以单跳方式传送到汇聚节点， 按照不同的 CDMA代码直接发送给基站。
但是， LEACH 协议存在以下问题：

1. LEACH 协议算法中， 簇头的选取是随机的， 而没有考虑到节点当前的能量， 有可能剩余能量很小的节点仍然被选为簇头， 导致该节点过早死亡， 引发再组网等一系列问题， 降低网络生存周期；
2. 簇头与基站之间直接单跳通信， 当距离较远时， 能量消耗成指数增长， 造成簇头节点过早死亡， 进而降低网络生命周期；
3. 簇头的选取无法保证节点在空间上均匀分布，在某些可能的情况下， 形成的簇头节点可能聚集在某一个小范围内， 导致某些节点无法加入任何簇。
## 2、改进后的LEACH协议
改进后的 LEACH 协议(IMP-LEACH) 与 LEACH算法有如下相同的网络模型：

1. 传感器节点随机分布在某一片方形区域， 基站唯一；
2. 部署之后所有节点(包括基站)的位置不变；
3. 各个传感器节点的初始能量相同， 且已知节点任何时刻的剩余能量， 基站能量不受限。

在上面的模型中， 文献[5]给出了几个定义：邻居节点、前邻节点、前簇头节点。具体定义如下：
（1）邻居节点：![[Pasted image 20210524123129.png]]
其中V 是所有传感器的集合，R是节点通信半径，d(i,j)是节点𝑖到节点 𝑗的距离。
（2）前邻节点：![[Pasted image 20210524123116.png]]
其中d(i,BS)是节点i 到基站的距离。
（3）前簇头节点：![[Pasted image 20210524123225.png]]
其中是d(CH_i,BS)簇头节点CH_i到基站的距离。

### （1）簇头判断
在判断是否为簇头的过程中，加入节点当前能量这个考量因素，提出如
下公式：

![[Pasted image 20210524123333.png]]

其中，p 为成为簇首的期望百分比；r 为当前轮数；G 为在最近1/p轮中还未成为簇首的节点集合，E_current为节点i当前能量，E_{avg} 为系统节点当前平均能量，$\alpha$ 定义为能量距离因子，取值为2或者4，取决于节点间距离d。

### （2）簇头选取
在簇头选取的过程中，考虑到不同簇头节点能量的不同，会导致其通信距离的不同，同时，不同能量的簇头节点，其所能容纳的节点数也不同，在节点选择加入簇头的过程中，综合考虑以上两点，给出如下定义：
对于一个特定的节点𝑖，其所能加入的簇头节点集合 $CH_s$ 需满足以下两个条件：
条件一：![[Pasted image 20210524123616.png]]
其中，d ( i , C H s ) < R d(i,CH_s)<Rd(i,CHs)<R表示簇头C H s CH_sCH s
 到节点i ii的距离在节点的通信半径之内，V VV表示所有簇头节点集合。
条件二：在满足条件一的情况下，进行如下运算：![[Pasted image 20210524123714.png]]
其中 {CH_i|CH_i∈CH_s}，E_{min}	表示使节点存活所需要的最小能量，E_{CH_i}
 表示簇头节点i,当前剩余能量，n 表示当前加入簇头节点i 的节点个数，E(i,n)表示当有n nn个节点加入簇头节点i ii时，每个节点所能分配的能量。通过计算可以得到一系列的E(i,n)值，则当E(i,n)取最大值时节点选择加入，至此，节点i ii的簇头选取过程结束。

### （3）数据传输阶段
参考文献[1]。

## 二、算法流程

![[20210131183713493.png]]

图1 算法流程图
## 三、仿真与分析
实验采用 MATLAB 模拟运行对改进后的算法，同时将仿真结果与经典LEACH 算法进行比较。实验中用到的参数与文献[1]相同。

### 1、仿真结果
存活节点数量比较、剩余能量比较：

![[20210131191557600.png]]

图2 存活节点个数比较

![[20210131191613190.png]]

图3 剩余能量比较
死亡节点比例比较如图4所示。

![[20210131182257147.png]]

图4 死亡节点比例比较
从仿真结果可以得知，改进后的LEACH协议（IMP-LEACH）在相同循环次数下存活节点个数明显比改进之前的多，并且从横轴可以得知，原LEACH协议在大约1625次循环之后就再无存活结点了，而改进后的协议这一数值提高到了1806，说明改进后的协议能大大地延缓节点死亡时间；通过仿真结果导出的数据所制作的节点死亡比例与循环轮数的关系图同样可以看出改进后的协议在延长节点寿命方面有突出提升，尤其是进入传感器网络运行后期阶段，这种差异更加明显。

2、MATLAB程序实现
具体代码如下：
```matlab
%% 清空环境变量
clear;
clc;

%% 初始化参数
xm = 100;                        % x轴范围
ym = 100;                        % y轴范围
sink.x = 50;                     % 基站x轴 
sink.y = 125;                    % 基站y轴 
n = 400;                         % 节点总数
p = 0.08;                        % 簇头概率
Eelec = 50*10^(-9);
Efs=10*10^(-12);
Emp=0.0013*10^(-12);
ED=5*10^(-9);
d0 = 87;
packetLength = 4000;
ctrPacketLength = 100;
rmax = 2000;
E0 = 0.5;                   % 初始能量
Emin = 0.001;               % 节点存活所需的最小能量
Rmax = 15;                  % 初始通信距离
figure;
%% 节点随机分布
for i = 1:n
    Node(i).xd = rand(1,1)*xm;
    Node(i).yd = rand(1,1)*ym;       % 随机产生100个点
    Node(i).d = sqrt((Node(i).xd-sink.x)^2+(Node(i).yd-sink.y)^2);  % 节点距基站的距离
    Node(i).Rc = Rmax;               % 节点的通信距离
    Node(i).temp_rand = rand;        % rand为(0,1)的随机数
    Node(i).type = 'N';              % 进行选举簇头前先将所有节点设为普通节点
    Node(i).selected = 'N';
    Node(i).power = E0;              % 初始能量
    Node(i).CH = 0;                  % 保存普通节点的簇头节点，-1代表自己是簇头
    Node(i).flag = 1;                % 1代表存活；0代表死亡         
    Node(i).N = zeros(1, n);         % 邻居节点集
    Node(i).Num_N = 0;               % 邻居节点集个数
    Node(i).FN = zeros(1, n);        % 前邻节点集
    Node(i).Num_FN = 0;              % 前邻节点集个数
    Node(i).CN = zeros(1, n);        % 前簇头节点集
    Node(i).Num_CN = 0;              % 前簇头节点集个数
    plot(Node(i).xd, Node(i).yd, 'o', sink.x, sink.y, 'p', 'LineWidth', 2);
    hold on;
end
legend('节点', '基站');
xlabel 'x'; ylabel 'y'; title 'WSN分布图';
save data;
flag = 1;
%%%%%%%%%%%%%%%%IMP_LEACH%%%%%%%%%%%%%%%%%%
%% 迭代
alive_ima_leach = zeros(rmax, 1);        % 每轮存活节点数
re_ima_leach = zeros(rmax, 1);           % 每轮节点总能量
for r = 1:rmax
%     figure;
    for i = 1:n
        if Node(i).flag ~= 0
            re_ima_leach(r) = re_ima_leach(r)+Node(i).power;
            alive_ima_leach(r) = alive_ima_leach(r)+1;
        end
    end
    f = 0;
    if alive_ima_leach(r) == 0
        stop = r;
        f = 1;
        break;
    end
    for i = 1:n
        Node(i).type = 'N';              % 进行选举簇头前先将所有节点设为普通节点
        Node(i).selected = 'N';
        Node(i).temp_rand = rand;        % 节点取一个(0,1)的随机值，与p比较
        Node(i).Rc = Rmax*Node(i).power/E0;   % 节点的通信距离
        Node(i).CH = 0;
        Node(i).N = zeros(1, n);         % 邻居节点集
        Node(i).Num_N = 0;               % 邻居节点集个数
        Node(i).FN = zeros(1, n);        % 前邻节点集
        Node(i).Num_FN = 0;              % 前邻节点集个数
        Node(i).CN = zeros(1, n);        % 前簇头节点集
        Node(i).Num_CN = 0;              % 前簇头节点集个数
        Node(i).num_join = 1;            % 簇成员的个数
    end
    %% 簇头选举
    count = 0;    % 簇头个数
    for i = 1:n
        if  Node(i).selected == 'N' && Node(i).flag ~= 0
            if Node(i).d > d0
                alpha = 4;      % 能量损失指数
            else
                alpha = 2;
            end
            Eavg = 0;       % 系统节点平均能量
            m = 0;           % 存活节点个数
            for j = 1:n
                if Node(i).flag ~= 0
                    Eavg = Eavg+Node(i).power;
                    m = m + 1;
                end
            end
            if m ~= 0
                Eavg = Eavg/n;
            else
                break;
            end
            if Node(i).temp_rand <= (p/(1-p*mod(r,round(1/p))*(Node(i).power/Eavg)^(1/alpha))) && Node(i).d > Node(i).Rc
                Node(i).type = 'C';      % 节点类型为簇头
                Node(i).selected = 'O';  % 该节点标记'O'，说明当选过簇头
                Node(i).CH = -1;
                count = count + 1;
                final_CH(count) = i;
                % 广播自成为簇头
                distanceBroad = sqrt(Node(i).Rc^2+Node(i).Rc^2);
                if (distanceBroad > d0)
                    Node(i).power = Node(i).power- (Eelec*ctrPacketLength+Emp*ctrPacketLength*distanceBroad^4);
                else
                    Node(i).power = Node(i).power- (Eelec*ctrPacketLength+Efs*ctrPacketLength*distanceBroad^2);
                end
%                 plot(Node(i).xd,Node(i).yd,'*'); % 簇头节点以*标记
%                 hold on;
%                 text(Node(i).xd,Node(i).yd,num2str(i));
            else
                Node(i).type = 'N';      %节点类型为普通
%                 plot(Node(i).xd, Node(i).yd, 'o');   %普通节点以o标记
%                 hold on;
%                 text(Node(i).xd, Node(i).yd, num2str(i));
            end
        end
    end
    % 计算邻居节点集合
    for i = 1:n
        cnt = 0;
        for j = 1:n
            if i ~= j
                dist = sqrt((Node(i).xd-Node(j).xd)^2+(Node(i).yd-Node(j).yd)^2);
                if dist < Node(i).Rc
                    cnt = cnt + 1;
                    Node(i).N(cnt) = j;
                end
            end
            if j == n
                Node(i).Num_N = cnt;
            end
        end
    end
    % 计算前邻节点集
    for i = 1:n
        cnt = 0;
        for j = 1:Node(i).Num_N
            ne = Node(i).N(j);
            if Node(ne).d < Node(i).d
                cnt = cnt + 1;
                Node(i).FN(cnt) = ne;
            end
            if j == Node(i).Num_N
                Node(i).Num_FN = cnt;
            end
        end
    end
    % 计算前簇头节点集
    for i = 1:count
        cnt = 0;
        for j = 1:Node(i).Num_FN
            fne = Node(final_CH(i)).FN(j);
            if fne ~= 0 && Node(fne).d < Node(final_CH(i)).d && Node(fne).CH == -1
                cnt = cnt + 1;
                Node(final_CH(i)).CN(cnt) = fne;
            end
            if j == Node(i).Num_FN
                Node(final_CH(i)).Num_CN = cnt;
            end
        end
    end
    %% 加入簇
    for i = 1:n
        if Node(i).type == 'N' && Node(i).power > 0
            E = zeros(count, 1);
            for j = 1:count
                dist = sqrt((Node(i).xd-Node(final_CH(j)).xd)^2+(Node(i).yd-Node(final_CH(j)).yd)^2);
                if dist < Node(final_CH(j)).Rc    % 满足条件1
                    E(j) = (Node(final_CH(j)).power-Emin)/Node(final_CH(j)).num_join;
                end
            end
            [max_value, max_index] = max(E);
            % 节点发送加入簇的消息
            if exist('final_CH') ~= 0 
                dist = sqrt((Node(i).xd-Node(final_CH(max_index)).xd)^2+(Node(i).yd-Node(final_CH(max_index)).yd)^2);
                if dist > Node(final_CH(max_index)).Rc   % 不满足条件1，选择最近的簇头加入
                    Length = zeros(count, 1);
                    for j = 1:count
                        Length(j) = sqrt((Node(i).xd-Node(final_CH(j)).xd)^2+(Node(i).yd-Node(final_CH(j)).yd)^2);
                    end
                    [min_value, min_index] = min(Length);
                    Node(i).CH = final_CH(min_index);
                    % 节点发送加入簇的消息
                    Node(i).power = Node(i).power - (Eelec*ctrPacketLength+Efs*ctrPacketLength*dist^2);
                    % 簇头接收消息
                    Node(final_CH(min_index)).power = Node(final_CH(min_index)).power-Eelec*ctrPacketLength;
                    Node(final_CH(min_index)).num_join = Node(final_CH(min_index)).num_join+1;
                    %                 plot([Node(final_CH(min_index)).xd; Node(i).xd], [Node(final_CH(min_index)).yd; Node(i).yd]);  % 将节点与簇头连起来，即加入簇头集合
                    %                 hold on;
                else
                    % 节点发送加入簇的消息
                    Node(i).power = Node(i).power - (Eelec*ctrPacketLength+Efs*ctrPacketLength*dist^2);
                    % 簇头接收消息
                    Node(final_CH(max_index)).power = Node(final_CH(max_index)).power-Eelec*ctrPacketLength;
                    Node(final_CH(max_index)).Rc = Rmax*Node(final_CH(max_index)).power/E0;
                    Node(i).CH = final_CH(max_index);
                    Node(final_CH(max_index)).num_join = Node(final_CH(max_index)).num_join+1;
                    %                 plot([Node(final_CH(max_index)).xd; Node(i).xd], [Node(final_CH(max_index)).yd; Node(i).yd]);  % 将节点与簇头连起来，即加入簇头集合
                    %                 hold on;
                end
            end
        end
    end
    %% 能量模型
    % 发送数据
    for i = 1:n
        if Node(i).flag ~= 0
            if Node(i).type == 'N' && Node(i).CH ~= 0   % 普通节点
                dist = sqrt((Node(i).xd-Node(Node(i).CH).xd)^2+(Node(i).yd-Node(Node(i).CH).yd)^2);
                if dist > d0
                    Node(i).power = Node(i).power-(Eelec*packetLength+Emp*packetLength*dist^4);
                else
                    Node(i).power = Node(i).power-(Eelec*packetLength+Efs*packetLength*dist^2);
                end
            else                             % 簇头节点
                Node(i).power = Node(i).power-(Eelec+ED)*packetLength;      % 簇头接收数据
                if Node(i).d <= Node(i).Rc
                    Node(i).power = Node(i).power-(Eelec*packetLength+Efs*packetLength*Node(i).d^2);
                else
                    if Node(i).Num_CN == 0
                        if Node(i).d > d0
                            Node(i).power = Node(i).power-(Eelec*packetLength+Emp*packetLength*Node(i).d^4);
                        else
                            Node(i).power = Node(i).power-(Eelec*packetLength+Efs*packetLength*Node(i).d^2);
                        end
                    else
                        % 选择中继节点
                        dis = zeros(Node(i).Num_CN, 1);
                        % 计算前簇头节点距基站的距离
                        for j = 1:Node(i).Num_CN
                            dis(j) = Node(Node(i).CN(j)).d;
                        end
                        [~, index] = sort(dis);
                        di = dis(index);
                        % 中继转发
                        for j = 1:Node(i).Num_CN
                            Node(i).power = Node(i).power - di(j)/sum(di)*(Eelec*packetLength+Emp*packetLength*di(Node(i).Num_CN+1-j)^2);
                        end
                    end
                end
            end
        end
    end
    for i = 1:n
        if Node(i).power < Emin
            Node(i).flag = 0;
        end
    end
    clear final_CH;
end
if f == 0
    stop = rmax;
end
load data.mat;
%%%%%%%%%%%%%%%%LEACH%%%%%%%%%%%%%%%%%%
%% 
alive_leach = zeros(rmax, 1);        % 每轮存活节点数
re_leach = zeros(rmax, 1);           % 每轮节点总能量
for r = 1:rmax
%     figure;
    for i = 1:n
        if Node(i).flag ~= 0
            re_leach(r) = re_leach(r)+Node(i).power;
            alive_leach(r) = alive_leach(r)+1;
        end
    end
    f = 0;
    if alive_leach(r) == 0
        stop = r;
        f = 1;
        break;
    end
    for i = 1:n             
        Node(i).type = 'N';                  % 进行选举簇头前先将所有节点设为普通节点
        Node(i).selected = 'N';
        Node(i).temp_rand = rand;  %节点取一个(0,1)的随机值，与p比较
    end
    for i = 1:n
        if  Node(i).selected == 'N' && Node(i).flag ~= 0
            % if  Node(i).type=='N' %只对普通节点进行选举，即已经当选簇头的节点不进行再选举
            if Node(i).temp_rand <= (p/(1-p*mod(r,round(1/p)))) % 选取随机数小于等于阈值，则为簇头
                Node(i).type = 'C';      % 节点类型为蔟头
                Node(i).selected = 'O';  % 该节点标记'O'，说明当选过簇头
                Node(i).CH = -1;
                % 广播自成为簇头
                distanceBroad = sqrt(xm*xm+ym*ym);
                if distanceBroad > d0
                    Node(i).power = Node(i).power- (Eelec*ctrPacketLength + Emp*ctrPacketLength*distanceBroad^4);
                else
                    Node(i).power = Node(i).power- (Eelec*ctrPacketLength + Efs*ctrPacketLength*distanceBroad^2);
                end
%                 plot(Node(i).xd,Node(i).yd,'*'); % 簇头节点以*标记
%                 hold on;
%                 text(Node(i).xd,Node(i).yd,num2str(i));
            else
                Node(i).type = 'N';      %节点类型为普通
%                 plot(Node(i).xd, Node(i).yd, 'o');   %普通节点以o标记
%                 hold on;
%                 text(Node(i).xd, Node(i).yd, num2str(i));
            end
        end
    end
    % 判断最近的簇头结点，如何去判断，采用距离矩阵
    yy = zeros(n);
    for i = 1:n
        if Node(i).type == 'N' && Node(i).flag ~= 0
            for j = 1:n
                if Node(j).type == 'C' && Node(j).flag ~= 0    % 计算普通节点到簇头的距离
                    Length(i, j) = sqrt((Node(i).xd-Node(j).xd)^2+(Node(i).yd-Node(j).yd)^2);
                else
                    Length(i, j) = inf;
                end
            end
            [dist, index] = min(Length(i, :));    % 找到距离簇头最近的簇成员节点
            % 加入这个簇
            if Length(i, index) < d0
                Node(i).power = Node(i).power-(Eelec*ctrPacketLength+Efs*ctrPacketLength*Length(i, index)^2);
            else
                Node(i).power = Node(i).power-(Eelec*ctrPacketLength+Emp*ctrPacketLength*Length(i, index)^4);
            end
            Node(i).CH = index;
            % 接收簇头发来的广播的消耗
            Node(i).power = Node(i).power - Eelec*ctrPacketLength;
            % 对应簇头接收确认加入的消息
            Node(index).power = Node(index).power-Eelec*ctrPacketLength;
%             plot([Node(index).xd; Node(i).xd], [Node(index).yd; Node(i).yd]);  % 将节点与簇头连起来，即加入簇头集合
%             hold on;
            yy(i, index)=1;
        else
            Length(i, :) = inf;
        end
    end
    for i = 1:n
        if Node(i).flag ~= 0
            if Node(i).type == 'C'
                number = sum(yy(:, i));     % 统计簇头节点i的成员数量
                % 簇头接收普通节点发来的数据
                Node(i).power = Node(i).power-(Eelec+ED)*packetLength;
                % 簇头节点向基站发送数据
                len = sqrt((Node(i).xd-sink.x)^2+(Node(i).yd-sink.y)^2);
                if len < d0
                    Node(i).power = Node(i).power-((Eelec+ED)*packetLength+Efs*packetLength*len^2);
                else
                    Node(i).power = Node(i).power-((Eelec+ED)*packetLength+Emp*packetLength*len^4);
                end
            else
                % 普通节点向簇头发数据
                len = Length(i, Node(i).CH);
                if len < d0
                    Node(i).power = Node(i).power-(Eelec*packetLength+Efs*packetLength*len^2);
                else
                    Node(i).power = Node(i).power-(Eelec*packetLength+Emp*packetLength*len^4);
                end
            end
        end
    end
    for i = 1:n
        if Node(i).power < 0
            Node(i).flag = 0;
        end
    end
end
if f == 0
    stop = rmax;
end
%% 绘图显示
figure;
plot(1:rmax, alive_ima_leach, 'r', 1:rmax, alive_leach, 'b', 'LineWidth', 2);
legend('IMP\_LEACH', 'LEACH');
xlabel '轮数'; ylabel '存活节点数';
figure;
plot(1:rmax, re_ima_leach, 'r', 1:rmax, re_leach, 'b', 'LineWidth', 2);
legend('IMP\_LEACH', 'LEACH');
xlabel '轮数'; ylabel '系统总能量';
figure;
for r = 1:rmax
    if alive_ima_leach(r) >= n
        a1 = r;
    end
    if alive_leach(r) >= n
        a2 = r;
    end
    if alive_ima_leach(r) >= (1-0.1)*n
        b1 = r;
    end
    if alive_leach(r) >= (1-0.1)*n
        b2 = r;
    end
    if alive_ima_leach(r) >= (1-0.2)*n
        c1 = r;
    end
    if alive_leach(r) >= (1-0.2)*n
        c2 = r;
    end
     if alive_ima_leach(r) >= (1-0.5)*n
        d1 = r;
    end
    if alive_leach(r) >= (1-0.5)*n
        d2 = r;
    end
     if alive_ima_leach(r) > 0
        e1 = r;
    end
    if alive_leach(r) > 0
        e2 = r;
    end
end
y=[a1, a2; b1, b2; c1, c2; d1, d2; e1, e2];
b=bar(y);
grid on;
set(gca,'XTickLabel',{'0', '10', '20', '50', '100'})
legend('IMP\_LEACH', 'LEACH');
xlabel('死亡比例');
ylabel('循环轮数');
```

四、参考文献
[1] 潘华. 无线传感器网络中LEACH协议的优化研究[D].上海交通大学,2018.
[2] Heinzelman W R , Chandrakasan A P , Balakrishnan H . Energy-efficient communication protocol for wireless sensor networks[C]// Hawaii International Conference on System Sciences. IEEE, 2000.
[3] 周萌,陈跃东,陈孟元. 能耗最优的LEACH协议改进[J]. 计算机工程与应用,2014,50(23) :82- 86.
[4] 牛小娇, 吕程林. 一种基于LEACH协议的分簇路由算法[J]. 计算机技术与发展, 2011(07):13-15.
[5] Mao L , Zhang Y . An energy-efficient LEACH algorithm for wireless sensor networks[C]// 2017 36th Chinese Control Conference (CCC). 2017.
————————————————
