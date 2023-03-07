## 温工有道云作业

#### 01 GPIO
【01】知识点：GPIO输出模式
编程实现流水灯/跑马灯。

【02】知识点：GPIO输入模式
编程实现按键控制灯的亮与灭。

#### 02时钟体系
【01】知识点：PLL调节系统时钟频率
基于跑马灯的代码，调节system_stm32f4xx.c中的 PLL_N 值，观察跑马灯速度变化。
提示：
保证当前的外部晶振的宏定义要正确配置为8MHz，及PLL_M的值为8。
调节PLL_N的值为432的时候，那么CPU跑的频率能够高达216MHz！（满血版/超频版/打鸡血）
调节PLL_N的值为192的时候，那么CPU跑的频率能够高达96MHz！

【02】知识点：时钟源切换
基于跑马灯的代码，添加时钟源切换功能（支持 HSI、HSE、PLL），并能够使用按键切换，观察跑马灯速度变化。


#### 03位带操作
【01】知识点：端口引脚映射，实现位操作。
使用位带映射GPIO中的ODR寄存器，控制LED灯的亮与灭。

【02】知识点：端口引脚映射，实现位操作。
使用位带映射GPIO中的IDR寄存器，按键控制LED灯的亮与灭。
提示：
编译器选择-O2优化等级
寄存器的访问要添加volatile关键字

【03】知识点：volatile关键字使用技巧。
使用位带映射寄存器地址若不加该关键字有什么影响。
#### 04外部中断
【01】知识点：中断配置
编程实现多个按键中断。

【02】知识点：抢占优先级
编程实现多个按键中断抢占CPU使用权，观察抢占优先级触发的现象，并详细描述原因。

【03】知识点：响应优先级
编程实现两个按键中断同时发生，观察响应优先级触发的现象，并详细描述原因。
提示：

【04】知识点：中断分组
修改中断分组，观察之前与现在的中断实验现象是否有发生变化，并详细描述原因。
#### 05启动文件
【01】知识点：启动文件的作用。
以下为启动文件的英文说明，翻译以下英文，并了解其作用。
;* Description        : STM32F40xxx/41xxx devices vector table for MDK-ARM toolchain. 
;*                      This module performs:
;*                      - Set the initial SP
;*                      - Set the initial PC == Reset_Handler
;*                      - Set the vector table entries with the exceptions ISR address
;*                      - Configure the system clock and the external SRAM mounted on 
;*                        STM324xG-EVAL board to be used as data memory (optional, 
;*                        to be enabled by user)
;*                      - Branches to __main in the C library (which eventually
;*                        calls main()).
;*                      After Reset the CortexM4 processor is in Thread mode,
;*                      priority is Privileged, and the Stack is set to Main.

【02】知识点：了解中断跳转的过程
进入仿真界面，跟踪中断的跳转。用画图软件绘制中断跳转过程。
#### 06系统定时器
【01】知识点：熟悉SysTick_Config函数
基于SysTick_Config函数，配置系统定时器触发1ms定时中断，并实现LED不同的闪烁时间。
LED1 100ms
LED2 330ms
LED3 1500ms
LED4 2200ms

思考：
配置系统定时器触发最大的定时时间为多少？

【02】知识点：熟悉系统定时器相关寄存器
基于系统定时器相关寄存器，编写出微秒和毫秒的延时函数。
提示：
在《Cortex M3与M4权威指南》P316页有以下英文描述：
If you want to use the SysTick timer in polling mode, you can use the count flflag
in the SysTick Control and Status Register (SysTick->CTRL) to determine when the
timer reaches zero. For example, you can create a timed delay by setting the SysTick
timer to a certain value and waiting until it reaches zero:

```c

SysTick->CTRL = 0; // Disable SysTick
SysTick->LOAD = 0xFF; // Count from 255 to 0 (256 cycles)
SysTick->VAL = 0; // Clear current value as well as count flag
SysTick->CTRL = 5; // Enable SysTick timer with processor clock
while ((SysTick->CTRL & 0x00010000)==0);// Wait until count flag is set
SysTick->CTRL = 0; // Disable SysTick
```

相关寄存器

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/DA4792405FCC47E79289DDF7525EA0F5/159E6A82EBED436A8BBB02A463420DA1/12448)



#### 07硬件定时器
【01】知识点：单定时器硬件时钟频率及定时时间的配置。
编程实现定时器3触发500ms定时中断，控制LED1闪烁。。

