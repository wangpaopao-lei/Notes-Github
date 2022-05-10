#### 2(a).

Since the weight of the pig after t days is $w = 800/(1 + 3e^{-t/30})$ lbs.
$$
\frac{dw}{dt}=(80e^{-t/30})/(3e^{-t/30} + 1)^2>0
$$
Hence, the weight of the pig will keep gaining ae $t$ increases.

#### 2(b).

#### Step 1. Ask the question

**Variables:**
$$
\begin{aligned}
t &= time\; (days)\\
w &= weight\;of \;pig\; (lbs)\\
p &= price \;for\; pigs \;(\$/lb)\\
C &= cost \;of\; keeping\; pig\; t \;days\; (\$)\\
R &= revenue \;obtained\; by\; selling\; pig \;(\$)\\
P &= profit\; from\; sale \;of\; pig \;(\$)\\
\end{aligned}
$$
**Assumptions:**
$$
\begin{aligned}
w &= 800/(1 + 3e^{-t/30}) \\
p &= 0.65 - 0.01t \\
C &= 0.45t\\
R &= p · w\\ 
P &= R - C\\ 
t &≥ 0\\
\end{aligned}
$$
**Objectives:**
$$Maximize\quad p$$
#### Step 2. Select the modeling approach
We will model the problem as a one-variable optimization problem. The general solution procedure for one-variable optimization problems is relatively complex for this question. So, here we will use some computational methods that can be used to implement this general solution procedure.
#### Step 3. Formulate the model
The objective function for this problem is given by
$$
y = f(x) = (0.65-0.01x)(800/(1 + 3e^{-x/30}))-0.45x
$$
and our problem is to maximize the above function over the set $S = \left\{x : x ≥ 0\right\}$.

#### Step 4. Solve the model
We will use the graphical method. We provide the code of MATLAB of the graphical method as follows.
```matlab{.line-numbers} 
%%
clear all;
clc;close all;
h= fplot('(0.65-0.01*x)*(800/(1 + 3*exp(-x/30)))-0.45*x',[11,14,135,135.5]);
xlabel('x') 
ylabel('f(x)') 
grid on;
%%
clear all;
clc;close all;
x=12:0.001:13;
y=(0.65-0.01.*x).*(800./(1 + 3.*exp(-x.*(1/30))))-0.45.*x;
plot(x,y);
xlabel('x') 
ylabel('f(x)') 
grid on;
```

We start our graphical analysis of this problem by producing a graph of the function for the range of value for x from 0 to 20. In this case we are left with the feeling that there is more to see on the graph of this function. We would say that Fig. 1 is not a complete graph of this function over the set $S = [0,∞)$. 
Fig. 1 is a complete graph. It shows all of the important features we need for the solution of our problem. From the graph in Fig. 1, we can conclude that the maximum occurs around $x = 12$. 
To obtain a better estimate, we can zoom in on the maximum point of the graph. Fig. 2, Fig. 3 and Fig. 4 show the outcome of successive zooms.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-15-26-14.png" style="zoom:50%;" />

Figure 1: Graph of net profit f(x) versus time to sell x for the pig problem with nonlinear weight model.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-15-27-58.png" alt="2022-04-07-15-27-58" style="zoom:50%;" />

Figure 2: First zoom-in on the graph of net profit f(x) versus time to sell x for the pig problem with nonlinear weight model.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-15-29-16.png" alt="2022-04-07-15-29-16" style="zoom:50%;" />

Figure 3: Second zoom-in on the graph of net profit f(x) versus time to sell x for the pig problem with nonlinear weight model.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-15-51-01.png" alt="2022-04-07-15-51-01" style="zoom:50%;" />

Figure 4: Third zoom-in on the graph of net profit f(x) versus time to sell x for the pig problem with nonlinear weight model.


On the basis of the graph in Fig. 3 and Fig. 4, we would estimate that the maximum occurs at
$$
x = 12.33, \quad y = f(x) =  135.4236.
$$
Since $f'(x) = 0$ at the maximum, the function $f(x)$ is quite insensitive to changes in $x$ near this point, so we are able to obtain more accuracy for $f(x)$ than for $x$.

#### Step 5. Answer the question
After taking into account the fact that the growth rate of the young pig is still increasing, we now recommend waiting 12 days to sell. This should result in a net profit of approximately \$135.4.

