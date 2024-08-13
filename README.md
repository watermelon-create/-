# 好运驾驶帮手微服务项目

技术：Spring、 SpringMVC、Mybatis、Mybatis-plus、SpringCloud、Redis、Redisson、MySQL、Mongodb、Rabbitmq、Minor、Seata、XXL-J0B、Drools、Natapp

## 独立完成的模块

登录：乘客端登录注册并使用Redis缓存token利用aop实现免登录，司机端使用腾讯云注册认证并存储司机认证信息

下单：乘客选定代驾起点终点，使用规则引擎Drools根据订单的距离编写规则得到预估金额，用Redis缓存乘客当前位置，并使用Redis的Geo寻找附近司机，推送订单给司机；使用XXL-JOB定时搜索附近司机

接单：人脸识别的司机抢单生成到达乘客位置的路线，司机实时位置信息缓存到Redis中，乘客和司机位置司乘同显，到达代驾位置（1KM以内）进行服务；司机抢单使用Redisson作为分布式锁防止重复抢单

服务：司机按照订单路线为乘客服务，位置实时存储到mongodb中，并将过程中的语音等文件存储在Minor中，将乘客送到终点1KM范围；使用CompletableFuture异步编排优化远程调用微服务生成订单的过程，司机推送订单

支付：通过Natapp内网穿透调用本地微服务向Rabbitmq发送交易等信息，后续开启线程异步更新订单、账单

其他：使用Redisson延迟队列实现超时订单自动取消；优惠卷的领取防止并发问题；生成的订单根据现金券或者折扣券更新订单总金额