【02】知识点：多定时器硬件时钟频率及定时时间的配置。
编程多定时器实现以下功能：
定时器1触发100ms定时中断，控制LED1闪烁。
定时器2触发200ms定时中断，控制LED2闪烁。
定时器3触发500ms定时中断，控制LED3闪烁。
定时器8触发2000ms定时中断，控制LED4闪烁。

提示：
1.中断号
定时器1中断号：TIM1_UP_TIM10_IRQn
定时器2中断号：TIM2_IRQn
定时器3中断号：TIM3_IRQn
定时器8中断号：TIM8_UP_TIM13_IRQn
2.中断服务函数
定时器1中断服务函数：TIM1_UP_TIM10_IRQHandler
定时器2中断服务函数：TIM2_IRQHandler
定时器3中断服务函数：TIM3_IRQHandler
定时器8中断服务函数：TIM8_UP_TIM13_IRQHandler
#### 08脉冲宽度调制
01】知识点：掌握单通道输出。

使用 PWM 控制 LED1的亮度。

【02】知识点：掌握多通道输出。
使用 PWM 控制 LED1、LED3、LED4 产生渐亮与渐灭的控制，模仿手机的通知灯。
提示：
定时器 1 属于高级定时器，使能输出的时候要额外添加函数“TIM_CtrlPWMOutputs(TIM1,ENABLE);”

思考题：
LED3和LED4共用TIM1，分别为通道3和通道4，那么TIM1_CH3和TIM1_CH4输出频率可以不一样吗？占空比可以不一样吗？
答案：
输出的频率肯定是一样的，但输出的占空比可以不一样。


【03】知识点：强化输出频率与占空比的计算技巧。
如果控制 LED 灯的频率在 20Hz、30Hz、50Hz、100Hz，效果是怎样的？代码如何修改？
答：当尝试去调整频率低于 30Hz 的情况下，可以看到灯在闪烁。

1.假如控制 LED 灯的频率在 20Hz 参考代码如下：
void tim14_init(void)
{
.......
//10000 是 10KHz，是定时器 14 的时钟源，20 就是定时器 14 输出方波的频率，那么计数值为 499.
TIM_TimeBaseStructure.TIM_Period = (10000/20)-1;
......
}
int main(void)
{
.....
//设置定时器 14 通道 1 的比较值为 250，那么占空比=250/（499+1）为 50%
TIM_SetCompare1(TIM14,250);
while(1);
}

2.为了更加方便设置频率与占空比，可以封装以下的函数：

设置定时器 14 的 PWM 频率
void tim14_set_freq(uint32_t freq)
{
/*定时器的基本配置，用于配置定时器的输出脉冲的频率为 freq Hz */
TIM_TimeBaseStructure.TIM_Period = (10000/freq)-1; //设置定时脉冲的频率
TIM_TimeBaseStructure.TIM_Prescaler = 8400-1; //第一次分频，简称为预分频
TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;
tim14_cnt= TIM_TimeBaseStructure.TIM_Period;
TIM_TimeBaseInit(TIM14, &TIM_TimeBaseStructure);
}

设置定时器 14 的 PWM 占空比 0%~100%

void tim14_set_duty(uint32_t duty)
{
uint32_t cmp=0;
cmp = (tim14_cnt+1) * duty/100;
TIM_SetCompare1(TIM14,cmp);
}


【04】音乐播放。
编程驱动蜂鸣器实现音乐播放，例如播放中音1~中音7。
提示：以下为C调音符与频率对照表
#### 09串☐
【01】知识点：串口1硬件连接。
阅读原理图及观察开发板，正确跳线M4引脚连接到USB转串口芯片，电脑端安装好CH340驱动，并正常识别到串口号，如下图。

【02】知识点：串口数据收发。
编程实现串口1的数据收发。
提示：
使用串口调试软件，如下图。

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/85BCA74EB2C041EBA0658873A119C9E2/9A84974628634C65BD7D0CA8551993AF/12456)

【03】知识点：重映射printf函数。
编程实现串口1支持printf格式化输出。
提示：
在keil的帮助手册去搜索fputc关键字，如下图。

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/85BCA74EB2C041EBA0658873A119C9E2/3179B0AF0AE34906879CD4AE99341144/12462)

方法1：

```c
#include <stdio.h>

struct __FILE { int handle; /* Add whatever you need here */ };
FILE __stdout;
FILE __stdin;

int fputc(int ch, FILE *f) {
	
 
USART_SendData(USART1,ch);
while(USART_GetFlagStatus(USART1,USART_FLAG_TXE)==RESET);
return(ch); 

}
```