#### 2.(c)
Now we examine the sensitivity of the optimum coordinates of $f(x)$ to the maximum weight for the young pig. We assume that
$$w = c/(1 + 3e^{-t/30})$$
This leads to the objective function
$$
f(x) = (0.65-0.01x)(c/(1 + 3e^{-x/30}))-0.45x
$$
Hence
$$
f'(x)=-c/(100(3e^{-x/30} + 1)) - (ce^{-x/30}(x/100 - 13/20))/(10(3e^{-x/30} + 1)^2) - 9/20
$$
Then
$$
f''(x)=(ce^{-x/30}(x/100 - 13/20))/(300(3e^{-x/30} + 1)^2) - (ce^{-x/15}(x/100 - 13/20))/(50(3e^{-x/30} + 1)^3) - (ce^{-x/30})/(500(3e^{-x/30} + 1)^2)
$$
Use the Newton method to find the root of the objective function
$$
F(x)=f'(x)=-c/(100(3e^{-x/30} + 1)) - (ce^{-x/30}(x/100 - 13/20))/(10(3e^{-x/30} + 1)^2) - 9/20=0
$$
We provide the code of MATLAB of Newton’s method as follows.

```matlab{.line-numbers}
function x=NewtonIteration(x0, n)   %初始值x0，n迭代次数
%format long
for c=768:8:824
F=@(x)- c/(100*(3*exp(-x/30) + 1)) - (c*exp(-x/30)*(x/100 - 13/20))/(10*(3*exp(-x/30) + 1)^2) - 9/20;
f=@(x)(c*exp(-x/30)*(x/100 - 13/20))/(300*(3*exp(-x/30) + 1)^2) - (c*exp(-x/15)*(x/100 - 13/20))/(50*(3*exp(-x/30) + 1)^3) - (c*exp(-x/30))/(500*(3*exp(-x/30) + 1)^2);
x(1)=x0;
for i=1:n
    x(i+1)=x(i)-F(x(i))./f(x(i));
end
x=x(i+1)
end
end
```
For each value of c, we performed $N = 100$ iterations starting at the point $x_0 = 12$. 
* Let $x=NewtonIteration(12, 100)$ , we have table 2 below.
<div align=center>
<table align="center" style="width:20%;border:#000  solid; border-width:1px 0">
<caption>Table 1  </caption>
<thead style="border-bottom:#000 1px solid;">
<tr>
<th style="border:0">c</th>
<td style="border:0">x</td>
</tr>
</thead>
<tr>
<th style="border:0">768</th>
<td style="border:0">12.1115</td>
</tr>
<th style="border:0">776</th>
<td style="border:0">12.1692</td>
</tr>
<th style="border:0">784</th>
<td style="border:0">12.2257</td>
</tr>
<th style="border:0">792</th>
<td style="border:0">12.2809</td>
</tr>
<th style="border:0">800</th>
<td style="border:0">12.3349</td>
</tr>
<th style="border:0">808</th>
<td style="border:0">12.3877</td>
</tr>
<th style="border:0">816</th>
<td style="border:0"> 12.4394</td>
</tr>
<th style="border:0">824</th>
<td style="border:0">12.4900</td>
</tr>
</table>
</div>

Setting $c=808$ (increasing 1%), we have $x=12.3877,\;f(x)=136.8334$
Hence , we have
$$
\begin{aligned}
S(x,c)&=\frac{dx}{dc}\frac{c}{x}\\
&\approx 0.043\\
\end{aligned}
$$
$$
\begin{aligned}
S(f(x),c)
&=\frac{df(x)}{dc}\frac{c}{f(x)}\\
&\approx 1.04\\
\end{aligned}
$$
Therefore, if $c$ goes up by 1%, then $x$ goes up by about 0.043% and $f(x)$ goes up by about 1.04%.

#### 6(a).
#### Step 1. Ask the question

