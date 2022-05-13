---

title: "Matrix_mul Report" 

permalink: / 

layout: default 

---



# 矩阵乘实验报告总结

# 实验环境

编程语言：C语言

使用机器：192.168.139.21

## 环境设置

设置随机种子，防止每次随机化带来的运行时间误差

```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>

unsigned int seed = 0;
srand(seed);
```

## 矩阵乘基准程序

```C
#define TYPE double
int SIZE;

void matrix_mul_base(TYPE *matrix_1, TYPE *matrix_2, TYPE *matrix_3)
{
  for (int i = 0; i < SIZE; i++)
  {
    for (int j = 0; j < SIZE; j++)
    {
      for (int k = 0; k < SIZE; k++)
      {
        matrix_3[i * SIZE + j] += matrix_1[i * SIZE + k] * matrix_2[k * SIZE + j];
      }
    }
  }
}
```

## 矩阵优化方法

### 串行矩阵优化

1. 循环重排序
2. 消除内存引用
3. 循环展开

### 并行矩阵优化

### 异构矩阵优化

## 优化结果

<table style="text-align: center;">
	<tr>
	    <th style = "width:15%">优化方法</th>
	    <th style = "width:10%">数据规模</th>
	    <th style = "width:10%">数据类型</th>  
	    <th style = "width:15%">运行时间</th>  
        <th style = "width:10%">加速比</th>
	    <th style = "width:40%">备注</th>
	</tr>
	<tr>
	    <td rowspan="3">矩阵乘基本程序</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>7.99 s</td>
	    <td>1</td>
	    <td rowspan="3">无</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>69.58 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>—</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td rowspan="3">循环重排序</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>3.81 s</td>
	    <td>1</td>
	    <td rowspan="3">调整循环中的读取数据的顺序，更好的利用内存读取方式以及运算的交错形式来提高效率。</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>30.84 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>247.04 s</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td rowspan="3">消除内存引用</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>5.32 s</td>
	    <td>1</td>
	    <td rowspan="3">减少了对matrix_3的访问次数</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>67.66 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>1431.81 s</td>
	    <td>1</td>
	</tr>
	<tr>
	    <td rowspan="3">循环重排序+消除内存引用</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>3.34 s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>26.94 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>218.59 s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">矩阵转置</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>3.94 s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>31.63 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>258.50 s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">矩阵转置+消除内存引用</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>2.82 s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>22.84 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>184.37 s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">1*2循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>7.86 s</td>
	    <td>1</td>
	    <td rowspan="3">减少了循环的跳转和条件分支</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>68.38 s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">1*4循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">1*8循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">1*16循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">2*1循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">2*2循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">2*4循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">2*8循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">4*1循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">4*2循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">4*4循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">8*1循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">8*2循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">16*1循环展开</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
    <tr>
	    <td rowspan="3">1*2循环展开+矩阵转置</td>
	</tr>
	<tr>
	    <td>1024*1024</td>
	    <td rowspan="3">double</td>
	    <td>- s</td>
	    <td>1</td>
	    <td rowspan="3">-</td>
	</tr>
	<tr>
		<td>2048*2048</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
	<tr>
		<td>4096*4096</td>
	    <td>- s</td>
	    <td>1</td>
	</tr>
</table>