方法2：
工程中勾选“Use MicroLIB”，如下图。

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/85BCA74EB2C041EBA0658873A119C9E2/9289E66882974A36ADB8BC72AA7C6803/12459)

```c
#include <stdio.h>

int fputc(int ch, FILE *f) {
 
    USART_SendData(USART1,ch);
    while(USART_GetFlagStatus(USART1,USART_FLAG_TXE)==RESET);
    return(ch); 

}
```



【04】知识点：串口控制硬件
使用PC通过串口发送数据给开发板实现灯的控制，要求如下：
接收到数据0x00，则LED1点亮；接收到数据0xF0，则LED1熄灭！
接收到数据0x01，则LED2点亮；接收到数据0xF1，则LED2熄灭！
接收到数据0x02，则LED3点亮；接收到数据0xF2，则LED3熄灭！
接收到数据0x03，则LED4点亮；接收到数据0xF3，则LED4熄灭！

注意：使用单片机多功能调试助手的串口调试功能发送字符与16进制数细节如下图。

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/85BCA74EB2C041EBA0658873A119C9E2/FA3FC6C569AE4A4A86A86E455B291FD2/12453)

#### 10超声波
【01】知识点：初步接触时序图。
1.实物图

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/92D74885B42F4D6784E800D71058217D/B5748B1F67B44CBBA961DEFE747BCC19/12455)

图1 超声波模块提示：
触发信号引脚、回响信号引脚连接到开发板的任意IO引脚。

2.时序图

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/92D74885B42F4D6784E800D71058217D/B023F08D6CE04BA3BAC6D7DBD6BC412E/12464)
3.要求

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/92D74885B42F4D6784E800D71058217D/DE2ECFAC811C4F89A97938F0490D9A27/12458)

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/92D74885B42F4D6784E800D71058217D/A29A344E21F149D3B6FB8E43CF9CFDA1/12461)

结合图1与图2，编写程序实现超声波测距，按照生活中倒车雷达实现以下要求：
1）串口显示距离
2）LED灯表示不同的安全等级
＞45cm：理想安全区，所有LED灯熄灭
35~45cm：非常安全区，亮1盏LED灯
25~35cm：安全区，亮2盏LED灯
15~25cm：警告区，亮3盏LED灯
＜15cm：危险区，亮4盏LED灯
3）拓展功能，蜂鸣器控制
声音大小能通过KEY1与KEY2调整
进入安全区，蜂鸣器开始工作，随着距离的减小，频率加快
提示：控制延时时间就能控制蜂鸣器频率。

#### 11温湿度模块
【01】知识点：时序图阅读进阶。

1. 核心内容

- 通信开始、通信过程、通信结束
- 电平时间有效检测
- 数据0与数据1的判断

1. 时序图

- 整个通讯过程

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/3250400EDD6944609F9230DF82222D40/A119361DDC6047D0BB5B46708EF27143/12460)

- 通信开始

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/3250400EDD6944609F9230DF82222D40/08E0A82A52B84F2B869A443EAE83C78B/12457)

- 数据0

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/3250400EDD6944609F9230DF82222D40/F55653D9919B4033B4B0EECCA976AD04/12463)

- 数据1

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/3250400EDD6944609F9230DF82222D40/0060027BEB044DDC9A973FE54B51AD09/12454)

1. 编写代码实现温湿度数据采集。

#### 12蓝牙通信
【01】知识点：AT指令。
基于AT指令格式，配置蓝牙模块的名字。
提示：
1）建议使用串口3，通过杜邦线连接到蓝牙模块
2）认真观察原理图与跳线帽

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/5716BCC0A1E44348ABC0C8CADF9743E4/6CC463FFF9B2493C86BFF1C76F24892E/12471)

【02】知识点：数据透传。

【02】知识点：数据透传。
使用手机蓝牙控制开发板，实现灯的亮灭，要求如下：
接收到数据‘0’，则LED1点亮；接收到数据‘a’，则LED1熄灭！
接收到数据‘1’，则LED2点亮；接收到数据‘b’，则LED2熄灭！
接收到数据‘2’，则LED3点亮；接收到数据‘c’，则LED3熄灭！
接收到数据‘3’，则LED4点亮；接收到数据‘d’，则LED4熄灭！

