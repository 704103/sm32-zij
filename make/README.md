##笔记
####包含代码,单词,函数
### GPIO

```c
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE);
GPIO_InitTypeDef GPIO_InitStruct;  

GPIO_InitStruct.GPIO_Mode=GPIO_Mode_Out_PP ;
GPIO_InitStruct.GPIO_Pin=GPIO_Pin_0 ;
GPIO_InitStruct.GPIO_Speed=GPIO_Speed_50MHz ;
GPIO_Init(GPIOB,&GPIO_InitStruct);
```

struct	结构

Init 	初始化

periphery 外围

 Mode 型号

Speed 速度

Set 设置

Bits



### TIM

```c
//硬件定时器
void tim2_init(void)
{
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStruct;
	
	TIM_InternalClockConfig(TIM2); //选择时钟源,使用内部时钟,默认是内部时钟,可以不写
	TIM_TimeBaseInitStruct.TIM_ClockDivision= TIM_CKD_DIV1;		//分频
	TIM_TimeBaseInitStruct.TIM_CounterMode= TIM_CounterMode_Up;		//向上计数模式
	TIM_TimeBaseInitStruct.TIM_Period= 10000-1;		//计数值,72M/7200分频值,计10000次为1秒
	TIM_TimeBaseInitStruct.TIM_Prescaler= 7200-1;		//分频
	TIM_TimeBaseInitStruct.TIM_RepetitionCounter= 0;	//重复计数器,用不上
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStruct);
	
	TIM_ClearFlag(TIM2, TIM_FLAG_Update);	//在设置定时器中断前手动清楚一次标志位,让计数器从0开始计算
	TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE);		//配置定时器中断,TIM_IT_Update是中断标志位
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);		//NVIC分组配置
	NVIC_InitTypeDef NVIC_InitStruct;		
	NVIC_InitStruct.NVIC_IRQChannel=TIM2_IRQn ;			//通道选择
	NVIC_InitStruct.NVIC_IRQChannelCmd=ENABLE ;	
	NVIC_InitStruct.NVIC_IRQChannelPreemptionPriority= 1;		//配置抢占优先级
	NVIC_InitStruct.NVIC_IRQChannelSubPriority=1 ;		//响应优先级
	NVIC_Init(&NVIC_InitStruct);
	TIM_Cmd(TIM2,ENABLE);
	
}

void TIM2_IRQHandler(void)
{
	//判断标志位状态
	if(TIM_GetITStatus(TIM2,TIM_IT_Update)==SET)	//判断标志位为SET
	{
		if(num==1){
			num=0;
			GPIO_ResetBits(GPIOB,GPIO_Pin_0);
		}
		else{
			num=1;
			GPIO_SetBits(GPIOB,GPIO_Pin_0);
		}
		 
		TIM_ClearITPendingBit(TIM2,TIM_IT_Update);		//清楚标志位
	} 
}


//函数
TIM_TimeBaseInit(TIM2, &TIM_TimeBaseInitStructure);		//传入结构体,初始化指定硬件定时器
TIM_ClearFlag(TIM2, TIM_FLAG_Update);	//在设置定时器中断前手动清楚一次标志位,让计数器从0开始计算
TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE);		//配置定时器中断,TIM_IT_Update是中断标志位
TIM_Cmd(TIM2, ENABLE);		//定时器使能

TIM_GetITStatus(TIM2, TIM_IT_Update) == SET;	//检查定时器中断
TIM_ClearITPendingBit(TIM2, TIM_IT_Update);		//清楚中断标志位
TIM_GetCounter(TIM2);			//获取定时器计数值

第二步选择时钟源，有多个函数，对应都可见上框图：
TIM_InternalClockConfig 使用内部时钟,默认选择内部,可以不配置
TIM_ITRxExternalClockConfig 使用 ITRx 即其它定时器的时钟
TIM_TIxExternalClockConfig 使用 TIx 捕获通道的时钟，按前文这里还可以进行极性选择和滤波器
TIM_ETRClockMode1Config //使用外部时钟模式 1 路线的 ETR 时钟,课程演示使用按键计数达到定时效果就是这个模式
TIM_ETRClockMode2Config 使用走外部时钟模式 2 路线的 ETR 时钟，如果不需要触发输入的功能，这个和前一个可以互换
TIM_ETRConfig 不是用于选择时钟，是用于单独配置 ETR 的预分频器、极性、滤波器等参数

;TIM_EETRClockMode2Config(TIM2, TIM_ExtTRGPSC_OFF, TIM_TxtTRGPolarity_NonInverted, 0x00);
// 四个参数第一个是选择配置 TIM2
// 第二个是不对外部时钟输入进行预分频
// 第三个是极性选择，选了个不反相（也就是上边沿或高电平有效）
// 第四个是选择滤波器的采样频率和采样点数，具体对应关系手册有，这里就写个不需要了
```

