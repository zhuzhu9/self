# I2C

[TOC]

## 1、简介

I2C：双向，二线，同步，串行，总线。

一共有两根线

多主从结构

## 2、特性

两条线：SDA（串行数据线）和SCL（串行时钟线）

**没有严格的波特率要求**，例如使用RS232，**主设备生成总线时钟**

所有组件之间都存在简单的主/从关系，连接到总线的每个设备均可通过唯一地址进行软件寻址

I²C是真正的多主设备总线，可以一个主机连接多个从机，也可以多个主机控制一个或者多个从机，可提供仲裁和冲突检测。
最大主设备数：无限制；最大从机数：127

传输速度。标准模式：Standard Mode = 100 `Kbps`；快速模式：Fast Mode = 400 `Kbps`；高速模式： High speed mode = 3.4 `Mbps`；超快速模式： Ultra fast mode = 5 `Mbps`

## 3、硬件层

每个连接到总线的设备都有一个**独立的地址**，主机可以利用这个地址进行不同设备之间的访问。

SDA和SCL都需要接上拉电阻

I²C总线（`SDA`，`SCL`）内部都使用漏极开路驱动器（开漏驱动），因此`SDA`和`SCL` **可以被拉低为低电平，但是不能被驱动为高电平**，所以每条线上都要使用一个上拉电阻，默认情况下将其保持在高电平；

具有**三种传输模式**：标准模式传输速率为100kbit/s ，快速模式为400kbit/s ，高速模下可达 3.4Mbit/s，但目前大多I2C 设备尚不支持高速模式。

## 4、数据传输协议

SDA传输串行数据

串行数据结构为：开始条件、地址位、读写位、应答位、数据位、应答位、停止条件![I2C](图片/I2C.assets/I2C-1623924021905.jpg)

### 开始信号

SDA从**高压电平到低压电平**
SCL从**高电平到低电平**

在开始信号之后，所有从机从睡眠模式变为活动状态，等待接受地质数据           <img src="图片/I2C.assets/20201020202551889.jpg" alt="img" style="zoom:50%;" />

### 地址位

通常占七位数据（也支持10位地址位），想要向从机发送/接受数据，先要知道对应的从机的地址；

发送对应的从机的地址位，然后会匹配总线上挂载的从机的地址，进行寻找。

I2C 协议规定设备地址可以是**7 位或10 位**，实际中7 位的地址应用比较广泛。

### 读写位

读写位用来指定数据传输方向：
0：主设备传输数据到从设备
1：主设备接受数据到从设备

### 应答位（ACK/NACK）

主设备发送数据后，第九位（应答位）的时钟期间，**主设备会将SDA线拉高**。

**若从设备可以接受数据，则将SDA线拉低**，保持平稳的低电平。
**接收端的SDA 为高电平表示非应答信号(NACK)，低电平表示应答信号(ACK)**
若从设备不可以接受数据（因为某些情况），则不将SDA拉底，主设备将从新发送数据，一直到从设备进行了应答。
![在这里插入图片描述](图片/I2C.assets/20190902173105659.png)

### 数据位

数据位上的每个字节有八位，但是每次可以发送的字节数不限，由传输方进行设置

每个字节的数据传输结束后进行应答，如上（应答位），若无响应，一直重复到接受
例如：从机要完成一些其他功能后（例如一个内部中断服务程序）才能接收或发送下一个完整的数据字节，可以使时钟线SCL 保持低电平，迫使主机进入等待状态，当从机准备好接收下一个数据字节并释放时钟线SCL 后数据传输继续。

### 停止位

当主设备决定停止通讯时，发送停止信号：
		先将SDA线从低电压电平切换到高电压电平；
		再将SCL线从高电平拉到低电平；
<img src="图片/I2C.assets/20201020202710717.jpg" style="zoom:50%;" />

### 数据有效性

I2C 使用SDA信号线来传输数据，使用SCL 信号线进行数据同步。

SDA数据线在SCL的每个时钟周期传输一位数据。

传输时，SCL为高电平的时候SDA表示的数据有效，即此时的SDA为高电平时表示数据“1”，为低电平时表示数据“0”。
当**SCL为低电平时，SDA的数据无效**，一般在**这个时候SDA进行电平切换，为下一次表示数据做好准备**。

![在这里插入图片描述](图片/I2C.assets/20190902172815368.png)

## 5、多个主设备时

当两个主设备试图通过SDA线路同时发送或接收数据时，同一系统中的多个主设备就会出现问题。

为了解决这个问题，**每个主设备都需要在发送消息之前检测SDA线是低电平还是高电平**：
		如果SDA线为低电平，则意味着另一个主设备可以控制总线，并且主设备应等待发送消息。
		如果SDA线为高电平，则可以安全地发送消息。

## 6、速度

I2C由两根线组成，数据线SDA，时钟线SCL，标准模式的时候，SCL的频率可达100KHZ，高速模式的时候，频率可达400KHZ。频率越高，对上升沿的要求越高。