【03】
通过串口1或手机进行控制，添加以下功能：
手机能够控制灯光占空比、蜂鸣器开/关，要求发送以下字符串，同时开发板作出响应。
设置灯光占空比
串口1或手机： "duty=50#"
开发板: "duty set 50 ok\r\n"
蜂鸣器开/关
串口1或手机： "beep=on#"
开发板: "beep on ok\r\n"
串口1或手机： "beep=off#"
开发板: "beep off ok\r\n"
手机查询当前的温湿度
串口1或手机： "temp?#"
开发板： "32.0\r\n"
串口1或手机： "humi?#"
开发板： "68.0\r\n" 
拓展:
设置某个灯占空比
串口1或手机： "duty=1=50#"
开发板: "duty set led1 50 ok\r\n"
提示：
重点的字符串函数：strtok、atoi、strstr
蓝牙模块的数据报长度不能超过20个字节。

项目：
数据报格式
头部数据+命令码+数据长度+数据内容+校验码（校验和、CRC16）
0xAA 0x01 0x01 0x01 校验码1 校验码2
0xAA 0x02 0x03 0x0A 0x0B 0x0C 校验码1 校验码2

例如，基于上述的数据包格式来控制某个灯光的亮度。
命令码             灯
0x01               LED1
0x02               LED2
0x03               LED3
0x04               LED4

控制LED1的占空比为50%，发送数据包的格式如下：
0xAA 0x01 0x01 0x32（50） 校验和

控制LED2的占空比为80%，发送数据包的格式如下：
0xAA 0x02 0x01 0x50（80） 校验和

函数strstr

```C
包含文件：string.h
函数原型：char *strstr(char *str1, const char *str2)
语法：
str1: 被查找目标　string expression to search.
str2: 要查找对象　The string expression to find.
返回值：若str2是str1的子串，则返回str2在str1的首次出现的地址；如果str2不是str1的子串，则返回NULL。
例子：
char str[]="1234xyz";
char *str1=strstr(str,"34");
cout << str1 << endl;
显示的是: 34xyz
```

函数atoi

```C
定义： 将字符串转换成整型数，跳过前面的空格字符，直到遇上数字或正负号才开始做转换，而再遇到非数字或字符串时（'\0'）结束转化，并将结果返回（返回转换后的整型数）。
头文件: #include <stdlib.h>
函数原型：int atoi(const char *nptr);
实现时需要注意的问题：
（1）检查字符串是否为空，为空则返回
（2）对空格和规定字符的处理, 若是，则跳过，不影响接下来的处理。
```



函数strtok



```c
描述：C 库函数 char *strtok(char *str, const char *delim) 分解字符串 str 为一组字符串，delim 为分隔符。
声明：char *strtok(char *str, const char *delim)
参数
str -- 要被分解成一组小字符串的字符串。
delim -- 包含分隔符的 C 字符串。
返回值
该函数返回被分解的第一个子字符串，如果没有可检索的字符串，则返回一个空指针。

实例
下面的实例演示了 strtok() 函数的用法。
#include <string.h>
#include <stdio.h>

int main () {
   char str[80] = "This is-www.baidu.com-website";
   const char s[2] = "-";
   char *token;

   /* 获取第一个子字符串 */
   token = strtok(str, s);

   /* 继续获取其他的子字符串 */
   while( token != NULL ) {
      printf( "%s\n", token );
    
      token = strtok(NULL, s);
   }

   return(0);
}

输出结果：
This is 
www.baidu.com 
website
```


【04】主机角色
应用案例：

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/5716BCC0A1E44348ABC0C8CADF9743E4/1B35DC7ADC0443E1BA06EC707E8944A7/12473)

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/5716BCC0A1E44348ABC0C8CADF9743E4/5EA61EA31757451B8EB621F5BB7A5D68/12469)

实现两个蓝牙4.0模块进行通信，实现步骤如下：
1）指定一个蓝牙4.0模块为主机角色，输入以下AT指令：AT+ROLE1
2）主机角色的蓝牙4.0模块，进行搜索蓝牙设备，输入以下指令：AT+INQ，并显示内容如下：

```c
+INQS
+INQ:1 0x00158310DF50
+INQ:2 0x00158310E32A
+INQ:3 0x00158300E4D5
+INQ:4 0x00158310D064c
+INQ:5 0x00158310E191
+INQ:6 0x00158310E1DF
+INQ:7 0x00158310CF3F
+INQ:8 0x00158300E4B2
+INQE
Devices Found 8
```



3）例如连接地址序号为1的模块，地址为0x00158310DF50，输入以下指令：AT+CONN1，并显示如下

```c

+Connecting  0x00158310DF50
+Connected  0x00158310DF50
```


