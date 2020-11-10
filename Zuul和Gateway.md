

## 13,GateWay



![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的1.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的2.png)

**gateway之所以性能号,因为底层使用WebFlux,而webFlux底层使用netty通信(NIO)**



![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的3.png)



### GateWay的特性:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的4.png)



### GateWay与zuul的区别:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的5.png)



### zuul1.x的模型:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的6.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的7.png)





### 什么是webflux:

**是一个非阻塞的web框架,类似springmvc这样的**

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的8.png)



### GateWay的一些概念:

#### 1,路由:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的9.png)

就是根据某些规则,将请求发送到指定服务上



#### 2,断言:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的10.png)

就是判断,如果符合条件就是xxxx,反之yyyy



#### 3,过滤:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的11.png)

​	**路由前后,过滤请求**





### GateWay的工作原理:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的12.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的13.png)





### 使用GateWay:

想要新建一个GateWay的项目

名字: 	cloud_gateway_9527

#### 1,pom

#### 2,配置文件

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的14.png)

#### 3,主启动类

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的15.png)

#### 4,针对pay模块,设置路由:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的16.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的18.png)

**==修改GateWay模块(9527)的配置文件==:**

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的17.png)

这里表示,

​			当访问localhost:9527/payment/get/1时,     

​			路由到localhost:8001/payment/get/1



#### 5,开始测试

**启动7001,8001,9527**

```java
如果启动GateWay报错
  	可能是GateWay模块引入了web和监控的starter依赖,需要移除
```

访问:

​		localhost:9527/payment/get/1

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的19.png)







#### 6,GateWay的网关配置,

​		**GateWay的网关配置,除了支持配置文件,还支持硬编码方式**

#### 7使用硬编码配置GateWay:

##### 创建配置类:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的20.png)

#### 8,然后重启服务即可

 



### 重构:

上面的配置虽然首先了网关,但是是在配置文件中写死了要路由的地址

现在需要修改,不指定地址,而是根据微服务名字进行路由,我们可以在注册中心获取某组微服务的地址

需要:

​		1个eureka,2个pay模块

#### 修改GateWay模块的配置文件:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的21.png)



#### 然后就可以启动微服务.测试







### Pridicate断言:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的24.png)

**我们之前在配置文件中配置了断言:**

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的22.png)

**这个断言表示,如果外部访问路径是指定路径,就路由到指定微服务上**

可以看到,这里有一个Path,这个是断言的一种,==断言的类型==:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的23.png)



```java
After:
		可以指定,只有在指定时间后,才可以路由到指定微服务
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的26.png)

​				这里表示,只有在==2020年的2月21的15点51分37秒==之后,访问==才可以路由==

​				在此之前的访问,都会报404

如何获取当前时区?**

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的25.png)



```java
before:
		与after类似,他说在指定时间之前的才可以访问
between:
		需要指定两个时间,在他们之间的时间才可以访问
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的27.png)





```java
cookie:
		只有包含某些指定cookie(key,value),的请求才可以路由
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的28.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的29.png)



```java
Header:
		只有包含指定请求头的请求,才可以路由
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的31.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的32.png)

测试:
![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的33.png)







```java
host:
		只有指定主机的才可以访问,
		比如我们当前的网站的域名是www.aa.com
    那么这里就可以设置,只有用户是www.aa.com的请求,才进行路由
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的34.png)

![gateway的34](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的35.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的36.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的37.png)

可以看到,如果带了域名访问,就可以,但是直接访问ip地址.就报错了







```java
method:
		只有指定请求才可以路由,比如get请求...
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的38.png)

```java
path:
		只有访问指定路径,才进行路由
     比如访问,/abc才路由
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的39.png)



```java
Query:
		必须带有请求参数才可以访问
```

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的40.png)







### Filter过滤器:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的41.png)



#### 生命周期:

**在请求进入路由之前,和处理请求完成,再次到达路由之前**



#### 种类:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的42.png)

GateWayFilter,单一的过滤器

**与断言类似,比如闲置,请求头,只有特定的请求头才放行,反之就过滤**:

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的43.png)

GlobalFilter,全局过滤器:





#### **自定义过滤器:**

实现两个接口

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的44.png)

​	**然后启动服务,即可,因为过滤器通过@COmponet已经加入到容器了**

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的46.png)

![](/Users/zhangerchi/Desktop/SpringBootWeb/springcloud/./图片/gateway的45.png)









