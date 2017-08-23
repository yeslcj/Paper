## paper code
``` matlab
%% 参考陆传X《排队论》第二版[M].北京邮电大学出版社，2009，p72-76
%%=================初始化并赋初值================
clc;clear all
numda=0.25;    %numda表示排队模型M/M/n/m中，产品到达速率
mu=0.2;        %mu表示工站的加工速率
n=4;          %表示某个工站中并联加工设备的数量
m=5;          %m表示缓冲区大小
ro=numda/(n*mu);   %ro表示工站负荷水平或强度(ro=ro1/n)。
%============计算ro===========
if ro~=1
    sum = 0;
    for k = 0:n-1 % 循环变量根据需要改吧
      sum = sum+((n*ro)^k)/(factorial(k));
    end
    P0=(sum+((n*ro)^n)*(1-(ro^(m-n+1)))/(factorial(n)*(1-ro)))^(-1);  %计算p0表示饥饿概率
    Pm=((n^n)*(ro^m)*P0)/factorial(n);   %pm表示缓冲区为m时的阻塞概率
    wq=((((n^n)*(ro^(n+1))*P0)/( factorial(n)*((1-ro)^2)*numda*(1-Pm)))*(1-(m-n+1)*(ro^(m-n))+(m-n)*(ro^(m-n+1)))); %平均排队等待时间
    p_loss=Pm;  %（1）系统损失率，与上面的pm相同。
    Q= 1-Pm;     %（2）系统相对通过能力
    numda1=numda*Pm;    %（3）单位时间平均损失的产品数量
    numdae=numda*(1-Pm);    % （3）平均进入系统的产品数量
    n_busy=numdae/mu;     %（4）平均忙着的设备数量
    Lq=((n^n)*(ro^(n+1))*P0*(1-(m-n+1)*(ro^(m-n))+(m-n)*(ro^(m-n+1))))/(factorial(n)*((1-ro)^2));   %（5）平均排队等待数量（排队等待队长）
    Ls=Lq+(numdae/mu);   %（6）平均队长（即平均产品数量）
    Wq=Lq/numdae;    %（7）平均排队等待时间
    Ws=Wq+(1/mu);     %（7）产品在系统内平均逗留时间
%===========================显示结果================================
    disp(['P0(饥饿概率/系统空闲率）=' num2str(P0)]);
    disp(['Q(系统相对通过能力）=' num2str(Q)]);
    disp(['numda1(单位时间平均损失的产品数量）=' num2str(numda1)]);
    disp(['numdae(平均进入系统的产品数量）=' num2str(numdae)]);
    disp(['Pm(缓冲区为m时的阻塞概率）=' num2str(Pm)]);
    disp(['n_busy(平均忙着的设备数量）=' num2str(n_busy)]);
    disp(['Lq(平均排队等待数量）=' num2str(Lq)]);
    disp(['Ls(平均队长,即平均产品数量）=' num2str(Ls)]);
    disp(['Ws(平均逗留时间）=' num2str(Ws)]);
    disp(['Wq(平均排队等待时间）=' num2str(Wq)]);
else
    sum = 0;
    for k = 0:n-1 % 循环变量根据需要改吧
      sum = sum + (n^k)/(factorial(k));
    end
    P0=(sum+(n^n)*(m-n+1))/(factorial(n))^(-1)      %p0表示排队模型M/M/n/m中，饥饿率
end
```