4）这个时候两个蓝牙模块默认进入数据透传模式，不能接受任何的AT指令了，可以愉快c地进行通信！
5）可使用板载的4个按键控制远程开发板：按键1与按键2控制灯光亮度；按键3控制蜂鸣器；按键4获取温湿度。
注：
该模块没有对应的指令实现主动断开连接，需要手动复位模块，当前模块没有提供复位键，只能断电后再上电。
当模块被配置为主机角色后，手机无法跟模块进行连接，同时使用AT+LADDR指令会显示不出模块的地址，会显示：+LADDR=00:00:00:00:00:00，解决办法必须发送AT+ROLE0指令恢复为从机角色，接着发送AT+RESET或模块重新上电。


#### 13红外遥控
​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/529D243707D84881B1482F6F99D06736/14BCA3F7E1DF494998939EE3B6B22D16/12470)

【01】知识点：NEC解码。
基于NEC解码，实现红外遥控LED亮灭。

【02】
通过红外遥控，控制开发板，实现以下功能：
LED1灯亮度控制
控制LED1~LED4灯亮灭
蜂鸣器频率、音量控制
温湿度读取
提示：将红外读取放到中断服务函数会提高硬件响应。但是要注意系统定时器意外关闭，导致温湿度读取出现问题。

```c
int32_t delay_us(uint32_t nus)
{
	uint32_t temp;

	SysTick->CTRL = 0; 					
	SysTick->LOAD = (nus*21)-1; 		
	SysTick->VAL = 0; 					
	SysTick->CTRL = 1; 					
	
	while(1)
	{
	
		temp=SysTick->CTRL;
		
		//检测count flag
		if(SysTick->CTRL & 0x00010000)
			break;
		
		//检测系统定时器是否意外关闭	
		if((SysTick->CTRL & 0x1)==0)
			return -1;		
	}
	
	SysTick->CTRL = 0; 					

	return 0;
}


int32_t  delay_ms(uint32_t nms)
{
	uint32_t t = nms;

	uint32_t temp;
	
	
	while(t--)
	{
		SysTick->CTRL = 0; 			
		SysTick->LOAD = 21000-1; 	
		SysTick->VAL = 0; 			
		SysTick->CTRL = 1; 			
		while(1)
		{

			temp=SysTick->CTRL;
			
			//检测count flag
			if(SysTick->CTRL & 0x00010000)
				break;
			
			//检测系统定时器是否意外关闭	
			if((SysTick->CTRL & 0x1)==0)
				return -1;		
		}
	}	
	
	SysTick->CTRL = 0; 	
	
	return 0;
}

```


#### 14看门狗
【01】知识点：独立看门狗超时。

编写代码实现独立看门狗的超时复位与喂狗操作。
要求如下：
1）独立看门狗超时时间为1秒
2）定时器中断频率10Hz，并在中断服务函数中进行喂狗


【02】
温湿度传感器检测温度超过自定义预警值后，自动触发看门狗复位。

【03】知识点：窗口看门狗超时。

编写代码实现窗口看门狗的超时复位与喂狗操作。

【04】知识点：窗口看门狗提前唤醒中断。
编写代码实现窗口看门狗的提前唤醒中断，实现喂狗操作。

【05】
通过串口1/蓝牙发送特定的数据，让开发板进行复位！
特定的数据：
串口1/手机：“IWDG RESET#”
开发板，复位后发送：“IWDG RESET OK#”
串口1/手机：“WWDG RESET#”
开发板，复位后发送：“WWDG RESET OK#”

提示：
IWDG喂狗可放在定时器中断服务函数喂狗，若不喂狗，在其中断服务函数直接执行while(1)，触发超时复位。
WWDG喂狗可放在窗口看门狗唤醒中断服务函数中喂狗，若不喂狗，在其中断服务函数直接执行while(1)，触发超时复位。
#### 15 RTC
【01】知识点：RTC唤醒中断。
基于RTC的唤醒中断，实时得到RTC日期与时间。

【02】知识点：RTC闹钟中断。
设计RTC的闹钟功能，在指定某个日期与时间触发。

【03】
通过串口1或许手机蓝牙发送特定的字符串修改RTC日期与时间。

特定的字符串：
修改日期：“DATE SET-2020-3-2-1#”  	//年-月-日-星期
修改时间：“TIME SET-10-20-30#”     //时-分-秒

提示：得到数值后，直接调用RTC_SetDate和RTC_SetTime就可以设置日期和时间。

提示函数：strtok atoi strstr