**Variables:**
$$
\begin{aligned}
(x_0,y_0)&= locations\;of\;the\;calls\;on\;the\;city\;map\;by\;coordinates \\
(x,y)&= the\;location\;of\;the\;new\;station\;on\;the\;city\;map\;by\;coordinates \\
t &= the\;response\;time\;to\;a\;call \;(min)\\
T &= the\;average\;response\;time\;to\;a\;call \;(min)\\
r&=the\;length\;between\;a\;call\;and\;the\;station\;(miles)
\end{aligned}
$$
**Assumptions:**
$$
\begin{aligned}
t &= kr \\
r &= |(x-x_0,y-y_0)| \\
T &= \sum t/84\\
0 &≤ x, x_0 ≤ 6\\ 
0 &≤ y, y_0 ≤ 6\\
\end{aligned}
$$
**Objectives:**
$$Minimize\quad T$$
#### Step 2. Select the modeling approach
There are two variables in the objective function T, Hence we will use the Newton method of two variables to optimize this problem and find the most suitable location for the station to obtain the minimum average time to response calls.

#### Step 3. Formulate the model 
From the example 2.2, we have the graph below
								<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-17-48-05.png" alt="2022-04-07-17-48-05" style="zoom:50%;" />
So we have,
$$
\begin{aligned}
T=f(x, y)=&\left(6 \sqrt{(x-1)^{2}+(y-5)^{2}}+8 \sqrt{(x-3)^{2}+(y-5)^{2}}+8 \sqrt{(x-5)^{2}+(y-5)^{2}}\right.\\
&+21 \sqrt{(x-1)^{2}+(y-3)^{2}}+6 \sqrt{(x-3)^{2}+(y-3)^{2}}+3 \sqrt{(x-5)^{2}+(y-3)^{2}} \\
&\left.+18 \sqrt{(x-1)^{2}+(y-1)^{2}}+8 \sqrt{(x-3)^{2}+(y-1)^{2}}+6 \sqrt{(x-5)^{2}+(y-1)^{2}}\right)/84
\end{aligned}
$$
The problem is to minimize $T=f(x,y)$ in the feasible region $S=\left\{(x,y)
:0≤x≤6,0≤y≤6\right\}$.
#### Step 4. Solve the model
We will perform the random search method to find the optimize. 
First, we plot  3-D graph of average response time $T= f(x, y)$ versus map location $(x, y)$ for the facility location problem. 

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-19-16-34.png" alt="2022-04-07-19-16-34" style="zoom:50%;" />

Then rotate this figure to obtain the approximate $x$ and $y$ by the darkest color below.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-19-20-42.png" alt="2022-04-07-19-20-42" style="zoom:50%;" />

Zoom the figure, we obtain the minimum of $f(x,y)$ is approximately locate at $(2,3)$.

<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2022-04-07-19-23-31.png" alt="2022-04-07-19-23-31" style="zoom:50%;" />

We provide the code of MATLAB of random search method as follows.
```matlab{.line-numbers} 
%random search method
clear;clc;
format long
syms x y z
xmin=2;xmax=4;
ymin=1;ymax=3;
N=500;
zmin=(6*sqrt((x-1)^2+(y-5)^2)+8*sqrt((x-3)^2+(y-5)^2)+8*sqrt((x-5)^2+(y-5)^2)+21*sqrt((x-1)^2+(y-3)^2)+6*sqrt((x-3)^2+(y-3)^2)+3*sqrt((x-5)^2+(y-3)^2)+18*sqrt((x-1)^2+(y-1)^2)+8*sqrt((x-3)^2+(y-1)^2)+6*sqrt((x-5)^2+(y-1)^2))/84;
for n=1:N
    x=rand(1,1)*2+2;
    y=rand(1,1)*3+1;
    z=(6.*sqrt((x-1).^2+(y-5).^2)+8 .*sqrt((x-3).^2+(y-5).^2)+8.*sqrt((x-5).^2+(y-5).^2)+21.*sqrt((x-1).^2+(y-3).^2)+6.*sqrt((x-3).^2+(y-3).^2)+3.*sqrt((x-5).^2+(y-3).^2)+18.*sqrt((x-1).^2+(y-1).^2)+8.*sqrt((x-3).^2+(y-1).^2)+6.*sqrt((x-5).^2+(y-1).^2))./84;
    if z<zmin
        xmin=x;
        ymin=y;
        zmin=z;
    end
end
```
We have the answer $(x,y)=(1.7956,2.7156)$.

#### Answer the question

Therefore, the most suitable location that minimizes average response time is $(2,3)$.



