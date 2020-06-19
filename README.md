# 智慧物流质询系统

## 实验环境

后端框架：SpringBoot\
IDE: IDEA 2020.1, PyCharm\
框架语言：Java Version “14.0.1”\
日志框架：Slf4j + logback\
数据库：实验二三整合的sqlparser.exe + txt存储格式\
（sqlparser.exe源语言：python 3.7）

## 系统结构

![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0617/200035_8aa35e8a_6515767.png)


## 前端结构
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0618/192340_2a655cd5_6515767.png)


# 智慧物流协议

## 1. 系统功能

### 1.1 用户界面

- **提交订单功能**

  `cn.uestc.software.intelligentlogistics.controller.OrderController#submitOrder`

  订单信息包括：

  ```java
  //寄件信息（用户填写）
  private String sender;
  private Integer origin;         //起始城市
  private String originDetail;
  private String senderTele;
  
  //收件信息（用户填写）
  private String receiver;
  private Integer destination;     //目的城市
  private String destinationDetail;
  private String receiverTele;
  
  //物品属性（用户填写）
  private Double weight;          //重量
  private Integer priority;       //优先级（普通0，加急1）
  ```

  

- **根据订单号查询订单功能**

  `cn.uestc.software.intelligentlogistics.controller.OrderController.queryOrderById`\

  根据订单号(trackingId)就可以查询该包裹物流信息
  
- **根据用户名查询订单功能**

    `cn.uestc.software.intelligentlogistics.controller.OrderController.queryOrderByName`\
    根据用户名(name)就可以查询该用户的寄件和收件
  
- **修改订单信息（目的城市）**\
    `cn.uestc.software.intelligentlogistics.controller.OrderController.modifyOrder`

- **删除订单**\
    `cn.uestc.software.intelligentlogistics.controller.OrderController.deleteOrder`

### 1.2 管理员界面

- **发货功能（获取订单列表）**

  `cn.uestc.software.intelligentlogistics.controller.AdminController#deliverGoods`

  根据传入的发货城市信息，获得已排序的包裹列表，根据顺序发货

- **更新包裹位置信息**

  `cn.uestc.software.intelligentlogistics.controller.AdminController#updateLocation`

  包裹每到达一个城市，管理员（快递员）便更新其当前位置

- **更新最短路径**

  `cn.uestc.software.intelligentlogistics.controller.AdminController#updatePath`

  生成最短路径表至数据库中

### 1.3 登录/注册
- **登录**
   `cn.uestc.software.intelligentlogistics.controller.LoginController.login`
   
- **注册**
    `cn.uestc.software.intelligentlogistics.controller.LoginController.register`

## 2. 快递价格说明

总费用=首重$\ast$首重价格+续重$\ast$续重价格

> 1. 首重价格=12+运输距离加成（每超过500公里，增加1元）
>
> **若是加急件，首重价格在此基础上$\ast$1.5
>
> 2. 续重价格=首重价格/2

> ​		首重即1kg，不及1kg记为1kg；续重=总重量-首重



## 3. 物流方案

### 3.1 普通

- 具体实现：运输速度100 km/h

- 发货每天两次

  > 第一次是中午12点，发前一天下午5点之后到当天12点之前收到的快件；

  > 第二次是下午5点，发当天12点至下午五点之前收到的快件。

  > 具体发货时间根据是否加急和下单时间排序

- 派送

  > 遵守早8点-晚7点的派送时间

### 3.2 加急（航空物流）

- 具体实现：运输速度600 km/h

- 发货每天两次

  > 第一次是上午10点，发前一天下午3点之后到当天12点之前收到的快件；

  > 第二次是下午3点， 发当天10点至下午3点之前收到的快件。

  > 具体发货时间根据是否加急和下单时间排序

- 派送

  > 遵守早8点-晚7点的派送时间