【04】
通过串口1或许手机蓝牙发送特定的字符串设定RTC的闹钟时间，当时间到达设定的值后，则使用蜂鸣器与led灯模拟响铃效果（滴滴，滴滴，滴滴....）持续一段时间后，自动关闭蜂鸣器与led灯。

特定的字符串：
修改闹钟日期与时间：“ALARM SET-14-20-10#” //时-分-秒
提示函数：strtok atoi str
#### 16 FLASH
【01】知识点：FLASH的读写操作。
对某一扇区连续写入64字的数据，并读取验证数据是否正确！

【02】
温湿度数据记录仪，实现步骤如下：
按下KEY1或串口1接收到”start”，启动温湿度数据记录模式，每1秒（做项目的时候，改为6秒以上，低于30秒），将读取到温湿度模块数据保存到STM32内部FLASH当中，最大记录数目100条。 
按下KEY2或串口1接收到”stop”，停止温湿度数据记录。
按下KEY3或串口1接收到”show”，显示当前所有温湿度所有记录，显示格式如下：
[001]2020/03/25 Week 6 9:40:12 Temp=30.0 Humi=92.0
[002]2020/03/25 Week 6 9:42:32 Temp=31.0 Humi=93.0
......
按下KEY4或串口1接收到”clear”，清空所有温湿度数据记录。

如何界定存储的数据达到100条？如果超过100条记录，请打印“存储已满”。

应用场景：例如智能手表存储用户的身体健康的数据；指纹锁与密码锁，指纹数据与密码数据都可以存储到FLASH。
#### 17 ADC
【01】知识点：电压值读取。
调节可调电阻，模拟电池电量的变化，基于ADC，编写代码读取其电压值。

【02】知识点：分辨率。
基于6/8/10位精度测量电压值，代码如何做出修改，对测量结果有什么影响？
--------------------------------------------------------------------------------

【03】知识点：光敏电阻。
基于光敏电阻自动控制D1灯的亮灭。

应用案例：

衣柜感应灯（可使用人体感应传感器或光敏电阻）    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/14752A3D108A43C3A374DD39AEEA86C5/347C564386AF4EE095EEB4C6E078323A/12482)


手机/平板/电脑等电子设备的屏幕自动亮度

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/14752A3D108A43C3A374DD39AEEA86C5/297DE4A9DB1342349FFB99A4E1FF8D96/12479)

【04】知识点：滤波算法。
观察ADC转换结果，数据。
提示：可参考使用中位值滤波算法

```c
#define N 12
char filter()
{
	char count,i,j;
	char Value_buf[N];
	int sum=0;
	for(count=0;count<N;count++)
		Value_buf[count]= get_ad();
	for(j=0;j<(N-1);j++)
		for(i=0;i<(N-j);i++)
			if(Value_buf[i]>Value_buf[i+1])
			{
				 temp = Value_buf[i];
				 Value_buf[i]= Value_buf[i+1];
				  Value_buf[i+1]=temp;
			}
			for(count =1;count<N-1;count++)
				sum += Value_buf[count];
			return (char)(sum/(N-2));
}
```

【05】

火焰报警器制作。

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/14752A3D108A43C3A374DD39AEEA86C5/71106E89AB41408D956C1546A146C500/12489)

火焰传感器

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/14752A3D108A43C3A374DD39AEEA86C5/9BFDEBF196794596A213D3B89545B81E/12486)

火焰传感器连接图

要求如下：

1）熟悉使用火焰传感器，建立火焰传感器与开发板的连接，AO引脚连接到CPU具有ADC功能的引脚，DO引脚连接到具有外部中断触发的引脚，了解该两个引脚的作用。

2）若火焰传感器探测到火焰的存在，则进行蜂鸣器报警。

3）实时地监测火焰报警器，并将AO引脚得到的数据输出到串口，并分析。

提示：

1）DO引脚要配置为**双边沿触发**，下降沿触发中断，上升沿触发中断。

​                EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising_Falling;               

2）在中断服务函数识别上升沿和下降沿触发，参考以下代码：

```c
void EXTI3_IRQHandler(void)
{
	//检查是否有中断请求
	if(EXTI_GetITStatus(EXTI_Line3) != RESET)
	{
		//上升沿触发
		if(PAin(3))
		{
			//蜂鸣器鸣响
			PFout(8)=1; delay_ms(50);
 			PFout(8)=0; delay_ms(50);  
 			PFout(8)=1; delay_ms(50);
 			PFout(8)=0; delay_ms(50);                   

		}
		//下降沿触发
		else
		{
			//蜂鸣器禁鸣
			PFout(8)=0;		
		}

		/* 清空外部中断控制线3的挂起位（标志位），告诉CPU，已经处理完毕，可以触发下一次的中断请求 */
		EXTI_ClearITPendingBit(EXTI_Line3);
	}
}
```