Line 线

Config 配置

Source 来源

Port 端口

ENABLE 使能

disable 失能

Channel 通道

Base 基础

Priority 优先级

PreemptionPriority  抢占优先级

 Sub Priority 响应优先级

TypeDef 定义类型

Clock 时钟

Division 分开

Counter 计数器

Period 周期

Prescaler 预分频

Polarity 极性

Output State 输出 状态

Pulse 脉搏



### PWM

步骤配置：

1.  RCC 开启 TIM 和 GPIO 时钟，选择时钟源（这里和定时中断案例一样，选用内部时钟源即可），配置时基单元，这部分和前面一样。
2.  配置输出比较单元，包括 CCR 的值、输出比较模式、极性选择和输出使能等。
3.  配置 GPIO 引脚为复用推挽输出模式。
4.  运行控制，启动定时器。

```C
void PWM_Init(void)
{
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3, ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE); 
	
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;		
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);
	
	TIM_InternalClockConfig(TIM3);		//选择内部时钟源
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up;
	TIM_TimeBaseInitStructure.TIM_Period = 100 - 1;		//ARR
	TIM_TimeBaseInitStructure.TIM_Prescaler = 720 - 1;		//PSC
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;
	TIM_TimeBaseInit(TIM3, &TIM_TimeBaseInitStructure);
    
	//配置输出比较单元。输出比较共有四个通道，其标准库中的初始化函数对应为 TIM_OCxInit（x 为 1、2、3、4），参数是 TIM 的索引和一个结构体，可以用 TIM_OCStructInit 给它赋一个默认值。

     TIM_OCInitTypeDef TIM_OCInitStruture; 
    // 这里其它一些例如 .TIM_OCIdleState、.TIM_OCNIdleState、.TIM_OCNPolarrity 等参数是高级定时器才用的，
    // 通用定时器用不到，而这些局部的参数不赋初值则值不稳定，所以最好加一句这个，赋一个初始值：
    TIM_OCStructInit(&TIM_OCInitStruture); 
    TIM_OCInitStruture.TIM_OCMode = TIM_OCMode_PWM1; // 输出比较模式，设置为 PWM 模式 1
    TIM_OCInitStruture.TIM_OCPolarity = TIM_OCPolarity_High; // 极性选择，设置为高极性（REF 不翻转）
    TIM_OCInitStruture.TIM_OutputState = TIM_OutputState_Enable; // 输出使能，设置使能
    TIM_OCInitStruture.TIM_Pulse = 50; // CCR，前面计算得 50
    TIM_OC3Init(TIM3, &TIM_OCInitStruture);  
    
	TIM_Cmd(TIM3, ENABLE);
}

void PWM_SetCompare1(uint16_t Compare)
{
	TIM_SetCompare3(TIM3, Compare);
}


//函数
TIM_SetCompare3(TIM3, Compare);			//设置 TIMx 捕获比较 2 寄存器值


TIM_ForcedOCxConfig 	//用于配置强置输出模式（不常用，因为可以用 0% 或 100% 占空比的 PWM 信号替代）。
TIM_OCxPreloadConfig 	//用于配置 CCR 寄存器的预装功能（即影子寄存器，缓冲机制）。
TIM_OCxFastConfig 	//用于配置快速使能（TIM 的单脉冲模式指就进行一个计数周期，不循环，配合 PWM 模式就可以实现在检测到信号后延迟一段时间产生一段电平信号。快速使能是它的一个特殊功能，可以避免一些前置操作对最小延时时间的限制，具体见手册单脉冲模式部分）。
TIM_ClearOCxRef 	//对应手册中外部事件时清除 REF 信号的部分。
还有个比较重要的，TIM_CtrlPWMOutputs 	//用于高级定时器输出 PWM 时使能主输出，否则它不会正常输出。
//和一些运行时修改参数的函数：
TIM_OCxPolarityConfig 和 TIM_OCxNPolarityConfig 	//用于单独设置输出比较的极性（带 N 表示高级定时器中的互补通道，而 OC4 没有互补通道，所以 OC4 也就没有 OC4N 的那个函数）。
TIM_CCxCmd 和 TIM_CCxNCmd 用于单独设置输出使能。
TIM_SelectOCxM 用于单独修改输出比较模式（这里的 x 是真 x，不代表数字，通道序号在参数里）。
TIM_SetComparex 用于单独更改 CCR 寄存器的值（相当于即时修改占空比，所以这个比较重要）。
```