现在拿100KHZ的频率来说，一个SCL周期为10uS，这其中包括一个上升沿，一个下降沿，一个高电平时间和一个低电平的时间。如果SCL的上升沿要15uS，那么波形都还么有上升到1就开始向0变化了，这样连I2C的起始条件都不能达到。

因为I2C从设备一般都是MOS工艺，所以I2C总线都有上拉电阻，而传输线是有电容效应的，你接的设备越多，电容会增大，在上升的时候就会造成延时，连接到总线的接口数量只由总线电容限制决定。

我们也曾经遇到过类似问题，就是外围从设备I2C的速度太慢，用主设备的I2C去通讯出错，最后的解决方法是不用主设备的I2C，而是用I/O去模拟I2C去跟外围设备通讯。


代码

```c
HAL_StatusTypeDef HAL_I2C_Master_Transmit(I2C_HandleTypeDef *hi2c, 
                                          uint16_t DevAddress, 
                                          uint8_t *pData, 
                                          uint16_t Size, 
                                          uint32_t Timeout){
  uint32_t tickstart = 0x00U;

  /* Init tickstart for timeout management*/
  tickstart = HAL_GetTick();

  if(hi2c->State == HAL_I2C_STATE_READY){
    /* Wait until BUSY flag is reset */
    if(I2C_WaitOnFlagUntilTimeout(hi2c, I2C_FLAG_BUSY, SET, I2C_TIMEOUT_BUSY_FLAG, tickstart) != HAL_OK){
      return HAL_BUSY;
    }

    /* Process Locked */
    __HAL_LOCK(hi2c);
    
    /* Check if the I2C is already enabled */
    if((hi2c->Instance->CR1 & I2C_CR1_PE) != I2C_CR1_PE){
      /* Enable I2C peripheral */
      __HAL_I2C_ENABLE(hi2c);
    }
    
    /* Disable Pos */
    hi2c->Instance->CR1 &= ~I2C_CR1_POS;
    
    hi2c->State     = HAL_I2C_STATE_BUSY_TX;
    hi2c->Mode      = HAL_I2C_MODE_MASTER;
    hi2c->ErrorCode = HAL_I2C_ERROR_NONE;
    
    /* Prepare transfer parameters */
    hi2c->pBuffPtr    = pData;
    hi2c->XferCount   = Size;
    hi2c->XferOptions = I2C_NO_OPTION_FRAME;
    hi2c->XferSize    = hi2c->XferCount;
    
    /* Send Slave Address */
    if(I2C_MasterRequestWrite(hi2c, DevAddress, Timeout, tickstart) != HAL_OK){
      if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
        /* Process Unlocked */
        __HAL_UNLOCK(hi2c);
        return HAL_ERROR;
      }else{
        /* Process Unlocked */
        __HAL_UNLOCK(hi2c);
        return HAL_TIMEOUT;
      }
    }
    
    /* Clear ADDR flag */
    __HAL_I2C_CLEAR_ADDRFLAG(hi2c);
    
    while(hi2c->XferSize > 0U){
      /* Wait until TXE flag is set */
      if(I2C_WaitOnTXEFlagUntilTimeout(hi2c, Timeout, tickstart) != HAL_OK){
        if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
          /* Generate Stop */
          hi2c->Instance->CR1 |= I2C_CR1_STOP;
          return HAL_ERROR;
        }else{
          return HAL_TIMEOUT;
        }
      }
      /* Write data to DR */
      hi2c->Instance->DR = (*hi2c->pBuffPtr++);
      hi2c->XferCount--;
      hi2c->XferSize--;
    
      if((__HAL_I2C_GET_FLAG(hi2c, I2C_FLAG_BTF) == SET) 
         && (hi2c->XferSize != 0U)){
        /* Write data to DR */
        hi2c->Instance->DR = (*hi2c->pBuffPtr++);
        hi2c->XferCount--;
        hi2c->XferSize--;
      }
      /* Wait until BTF flag is set */
      if(I2C_WaitOnBTFFlagUntilTimeout(hi2c, Timeout, tickstart) != HAL_OK){
          
        if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
          /* Generate Stop */
          hi2c->Instance->CR1 |= I2C_CR1_STOP;
          return HAL_ERROR;
        }else{
          return HAL_TIMEOUT;
        }
      }
    }
    
    /* Generate Stop */
    hi2c->Instance->CR1 |= I2C_CR1_STOP;
    
    hi2c->State = HAL_I2C_STATE_READY;
    hi2c->Mode = HAL_I2C_MODE_NONE;
    
    /* Process Unlocked */
    __HAL_UNLOCK(hi2c);
    
    return HAL_OK;

  }else{
    return HAL_BUSY;
  }
}
```

**引用**：

[CSDN](https://blog.csdn.net/u010632165/article/details/109188507?spm=1001.2014.3001.5502)

[百度百科](https://baike.baidu.com/item/I2C%E6%80%BB%E7%BA%BF?fromtitle=I2C&fromid=1727975)

