#### 18 DAC

【01】知识点：可调电压。

编写代码实现输出自定义电压值。

【02】

读取DHT11的温度值，通过DAC输出对应的电压值，接着ADC读取该电压值，转换为温度值。

提示：

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/F9E8DFD219954B019D723EDF5C708D04/9CBBA880F16A49E4B2FFF2571B41AB5C/12480)

【03】

读取DHT11的温湿度值，通过DAC输出对应的电压值，接着ADC读取该电压值，转换为温湿度值。

提示：

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/F9E8DFD219954B019D723EDF5C708D04/77382D336BEE4E55BD050A3CBC4FBDBA/12478)

#### 19 SPI



【01】知识点：读数据

参考“W25Q128_读数据.bmp”的时序图，从0地址连续读取64字节的数据。

------

【02】知识点：扇区擦除

擦除某扇区数据，步骤如下：

\1) 允许写入数据，就是解锁写保护机制，参考W25Q128_写使能.bmp

\2) Flash进行扇区擦除，参考W25Q128_扇区擦除.bmp

\3) 检查是否已经擦除完毕，参考W25Q128_状态寄存器1.bmp

\4) 禁止写入数据，就是对Flash上锁，不允许再次写入数据，参考W25Q128_写失能.bmp

------

【03】知识点：页编程

实现页编程（对某个地址写入数据），执行页编程前保证当前地址被擦除过，步骤如下：

\1) 允许写入数据，就是解锁写保护机制，参考W25Q128_写使能.bmp

\2) 对Flash进行编程，参考W25Q128_写页数据.bmp

\3) 检查是否已经编程完毕，参考W25Q128_状态寄存器1.bmp

\4) 禁止写入数据，就是对Flash上锁，不允许再次写入数据，参考W25Q128_写失能.bmp

------

【04】

SPI OLED屏：将OLED代码移植到开发板，实现OLED显示。

【05】

SPI RFID模块：将RFID代码移植到开发板，实现卡的读写！

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/5E6FB892B90E462685ED32406C58C869/12493)

**（一）硬件引脚描述**

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/7AD8847AD8CC4C89B4FFB581BB05DA0B/12487)

**（二）硬件连接参考**

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/F559BFF96DB34C02841A45761F424C28/12490)

**（三）RFID通信过程**

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/32F2F3DBC9C24189A4AAD6BF2E4973DE/12481)

**组成部分：**

卡片的电气部分只由一个天线和 ASIC 组成。

天线：卡片的天线是只有几组绕线的线圈，很适于封装到 IS0 卡片中。

ASIC：卡片的 ASIC 由一个高速（106KB 波特率）的 RF 接口，一个控制单元和一个

8K 位 EEPROM 组成。

**工作原理：**

读写器向 M1 卡发一组固定频率的电磁波，卡片内有一个 LC 串联谐振电路，其频率与读写器发射的频率相同，在电磁波的激励下， LC 谐振电路产生共振，从而使电容内有了电荷，在这个电容的另一端，接有一个单向导通的电子泵，将电容内的电荷送到另一个电容内储存，当所积累的电荷达到 2V 时，此电容可做为电源为其它电路提供工作电压，将卡内数据发射出去或接取读写器的数据。

**（四）M1卡描述**

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/6518F7B21C924F2F819177F2EF0BAF1C/12495)

**（五）提示**

1.RC522文件

​	MFRC522.c与MFRC522.h

2.RC522测试函数

​	main.c中的MFRC522Test函数

3.将MFRC522.c、MFRC522.h、MFRC522Test函数添加到M4的工程中，以模拟SPI工程作为模板

4.移植好SPI代码之后，就进行验证SPI的读和写操作，保证SPI能够正常的进行工作！

​	Write_MFRC522

​	Read_MFRC522

注：

- 当前目录有M4_RC522_Test.hex文件可以验证读写器能否正常工作！
- RC522芯片有BUG，每次操作完卡之后必须重新初始化一遍，调用MFRC522_Initializtion。

移植

（一）定义

将某平台的源码运行到新的平台，该过程称之为移植。

（二）技巧

关键**修改**跟**硬件**平台相关的代码，该代码一般为如下：

- 引脚初始化
- 引脚控制与读取
- 数据收发
- 硬件中断
- 软件延时需要修改为硬件精准延时
- ......

