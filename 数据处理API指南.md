# 数据处理API指南

数据处理API源代码为 [calc.py](api/calc.py)，包含平均值/标准差/不确定度计算、最小二乘法线性回归、表达式及合成不确定度计算三大功能，以及科学计数法输出小工具，调用方法可参考 [exp5.py](module/exp5.py)。

## 科学计数法输出

- 调用方法：
  `numlatex(num, prec)`
- 参数：
  num：数字
  prec：有效数字位数（缺省值：5）
- 返回：
  Latex代码

## 平均值/标准差/不确定度计算

- 调用方法：
  `analyse(data, delta_b1, delta_b2, symbol, unit, confidence_C, confidence_P)`
- 参数：
  data：实验数据（一维数组）
  delta_b1：仪器最大允差 $\Delta_仪$
  delta_b2：估读最大允差 $\Delta_估$
  symbol：该物理量的符号
  unit：该物理量的单位
  confidence_C：置信系数C（缺省值：3）
  confidence_P：置信概率P（缺省值：0.95）
- 返回：
  average：平均值 $\overline{x}$
  sigma：标准差 $\sigma$
  delta_b：B类不确定度 $\Delta_B$
  unc：延伸不确定度 $U$

  averagex：平均值 $\overline{x}$ 的 Latex 代码
  sigmax：标准差 $\sigma$ 的 Latex 代码
  delta_bx：B类不确定度 $\Delta_B$ 的 Latex 代码
  uncx：延伸不确定度 $U$ 的 Latex 代码

  averagex2：平均值 $\overline{x}$ 的面向 MathML 的 Latex 代码
  sigmax2：标准差 $\sigma$ 的面向 MathML 的 Latex 代码
  delta_bx2：B类不确定度 $\Delta_B$ 的面向 MathML 的 Latex 代码
  uncx2：延伸不确定度 $U$ 的面向 MathML 的 Latex 代码
- 调用示例：
  `res_l = analyse(data["l"], 0.12, 0.05, "l", "cm")`

## 最小二乘法线性回归

- 由于最小二乘法的B类不确定度计算较复杂，且大物实验并无要求，故不提供该功能，而直接将A类不确定度视为延伸不确定度。
- 调用方法：
  `analyse_lsm(data_X, data_Y, symbol_X, symbol_Y, unit_m, unit_b)`
- 参数：
  data_X：X轴数据（一维数组）
  data_Y：Y轴数据（一维数组）
  symbol_X：X轴数据物理量的符号
  symbol_Y：Y轴数据物理量的符号
  unit_m：斜率的单位
  unit_b：截距的单位
- 返回：
  m：斜率 $m$
  b：截距 $b$
  r：线性拟合的相关系数 $r$
  s_m：斜率的标准差 $s_m$
  s_b：截距的标准差 $s_b$

  mx：斜率 $m$ 的 Latex 代码
  bx：截距 $b$ 的 Latex 代码
  rx：线性拟合的相关系数 $r$ 的 Latex 代码
  s_mx：斜率的标准差 $s_m$ 的 Latex 代码
  s_bx：截距的标准差 $s_b$ 的 Latex 代码

  mx2：斜率 $m$ 的面向 MathML 的 Latex 代码
  bx2：截距 $b$ 的面向 MathML 的 Latex 代码
  rx2：线性拟合的相关系数 $r$ 的面向 MathML 的 Latex 代码
  s_mx2：斜率的标准差 $s_m$ 的面向 MathML 的 Latex 代码
  s_bx2：截距的标准差 $s_b$ 的面向 MathML 的 Latex 代码
  
  注：严格来讲标准差并非不确定度，但在大物实验中将其视为不确定度已经足够
- 调用示例：
  `res_lsm = analyse_lsm(data["F"], data["b"], "F", "b", "cm/N", "cm")`

## 表达式及合成不确定度计算

- 调用方法：
  `analyse_com(exp, varr, constt, unit, confidence_P)`
- 参数：
  exp：计算表达式（字符串）
  varr：变量（元组），元组的每个元素均为元组，该子元组的第1个元素为变量名，第2个元素为变量值，第3个元素为其不确定度
  constt：常量（元组），元组的每个元素均为元组，该子元组的第1个元素为常量名，第2个元素为常量值
  unit：该物理量的单位
  confidence_P：置信概率P（缺省值：0.95）
  注意：变量/常量子元组需要使用2个括号；若仅计算表达式，不计算合成不确定度，则传入 `varr=()`，并将所有变量信息放在 `constt` 元组中
- 返回：
  ans：表达式计算值
  unc：合成不确定度计算值

  ansx：表达式计算的Latex代码
  uncx：合成不确定度计算的Latex代码
  finalx：最终结果的Latex代码

  ansx2：表达式计算的面向MathML的Latex代码
  uncx2：合成不确定度计算的面向MathML的Latex代码
  finalx2：最终结果的面向MathML的Latex代码
- 调用示例：
  `g_com = analyse_com("g=4*pi**2*l/T**2", (("l",0.6976,0.0021), ("T",1.677,0.0064)), (("pi",3.1415926),), "m/s^2")`