### USART

```c
void Serial_Init(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_USART1, ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
	
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);
	
	USART_InitTypeDef USART_InitStructure;
	USART_InitStructure.USART_BaudRate = 9600;		//波特率
	USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;	//
	USART_InitStructure.USART_Mode = USART_Mode_Tx | USART_Mode_Rx;		//发送模式和接受模式
	USART_InitStructure.USART_Parity = USART_Parity_No;		//奇偶检验位
	USART_InitStructure.USART_StopBits = USART_StopBits_1;		//1个停止位
	USART_InitStructure.USART_WordLength = USART_WordLength_8b; 	//数据位:8位
	USART_Init(USART1, &USART_InitStructure);
	
	USART_ITConfig(USART1, USART_IT_RXNE, ENABLE);	//中断配置,接收中断标志位
	
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
	
	NVIC_InitTypeDef NVIC_InitStructure;
	NVIC_InitStructure.NVIC_IRQChannel = USART1_IRQn;
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;
	NVIC_Init(&NVIC_InitStructure);
	
	USART_Cmd(USART1, ENABLE);
}
void Serial_SendByte(uint8_t Byte)
{
	USART_SendData(USART1, Byte);		//串口发送函数
	while (USART_GetFlagStatus(USART1, USART_FLAG_TXE) == RESET);	//等待发送完成,USART_FLAG_TXE发送中断标志位
}
void USART1_IRQHandler(void)
{
	if (USART_GetITStatus(USART1, USART_IT_RXNE) == SET)
	{
		Serial_RxData = USART_ReceiveData(USART1);
		Serial_RxFlag = 1;
		USART_ClearITPendingBit(USART1, USART_IT_RXNE);
	}
}

//函数
USART_SendData(USART1, Byte);		//串口发送函数
USART_ReceiveData(USART1);		//返回 USARTx 最近接收到的数据
USART_GetFlagStatus(USART1, USART_FLAG_TXE) ;	//获取USART 标志位
USART_ClearITPendingBit(USART1, USART_IT_RXNE);		//清除 USARTx 的中断待处理位
```



### AD

```C
void AD_Init(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
	
	RCC_ADCCLKConfig(RCC_PCLK2_Div6);	//设置 ADC1 分频	分频后 ADC1 的时钟（ADCCLK）不能超过 14Mhz。 这个我们设置分频因子位 6，时钟为 72/6=12MHz
	
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;	//模拟输入,AD专属模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);
	
	ADC_RegularChannelConfig(ADC1, ADC_Channel_0, 1, ADC_SampleTime_55Cycles5);	//设置指定 ADC 的规则组通道，设置它们的转化顺序和采样时间	ADC_Channel_0,选择ADC通道0	1,规则组采样顺序。取值范围 1 到 16。	ADC_SampleTime 设定了选中通道的 ADC 采样时间,采样时间为 55.5 周期
	
	ADC_InitTypeDef ADC_InitStructure;
	ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;	//ADC 工作模式:独立模式
	ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;		 //ADC 数据右对齐
	ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;		//转换由软件而不是外部触发启动
	ADC_InitStructure.ADC_ContinuousConvMode = DISABLE;		//AD 单次转换模式
	ADC_InitStructure.ADC_ScanConvMode = DISABLE;		//AD 单通道模式
	ADC_InitStructure.ADC_NbrOfChannel = 1;		//顺序进行规则转换的 ADC 通道的数目 1
	ADC_Init(ADC1, &ADC_InitStructure);
	
	ADC_Cmd(ADC1, ENABLE);		//使能指定的 ADC1
	
	ADC_ResetCalibration(ADC1);		//开始指定 ADC1 的校准状态
	while (ADC_GetResetCalibrationStatus(ADC1) == SET);		//等待复位校准结束
	ADC_StartCalibration(ADC1);		//开始校准 AD 
	while (ADC_GetCalibrationStatus(ADC1) == SET);		 //等待校准 AD 结束
}

uint16_t AD_GetValue(void)		//读取 ADC 值。
{
	ADC_SoftwareStartConvCmd(ADC1, ENABLE);		//使能软件转换功能
	while (ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC) == RESET);		;//等待转换结束
	return ADC_GetConversionValue(ADC1);		//返回最近一次 ADC1 规则组的转换结果
}
```



### IIC