移植就是逐步纠正的过程。

【05】

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/05D1BC6F0C9F48E69E1467ECBEA8F685/6DDADCF553714019AF93BD67FDACF948/12484)

刷卡读取开发板信息，要求如下：

1）有效卡的识别

- 有效卡，长亮LED1，并打印出以前存储RTC的日期时间与温湿度信息，如下：

​                2019/12/25 Week 3 9:40:12 Temp=28.2 Humi=65.0              

- 非法卡，蜂鸣器长鸣报警；当该卡离开有效的识别距离，则停止报警。

2）过一会儿，将开发板当前RTC的日期时间与温湿度信息存储到卡对应的数据块。

- 存储成功，蜂鸣器滴一声示意。
- 存储失败，蜂鸣器滴两声示意。

3）当卡离开有效的识别距离，则熄灭LED1。

#### 20 I2C

【01】知识点：I2C时序图

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/D7E9A27860CF4527928DB6C337CDD135/12491)

参考编写好的页编程函数at24c02_write，完成连续读函数at24c02_read。

提示：

主机发送应答信号，编写i2c_ack发送应答信号函数，可参考i2c_send_byte函数，发送8个bit数据变更为发送1个bit数据，删除for循环。

主机接收从机数据，编写i2c_recv_byte接收字节函数，可参考i2c_wait_ack函数，接收1个bit数据变更为接收8个bit数据，添加for循环。

------

【02】

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/4635500F13DA4618860B7E175F0C09E7/12494)

OLED屏代码的移植，能够显示图片、汉字、英文、数字等。

提示：

- 阅读《0.96寸OLED使用文档新手必看V2.0》.pdf中的第五章节的移植说明 
- 阅读《0.96寸OLED使用文档新手必看V2.0》.pdf中的第六章节的取模说明 

------

【03】

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/A9AFDD90BCDF4323B63A1B917EE33A03/12501)

MPU6050代码的移植，能够得到俯仰角、横滚角、航向角。

**提示：**

**一、姿态解算**

**1.欧拉角**

用来表示三维空间中运动物体绕坐标轴旋转的情况，即物体每时每秒的姿态可以由欧拉角表示。

**2.四元数**

四元数用于物体的旋转，是一种复杂但是效率较高的旋转方式。 

对于一个物体的旋转，我们只需要知道四个值：一个旋转向量+一个旋转角度，而四元素也正是 

这样设计的：q = (x, y, z, w).其中x,y,z代表向量的三维坐标，w代表角度。

**二、欧拉角**

**现以Z轴正方向为前进方向，按照绕X/Y/Z轴可区分为pitch（俯仰角）/yaw（航向角）/roll（横滚角）**

pitch ： 俯仰，将物体绕X轴旋转（localRotationX） 

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/094F94DBCEA0497EACB1B95E4B1F9388/12499)

yaw ： 航向，将物体绕Y轴旋转（localRotationY） 

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/4E410BB25D0947F7ACCA07FA3C7AAAE8/12496)

roll： 横滚，将物体Z轴旋转（localRotationZ） 

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/AF2E6149EDFF46BB91BA6A0127F05257/12488)

头模型的姿态角，标注 

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/72F9588FBDDE463C8F0A1557A2E74E18/86F787D5EF8A4179B25EC8DDB1C1EED6/12485)



#### 21 UCOSIII

​    ![0](https://note.youdao.com/yws/public/resource/e2eac0dd45493be783e77563084fc9c3/xmlnote/EF1A921F27094491AA760B6CFC1E6F8C/47C66034E7AA4D7F95CA9ADF9B3EFB9D/12498)

【01】知识点：任务创建

编写代码，创建多个任务，并负责每一个LED灯亮灭控制。

【02】知识点：任务挂起与恢复

编写代码，通过任务挂起与恢复，实现共享资源的保护。

【03】知识点：任务删除

编写代码，某个任务运行完一次之后，删除自身。

【04】知识点：临界区

编写代码，实现多任务之间互斥访问共享资源。

【05】知识点：互斥锁

编写代码，实现多任务之间互斥访问共享资源。

【06】知识点：信号量

编写代码，实现多任务之间的同步。

【07】知识点：事件标志组

编写代码，实现某任务多个事件的等待。

【08】知识点：消息队列

编写代码，实现消息队列传送串口接收到的数据。

【09】知识点：软件定时器

编写代码，实现软件定时器定时控制灯光亮灭。

【10】知识点：等待多个内核对象

编写代码，实现多个内核对象的等待。
