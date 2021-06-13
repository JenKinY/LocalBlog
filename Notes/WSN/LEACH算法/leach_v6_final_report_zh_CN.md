```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%                                                                      %
%                   对于任何相关问题或其他信息 LEACH 协议                %
% For any related questions or additional informat  LEACH Protocol     %
%                          论文部分：                                   %
%       Energy-Efficient Protocols In Wireless Sensor Networks         %
%       无线传感器网络中的节能协议                                       %
%                                                                      %
% (c) Alexandros Nikolaos Zattas                                       %
% University of Peloponnese                                            %
% Department of Informatics and Telecommunicationsion                  %
% send an e-mail to:                                                   %
% alexzattas@gmail.com                                                 %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

% leachv2.m is a program for the test for the number of cluster heads for
% leachv2.m 是用于测试簇头数量的程序。
% various topology configuration  各种拓扑配置  /程式主要是修改1/p，可以取整數。
% 多划一个图，可以画 packets to sink 的总封包。
% 可以取出節點位置。


close all;
clear;
clc;

%%%%%%%%%%%%%%%%%%%% Network Establishment Parameters %%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%% 网络建立参数 %%%%%%%%%%%%%%%%%%%%

%%% Area of Operation %%%
%%% 操作范围 %%%

% Field Dimensions in meters %
% 场地尺寸，以米为单位 %
xm=100;
ym=100;
x=0; % added for better display results of the plot 添加以更好地显示绘图结果
y=0; % added for better display results of the plot 添加以更好地显示绘图结果

% Number of Nodes in the field %
% 场地中的节点数 %
n=100;
% Number of Dead Nodes in the beggining %
% 开始时的死亡节点数 %
dead_nodes=0;
% Coordinates of the Sink (location is predetermined in this simulation) %
% Sink 的坐标（位置在此模拟中已预先确定） %
sinkx=50;
sinky=50;

%%% Energy Values %%%
%%% 能量值 %%%
% Initial Energy of a Node (in Joules) % 
% 节点的初始能量（以焦耳为单位） % 
Eo=0.5; % units in Joules 单位是：焦耳
% Energy required to run circuity (both for transmitter and receiver) %
% 运行电路所需的能量（包括发射器和接收器） %
Eelec=50*10^(-9); % units in Joules/bit 单位为：焦耳/bit
ETx=50*10^(-9); % units in Joules/bit
ERx=50*10^(-9); % units in Joules/bit
% Transmit Amplifier Types %
% 发射放大器类型 %
Eamp=100*10^(-12); % units in Joules/bit/m^2 (amount of energy spent by the amplifier to transmit the bits) 单位为焦耳/bit/m^2（放大器传输位所花费的能量）
% Data Aggregation Energy %
% 数据聚合能量 %
EDA=5*10^(-9); % units in Joules/bit 单位为：焦耳/bit
% Size of data package %
% 数据包大小 %
k=4000; % units in bits 单位为：bits
% Suggested percentage of cluster head % 建议的簇头百分比
p=0.1; % a 5 percent of the total amount of nodes used in the network is proposed to give good results 为了达到良好的效果，建议在网络中使用的节点总数的5％
% Number of Clusters % 簇头数量
No=p*n; 
% Round of Operation % 操作论数
rnd=0;
% Current Number of operating Nodes % 当前操作节点数
operating_nodes=n;
transmissions=0;
temp_val=0;
flag1stdead=0;
%%%%%%%%%%%%%%%%%%%%%%%%%%% End of Parameters %%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%% 参数结束 %%%%%%%%%%%%%%%%%%%%%%%%%%%%



            %%% 创建无线传感器网络 %%%
            %%% Creation of the Wireless Sensor Network %%%
            

% Plotting the WSN %
% 绘制 WSN 无线传感器网络 % 


% pos1 is for 100*100 
% pos1 100 * 100 
pos1=[82,79,38,22,16,11,7,30,37,99,41,19,11,77,16,45,74,51,72,45,26,49,62,45,21,72,55,39,89,27,81,14,59,68,51,32,2,38,20,44,62,98,82,76,11,69,7,14,26,43,90,8,13,54,16,92,23,59,85,98,52,80,2,30,62,61,46,20,1,62,64,64,60,64,81,21,9,45,75,49,35,97,10,15,36,2,49,12,53,7,84,84,33,55,43,96,14,56,57,48;
    15,57,95,40,88,47,58,4,46,36,51,40,99,12,34,23,94,56,28,87,80,67,8,25,1,50,39,64,45,24,40,82,54,14,33,40,52,18,92,71,88,4,90,22,72,72,32,69,43,77,20,77,46,64,70,44,5,58,79,93,6,42,21,77,99,97,26,63,84,79,54,44,73,47,26,77,76,94,78,70,88,32,81,46,85,14,73,17,37,93,65,2,85,91,0,25,40,93,71,6];

% pos2 is for 5*100
% pos2 5*100
% pos2=[1	 2	3	4	5	6	7	8	9	10	11	12	13	14	15	16	17	18	19	20	21	22	23	24	25	26	27	28	29	30	31	32	33	34	35	36	37	38	39	40	41	42	43	44	45	46	47	48	49	50	51	52	53	54	55	56	57	58	59	60	61	62	63	64	65	66	67	68	69	70	71	72	73	74	75	76	77	78	79	80	81	82	83	84	85	86	87	88	89	90	91	92	93	94	95	96	97	98	99	100
% 1	2	3	4	5	6	7	8	9	10	11	12	13	14	15	16	17	18	19	20	21	22	23	24	25	26	27	28	29	30	31	32	33	34	35	36	37	38	39	40	41	42	43	44	45	46	47	48	49	50	51	52	53	54	55	56	57	58	59	60	61	62	63	64	65	66	67	68	69	70	71	72	73	74	75	76	77	78	79	80	81	82	83	84	85	86	87	88	89	90	91	92	93	94	95	96	97	98	99	100
% ];
    
for i=1:n
    SN(i).x = pos1(1, i);
    SN(i).y = pos1(2, i);
end

% Initial energy assignment
% 初始能量分配
for i=1:10
    SN(i).E=Eo;
end
for i=11:n
    SN(i).E=Eo;
end

for i=1:n
    
    SN(i).id=i;	% sensor's ID number 传感器的ID号
    %SN(i).x=rand(1,1)*xm;	% X-axis coordinates of sensor node 传感器节点的X轴坐标
    %SN(i).y=rand(1,1)*ym;	% Y-axis coordinates of sensor node 传感器节点的Y轴坐标
    % SN(i).E=Eo;     % nodes energy levels (initially set to be equal to % 节点能级（最初设置为等于“ Eo”)
    
    % 如果值为“ 0”，则该节点正常工作；如果被选为簇头，则它将获得“ 1”的值（最初所有节点都是正常的）
    SN(i).role=0;   % node acts as normal if the value is '0', if elected as a cluster head it  gets the value '1' (initially all nodes are normal)
    % 节点所属簇的百分比
    SN(i).cluster=0;	% the cluster which a node belongs to
    % 指出节点的当前条件。 当节点处于运行状态时，其值为= 1，而当死亡时为0
    SN(i).cond=1;	% States the current condition of the node. when the node is operational its value is =1 and when dead =0
    % 轮数节点处于运行状态
    SN(i).rop=0;	% number of rounds node was operational
    % 节点向左舍入，可用于簇头选举
    SN(i).rleft=0;  % rounds left for node to become available for Cluster Head election
    % 节点距他所属的群集的群集头的距离
    SN(i).dtch=0;	% nodes distance from the cluster head of the cluster in which he belongs
    % 节点到接收器的距离
    SN(i).dts=0;    % nodes distance from the sink
    % 该节点被选为簇头的次数
    SN(i).tel=0;	% states how many times the node was elected as a Cluster Head
    % 轮数节点被选为簇头
    SN(i).rn=0;     % round node got elected as cluster head
    % 'i'普通节点所属的簇头的节点ID
    SN(i).chid=0;   % node ID of the cluster head which the "i" normal node belongs to
    % 尝试计算每个节点的能耗，直到第一个死节点为止
    SN(i).power=0;  % try to calculate the energy consumption for each node until the first dead node
    

    
    % 画图1 无线传感器网络
    hold on;
    figure(1)
    plot(x,y,xm,ym,SN(i).x,SN(i).y,'ob',sinkx,sinky,'*r');
    title 'Wireless Sensor Network';
    xlabel '(m)';
    ylabel '(m)';
    
end
 

                      %%%%%% %%%%%%设置阶段%%%%%% %%%%%%
                      %%%%%% Set-Up Phase %%%%%% 
while operating_nodes>0
        
    % Displays Current Round %     
    % 显示当前轮数 %     
    rnd;     

	% Threshold Value %
    % 阈值 %
	t=(p/ceil(1-p*(mod(rnd,ceil(1/p)))));
    %t=(p/(1-p*(mod(rnd,1/p))));
    
    
    % Re-election Value %
    % 重选值 %
    tleft=mod(rnd,ceil(1/p));
 
	% Reseting Previous Amount Of Cluster Heads In the Network %
    % 重置网络中以前的簇头数量 %
	CLheads=0;
    
    % Reseting Previous Amount Of Energy Consumed In the Network on the Previous Round %
    % 重置上一轮网络中以前消耗的能源量 %
    energy=0;
 
        
        
% Cluster Heads Election %
% 簇头选举 %
     while CLheads ==0          % added by rhc
        for i=1:n
            SN(i).cluster=0;    % reseting cluster in which the node belongs to 重置节点所属的簇
            SN(i).role=0;       % reseting node role 重置节点角色
            SN(i).chid=0;       % reseting cluster head id 重置簇头ID
            if SN(i).rleft>0 
               SN(i).rleft=SN(i).rleft-1;
            end
            
            if (SN(i).E>0) && (SN(i).rleft==0)	
                    %if 
                    %t1=t*SN(i).E/Eo;
                   generate=rand;
%                     if SN(i).E>0.4
%                         generate=rand*t;
%                     else
%                         generate=rand;
%                     end
                    if generate< t
                    SN(i).role=1;	% assigns the node role of a cluster head  分配簇头的节点角色
                    SN(i).rn=rnd;	% Assigns the round that the cluster head was elected to the data table 将选择簇头的回合分配给数据表
                    SN(i).tel=SN(i).tel + 1;   % cluster head times 簇头时间
                    SN(i).rleft=ceil(1/p)-tleft;    % rounds for which the node will be unable to become a   CH 节点将无法成为CH的轮数
                    % 计算 Sink 和簇头之间的距离
                    SN(i).dts=sqrt((sinkx-SN(i).x)^2 + (sinky-SN(i).y)^2); % calculates the distance between the sink and the cluster head
                    CLheads=CLheads+1;	% sum of cluster heads that have been elected  选出的簇头的总和
                    SN(i).cluster=CLheads; % cluster of which the node got elected to be cluster head 节点被选为簇头的簇
                    CL(CLheads).x=SN(i).x; % X-axis coordinates of elected cluster head  选出的簇头的X轴坐标
                    CL(CLheads).y=SN(i).y; % Y-axis coordinates of elected cluster head  选出的簇头的Y轴坐标
                    CL(CLheads).id=i; % Assigns the node ID of the newly elected cluster head to an array 将新选举的簇头的节点ID分配给数组
                    end
        
            end
        end
     end   
	% Fixing the size of "CL" array %
    % 固定“ CL”数组的大小 %
	CL=CL(1:CLheads);
  
% Grouping the Nodes into Clusters & caclulating the distance between node and cluster head %
% 将节点分组到群集中，并计算节点与群集头之间的距离 %
     
       for i=1:n
        if  (SN(i).role==0) && (SN(i).E>0) && (CLheads>0) % if node is normal
            for m=1:CLheads
            d(m)=sqrt((CL(m).x-SN(i).x)^2 + (CL(m).y-SN(i).y)^2);
            % we calculate the distance 'd' between the sensor node that is
            % transmitting and the cluster head that is receiving with the following equation+ 
            % d=sqrt((x2-x1)^2 + (y2-y1)^2) where x2 and y2 the coordinates of
            % the cluster head and x1 and y1 the coordinates of the transmitting node
            % 我们使用以下等式+ d=sqrt((x2-x1)^2 + (y2-y1)^2) 
            % 计算正在发送的传感器节点与正在接收的簇头之间的距离'd'
            % 其中x2和y2为 簇头的坐标，x1和y1的传输节点的坐标
            end
        d=d(1:CLheads); % fixing the size of "d" array 固定“ d”数组的大小
        [M,I]=min(d(:)); % finds the minimum distance of node to CH 找到节点到簇头的最小距离
        [Row, Col] = ind2sub(size(d),I); % displays the Cluster Number in which this node belongs too 同时显示该节点所属的簇号
        SN(i).cluster=Col; % assigns node to the cluster 将节点分配给簇
        SN(i).dtch= d(Col); % assigns the distance of node to CH  将节点的距离分配给CH
        SN(i).chid=CL(Col).id;
        end
       end
       
        
       
                           %%%%%% Steady-State Phase %%%%%%
                           %%%%%% 稳态阶段 %%%%%%
                      
  
% Energy Dissipation for normal nodes %
% 普通节点的能量耗散 %
    
    for i=1:n
       if (SN(i).cond==1) && (SN(i).role==0) && (CLheads>0)
       	if SN(i).E>0
            ETx= Eelec*k + Eamp * k * SN(i).dtch^2;
            SN(i).E=SN(i).E - ETx;
            energy=energy+ETx;
            SN(i).power=SN(i).power+ETx;  % for normal node tx energy consumption 普通节点的TX能耗
            
        % Dissipation for cluster head during reception
        % 接收期间簇头耗散
        if SN(SN(i).chid).E>0 && SN(SN(i).chid).cond==1 && SN(SN(i).chid).role==1
            ERx=(Eelec+EDA)*k;
            energy=energy+ERx;
            SN(i).power=SN(i).power+ERx;  % for CH node rx energy consumption CH节点的rx能耗
            SN(SN(i).chid).E=SN(SN(i).chid).E - ERx;
             if SN(SN(i).chid).E<=0  % if cluster heads energy depletes with reception  如果簇头能量随着接收而耗尽
                SN(SN(i).chid).cond=0;
                SN(SN(i).chid).rop=rnd;
                dead_nodes=dead_nodes +1;
                operating_nodes=operating_nodes - 1;
             end
        end
        end
        
        %  如果节点能量随着传输而耗尽
        if SN(i).E<=0       % if nodes energy depletes with transmission
        dead_nodes=dead_nodes +1;
        operating_nodes=operating_nodes - 1;
        SN(i).cond=0;
        SN(i).chid=0;
        SN(i).rop=rnd;
        end
        
      end
    end            
    
    
    
% Energy Dissipation for cluster head nodes %
% 簇头节点的能耗 %
   
   for i=1:n
     if (SN(i).cond==1)  && (SN(i).role==1)
         if SN(i).E>0
            % ETx= (Eelec+EDA)*k + Eamp * k * SN(i).dts^2;
            ETx= Eelec*k + Eamp * k * SN(i).dts^2;
            SN(i).E=SN(i).E - ETx; % residue battery power 剩余电池电量
            SN(i).power=SN(i).power+ETx; % for CH node tx energy consumption CH节点发射能量消耗
            energy=energy+ETx;  % energy consumption for each round          每轮能耗
         end
         % 如果簇头能量随着传输而耗尽
         if  SN(i).E<=0     % if cluster heads energy depletes with transmission
         dead_nodes=dead_nodes +1;
         operating_nodes= operating_nodes - 1;
         SN(i).cond=0;
         SN(i).rop=rnd;
         end
     end
   end

   

  
    if operating_nodes<n && temp_val==0
        temp_val=1;
        flag1stdead=rnd;
    end
    % Display Number of Cluster Heads of this round %
    %CLheads;
    % 显示此回合的簇头数
    % CLheads;
   
    %记录传输至sink的packet数，不太对？
    transmissions=transmissions+1;
    %前面强制一定要有cluster head，所以下面代码不需要
%     if CLheads==0
%     transmissions=transmissions-1;
%     end
    
 
    % Next Round %
    % 下一轮 %
    rnd= rnd +1;
    
    tr(transmissions)=operating_nodes;
    op(rnd)=operating_nodes;
    
    %transmissions可以和rnd合并
    %if energy>0
    nrg(transmissions)=energy;
    %end
    

end


sum=0;
for i=1:flag1stdead
    sum=nrg(i) + sum;
end

temp1=sum/flag1stdead;
temp2=temp1/n;
% ???
for i=1:flag1stdead
avg_node(i)=temp2;
end

sum1=zeros(1, transmissions);
for i=1:transmissions
    for j=1:i
    sum1(1, i)=sum1(1, i) + tr(1, j);
    end
end
sum1;

ch_number=zeros(1, n);
for i=1:n
    ch_number(1,i)=SN(i).tel;
end

round_number=zeros(1, n);
for i=1:n
    round_number(1,i)=SN(i).rop;
end

power_number=zeros(1, n);
for i=1:n
    power_number(1,i)=SN(i).power;
end

residue_power=zeros(1, n);
for i=1:n
    residue_power(1,i)=(Eo-SN(i).power)/Eo*100;
    if residue_power(1,i)<0,
        residue_power(1,i)=0;
    end
end

power_dist=zeros(1, n);
for i=1:n
    power_dist(1, i)=power_number(1, i)/round_number(1,i);
end


%%%%%%%%%%%%%%%%%%%% 绘图阶段 %%%%%%%%%%%%%%%%%%%%


    % Plotting Simulation Results "Operating Nodes per Round" %
    % 绘制仿真结果“每轮操作节点数” "Operating Nodes per Round" %
%     figure(2)
%     plot(1:rnd,op(1:rnd),'-r','Linewidth',2);
%     title ({'LEACH'; 'Operating Nodes per Round';})
%     xlabel 'Rounds';
%     ylabel 'Operational Nodes';
%     hold on;
    
    % Plotting Simulation Results "Operational Nodes per Transmission" %
    % 绘制仿真结果“每次传输的操作节点” "Operational Nodes per Transmission" %
%     figure(3)
%     plot(1:transmissions,tr(1:transmissions),'-r','Linewidth',2);
%     title ({'LEACH'; 'Operational Nodes per Transmission';})
%     xlabel 'Transmissions';
%     ylabel 'Operational Nodes';
%     hold on;
    
    % Plotting Simulation Results "Energy consumed per Transmission" %
    % 绘制模拟结果“每次传输所消耗的能量” %
    figure(4)
    plot(1:flag1stdead,nrg(1:flag1stdead),'-r','Linewidth',2);
    title ({'LEACH'; '每次传输所消耗的能量';})
    xlabel '传输';
    ylabel '能量 ( J )';
    hold on;
    

    % Plotting Simulation Results "Average Energy consumed by a Node per Transmission" %
    % 绘制模拟结果“每个传输节点平均消耗的能量” %
%     figure(5)
%     plot(1:flag1stdead,avg_node(1:flag1stdead),'-r','Linewidth',2);
%     title ({'LEACH'; 'Average Energy consumed by a Node per Transmission';})
%     xlabel 'Transmissions';
%     ylabel 'Energy ( J )';
%     hold on;
  
    % Plotting Simulation Results "Packet transmission to sink" %
    % 绘制仿真结果“数据包传输到接收器” %
    figure(5)
    plot(1:transmissions,sum1(1:transmissions),'-r','Linewidth',2);
    title ({'LEACH'; '数据包传输到接收器';})
    xlabel 'Transmissions';
    ylabel 'Packet transmission to sink';
    hold on; 
    
    % Plotting Simulation Results "Number of cluster head for each node" %
    % 绘制模拟结果“每个节点的簇头数” %
    figure(6)
    plot(1:n,ch_number(1:n),'-r','Linewidth',2);
    title ({'LEACH'; '每个节点的簇头数';})
    xlabel 'Node ID';
    ylabel 'Number of cluster head';
    hold on;   
    
    % Plotting Simulation Results "Number of operation round for each node" %
    % 绘制仿真结果“每个节点的操作回合数” %
    figure(7)
    plot(1:n,round_number(1:n),'-r','Linewidth',2);
    title ({'LEACH'; '每个节点的操作回合数';})
    xlabel 'Node ID';
    ylabel 'Number of operation round';
    hold on; 
    
    % Plotting Simulation Results "Energy consumption distribution of each node" %
    % 绘制模拟结果“每个节点的能耗分布” %
    figure(8)
    plot(1:n,power_number(1:n),'-r','Linewidth',2);
    title ({'LEACH'; '每个节点的能耗分布';})
    xlabel 'Node ID';
    ylabel 'Power consumption of each node';
    hold on;  
    
    % Plotting Simulation Results "Percentage on residue battery energy of each node" %
    % 绘制模拟结果“每个节点的剩余电池电量百分比” %
    figure(9)
    plot(1:n,residue_power(1:n),'-r','Linewidth',2);
    title ({'LEACH'; '每个节点的剩余电池电量百分比';})
    xlabel 'Node ID';
    ylabel 'Percentage on residue energy of each node (%)';
    hold on; 
    
    % Plotting Simulation Results "Average power consumption distribution of each node in each round" %
    % 绘制仿真结果“每一轮中每个节点的平均功耗分布 %
    figure(10)
    plot(1:n,power_dist(1:n),'-r','Linewidth',2);
    title ({'LEACH'; '每一轮中每个节点的平均功耗分布';})
    xlabel 'Node ID';
    ylabel 'Energy consumption of each node in a round';
    hold on; 
    
   
  
```