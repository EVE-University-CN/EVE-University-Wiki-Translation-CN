*翻译自[https://wiki.eveuniversity.org/Warp](https://wiki.eveuniversity.org/Warp)*

# 跃迁
跃迁是主要的应用于新伊甸的舰船的超光速航行方法。跃迁航行仅限于在两个在同一恒星系的地点间的穿梭，并且只能对至少150km远的地点启动。跃迁终点在跃迁指令被下达时就被决定。这对于你跃迁到移动物体时很重要，例如舰队成员。你也不会在精确的跃迁目的地点上脱出跃迁，而是在以该地点为球心的3km的球体上。这意味着你有时候会在一个你跃迁到的空间站的停靠范围外落地。这就是保存[秒进站](术语.md#instadock)点的原因。

## 跃迁阶段
跃迁航行包含以下三个阶段：
1. 加速
2. 巡航
3. 减速
 
基于CCP发布的公式，解出一艘舰船完成跃迁（自进入跃迁开始计算）应该花费的时间是有可能的。  
计算跃迁花费的时间是通过计算这些阶段中每个阶段花费的时间相加得到。这需要首先计算加速和减速时间，接着是巡航时间。这个计算不包括进入跃迁所花费的时间（加速到75%最大速度），也不包括重新获得对舰船的控制后的减速时间。  
$$
t_{total}=t_{accel}+t_{decel}+t_{cruise}
$$
## 长途跃迁
### **加速**
#### **公式**
CCP提供了加速t秒后所走的距离和达到的速度的公式。d是距离（m），v是速度（m/s），k是飞船的曲率速度（AU/s），a是149,597,870,700m（1AU）。
$$
\begin{align}
d&=e^{kt}\\
v&=k\cdot e^{kt}\\
v_{warp}&=k\cdot a
\end{align}
$$
#### **距离**
为了计算加速中航行的距离：
$$
\begin{align}
d&=e^{kt}\\
v&=k\cdot e^{kt}\\
&=k\cdot d\\
\therefore d&=\frac{v}{k}   
\end{align}
$$
对于$v_{warp}$加速中通过距离是：
$$
d_{accel}=\frac{v_{warp}}{k}=\frac{k\cdot a}{k}=a
$$
这意味着每一艘舰船在加速到她的最大航速时都会通过恰好1AU。

#### **时间**
为了计算加速到翘曲速度所花费的时间，$v$的方程应该重新排列为$t$，然后解决$v$等于翘曲速度的情况（单位：m/s）。
$$
\begin{align}
v&=k\cdot e^{kt}\\
\frac{v}{k}&=e^{kt}\\
kt&=\ln(\frac{v}{k})\\
t&=\frac{\ln(\frac{v}{k})}{k}
\end{align}
$$
### **减速**
减速段计算有点不一样。它使用定义为$min(\frac{k}{3},2)的$$j$而不是$k$来计算距离和速度。不同的减速率是用来防止飞船突然从 "许多许多AU的距离 "过渡到 "在网格上脱出跃迁"，其速度超过其他飞行员（或服务器/客户端）所能跟上的速度。在减速计算方面有一个复杂的问题。舰船并不是在0m/s时脱离曲率。相反，它们是在$s$m/s时脱出跃迁，之后再进行正常的亚跃迁计算。
$$
s=min(100,\frac{v_{subwarp}}{2})
$$
其中$v_{subwarp}$是舰船的最大次跃迁速度；这在很大程度上取决于船体和飞行员的技能。
#### **距离**
这使所使用的公式略有变化。请记住，航行的距离是速度的积分。
$$
\begin{align}
v&=k\cdot e^{jt}\\
d&=\begin{equation*}\int_{0}^{\infty} k\cdot e^{jt}\, dz\end{equation*}=\frac{k\cdot e^{jt}}{j}\\
&=\frac{v}{j}
\end{align}
$$
自$v_{wrap}$减速时通过距离为
$$
d_{decel}=\frac{v_{wrap}}{j}=\frac{k\cdot a}{j}
$$
#### **时间**
与加速度一样，从最大跃迁速度减速的时间是通过重新排列速度方程来计算的。
$$
\begin{align}
v&=k\cdot e^{jt}\\
\frac{v}{k}&=e^{jt}\\
t&=\frac{\ln(\frac{v}{k})}{j}
\end{align}
$$
虽然从$s$到0的减速在距离上并不显著，但在时间上却很重要。这意味着减速的时间计算如下：
$$
\begin{align}
t_{decel}&=t_{decel\_wrap}-t_{decel\_s}\\
&=\frac{\ln(\frac{v_{wrap}}{k})}{j}-\frac{\ln(\frac{s}{k})}{j}\\
&=\frac{\ln(\frac{v_{wrap}}{k})-\ln(\frac{s}{k})}{j}\\
&=\frac{\ln v_{wrap}-\ln k-\ln s+\ln k}{j}\\
&=\frac{\ln v_{wrap}-ln s}{j}\\
&=\frac{\ln(\frac{v_{wrap}}{s})}{j}
\end{align}
$$
### **巡航**
#### **距离**
巡航时的距离是总月前距离减去加速或减速时的任何距离。
$$
d_{cruise}=d_{total}-d_{accel}-d_{decel}
$$
除了最快的舰船之外，这将会是$d_{total}$-4 *AU*
#### **时间**
巡航花费的时间为：
$$
t_{cruise}=\frac{d_{cruise}}{v_{wrap}}
$$
## 短途跃迁
只要有一些时间是在最高跃迁速度下度过的，上述计算就有效；$d_{total}\geq d_{accel}+d_{decel}$.如果跃迁过短以至于舰船不能达到最高速度，就需要另一组计算。
$$
\begin{align}
d_{accel}&=\frac{v_{max}}{k},d_{decel}=\frac{v_{max}}{j}\\
d_{total}&=d_{accel}+d_{decel}=v_{max}(\frac{1}{k}+\frac{1}{j})\\
v_{max}&=\frac{d_{total}\cdot k\cdot j}{k+j}
\end{align}
$$
这样就可以使用前几节中描述的公式计算新的加速和减速时间,不过要将新的$v_{max}$代入：
$$
\begin{align}
t_{accel}&=\frac{\ln(\frac{v_{max}}{k})}{k}\\
t_{decel}&=\frac{\ln(\frac{v_{max}}{s})}{j}\\
t_{total}&=t_{accel}+t_{decel}
\end{align}
$$
## 实现
下面的python代码是上述内容的一个可能的实现。它试图生成与CCP在论坛上提出的相同的数据。除了50AU泰坦的跃迁速度是一秒之外，它与他们的数据是一致的。请注意，飞船的亚跃迁速度被固定为200m/s。这是因为CCP制作的表格假设每艘船都是以100m/s的速度脱出跃迁[2]。如果试图对实际的飞船进行计算，这个值需要用一个更合适的值来代替。输出值也通过ceil()函数，因为这似乎与CCP使用的四舍五入模式相匹配[3]。
```python
import math
AU_IN_M=149597870700

def get_distance(dist):
    if dist > 1e9:
        return (dist / AU_IN_M, "AU")
    else:
        return (dist/1000, "KM")

# 跃迁速度以AU/s为单位，亚跃迁速度以m/s为单位，距离以m为单位
def calculate_time_in_warp(max_warp_speed, max_subwarp_speed, warp_dist):

    k_accel = max_warp_speed
    k_decel = min(max_warp_speed / 3, 2)

    warp_dropout_speed = min(max_subwarp_speed / 2, 100)
    max_ms_warp_speed = max_warp_speed * AU_IN_M

    accel_dist = max_ms_warp_speed / k_accel
    decel_dist = max_ms_warp_speed / k_decel

    minimum_dist = accel_dist + decel_dist

    cruise_time = 0

    if minimum_dist > warp_dist:
        max_ms_warp_speed = warp_dist * k_accel * k_decel / (k_accel + k_decel)
    else:
        cruise_time = (warp_dist - minimum_dist) / max_ms_warp_speed

    accel_time = math.log(max_ms_warp_speed / k_accel) / k_accel
    decel_time = math.log(max_ms_warp_speed / warp_dropout_speed) / k_decel


    total_time = cruise_time + accel_time + decel_time
    distance = get_distance(warp_dist)
    return total_time


distances = [150e3, 1e9, AU_IN_M * 1, AU_IN_M * 2, AU_IN_M * 5, AU_IN_M * 10, AU_IN_M * 20, AU_IN_M * 50, AU_IN_M * 100, AU_IN_M * 200]
speeds = [1.36, 1.5, 2, 2.2, 2.5, 2.75, 3, 3.3, 4.5, 5, 5.5, 6, 8]

result = {}
for speed in speeds:
    for dist in distances:
        result[(dist, speed)] = calculate_time_in_warp(speed, 200, dist)

print("{:9s}".format(""), end="")
for speed in speeds:
    print("{:9.2f}".format(speed), end="")

last_dist = 1e999
for x,y in sorted(result.keys()):
    dist = get_distance(x)
    if (y < last_dist):
        print("\n{:7.5n} {:s}".format(dist[0],dist[1]), end="")
    last_dist = y

    print("{:9.0f}".format(math.ceil(result[x,y])), end="")

print()
```
### **输出**
**<div align="center" style="font-size: 1.2em">跃迁速度(AU/s)</div>**
|  Distance  |  1.36  |  1.50  |  2.00  |  2.20  |  2.50  |  2.75  |  3.00  |  3.30  |  4.50  |  5.00  |  5.50  |  6.00  |  8.00
|  --------  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----  |  ----
|    150 KM  |    22  |    20  |    16  |    14  |    13  |    12  |    11  |    10  |     8  |     7  |     7  |     6  |     6
|  1e+06 KM  |    48  |    44  |    33  |    30  |    27  |    25  |    23  |    21  |    16  |    14  |    13  |    12  |    11
|      1 AU  |    63  |    57  |    43  |    40  |    35  |    32  |    29  |    27  |    20  |    18  |    17  |    15  |    14
|      2 AU  |    65  |    59  |    45  |    41  |    36  |    33  |    30  |    28  |    21  |    19  |    17  |    16  |    15
|      5 AU  |    67  |    61  |    47  |    43  |    38  |    34  |    32  |    29  |    22  |    19  |    18  |    16  |    15
|     10 AU  |    71  |    65  |    49  |    45  |    40  |    36  |    33  |    30  |    23  |    20  |    19  |    17  |    16
|     20 AU  |    78  |    71  |    54  |    49  |    44  |    40  |    37  |    33  |    25  |    22  |    21  |    19  |    17
|     50 AU  |   100  |    91  |    69  |    63  |    56  |    51  |    47  |    43  |    32  |    28  |    26  |    24  |    21
|    100 AU  |   137  |   125  |    94  |    86  |    76  |    69  |    63  |    58  |    43  |    38  |    35  |    32  |    27
|    200 AU  |   211  |   191  |   144  |   131  |   116  |   105  |    97  |    88  |    65  |    58  |    53  |    49  |    40
