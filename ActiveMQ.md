# ActiveMQ

## 一、JMS简介

​	JMS即[Java消息服务](https://baike.baidu.com/item/Java%E6%B6%88%E6%81%AF%E6%9C%8D%E5%8A%A1)（Java Message Service）应用程序接口，是一个[Java平台](https://baike.baidu.com/item/Java%E5%B9%B3%E5%8F%B0)中关于面向[消息中间件](https://baike.baidu.com/item/%E6%B6%88%E6%81%AF%E4%B8%AD%E9%97%B4%E4%BB%B6/5899771)（MOM）的[API](https://baike.baidu.com/item/API/10154)，用于在两个应用程序之间，或[分布式系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F/4905336)中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。

​	==JMS是一种与厂商无关的 API==，用来访问收发系统消息，它类似于[JDBC](https://baike.baidu.com/item/JDBC)(Java  Database Connectivity)。这里，JDBC 是可以用来访问许多不同关系数据库的 API，而 JMS  则提供同样与厂商无关的访问方法，以访问消息收发服务。许多厂商都支持 JMS，包括 IBM 的 MQSeries、BEA的 Weblogic  JMS service和 Progress 的 SonicMQ。 JMS 使您能够通过消息收发服务（有时称为消息中介程序或路由器）从一个 JMS  客户机向另一个 JMS客户机发送消息。消息是 JMS  中的一种类型对象，由两部分组成：报头和消息主体。报头由路由信息以及有关该消息的元数据组成。消息主体则携带着应用程序的数据或有效负载。根据有效负载的类型来划分，可以将消息分为几种类型，它们分别携带：简单文本(TextMessage)、可序列化的对象  (ObjectMessage)、属性集合 (MapMessage)、字节流 (BytesMessage)、原始值流  (StreamMessage)，还有无有效负载的消息 (Message)。

## 二、ActiveMQ简介

​	ActiveMQ 是==Apache出品==，最流行的，能力强劲的开源消息总线。ActiveMQ 是一个完全支持JMS1.1和J2EE 1.4规范的 
JMS Provider实现，尽管JMS规范出台已经是很久的事情了，但是JMS在当今的J2EE应用中间仍然扮演着特殊的地位。

>⒈ ==多种语言和协议编写客户端==。语言: Java,C,C++,C#,[Ruby](https://baike.baidu.com/item/Ruby),[Perl](https://baike.baidu.com/item/Perl),[Python](https://baike.baidu.com/item/Python),[PHP](https://baike.baidu.com/item/PHP)。应用协议： OpenWire,Stomp REST,WS Notification,XMPP,[AMQP](https://baike.baidu.com/item/AMQP)
>
>⒉ 完全支持[JMS](https://baike.baidu.com/item/JMS)1.1和[J2EE](https://baike.baidu.com/item/J2EE) 1.4规范 （持久化，XA消息，[事务](https://baike.baidu.com/item/%E4%BA%8B%E5%8A%A1))
>
>⒊==对Spring的支持==，ActiveMQ可以很容易内嵌到使用Spring的系统里面去，而且也支持Spring2.0的特性
>
>⒋ 通过了常见J2EE[服务器](https://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E5%99%A8)（如 Geronimo,JBoss 4,GlassFish,WebLogic)的测试，其中通过JCA 1.5 resource adaptors的配置，可以让ActiveMQ可以自动的部署到任何兼容J2EE 1.4 商业服务器上
>
>⒌==支持多种传送协议==：in-VM,TCP,SSL,NIO,UDP,JGroups,JXTA
>
>⒍ 支持通过JDBC和journal提供高速的消息持久化
>
>⒎ ==从设计上保证了高性能的集群==，客户端-[服务器](https://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E5%99%A8)，点对点
>
>⒏ 支持[Ajax](https://baike.baidu.com/item/Ajax)
>
>⒐ 支持与Axis的整合
>
>⒑ 可以很容易的调用内嵌JMS provider，进行测试

### 关键词

> **Destination**
>
> ​	JMS Provider（消息中间件）负责维护，用于对Message进行管理的对象。MessageProducer需要指定Destination才能发送消息，MessgaeConsumer需要指定Destination才能接收消息
>
> **Producer**
>
> ​	消息生成者，负责发送Message到Destination；应用接口为MessageProducer。在JMS规范中，所有的标准定义都在javax.jms包中
>
> **Consumer**（**Receiver**）
>
> ​	负责从Destination中消费（处理|监听|订阅）Message。应用接口为MessageConsumer
>
> **Message**
>
> ​	消息封装一次同学的内容。常见类型有：StreamMessage、BytesMessage、TestMessage、ObjectMessage、MapMessage
>
> **ConnectionFactory**
>
> ​	用于创建连接的工厂类型
>
> **Connection**
>
> ​	用于建立访问ActiveMQ连接的类型，由连接工厂创建
>
> **Session**
>
> ​	一次持久有效有状态的访问，由连接创建。是具体操作消息的基础支撑
>
> **Queue&Topic**
>
> ​	队列Destination与主题Destination，都是Destination的子接口
>
> ​	Queue：队列中的消息，默认只能由唯一的一个消费者处理。一旦处理消息删除
>
> ​	Topic：主题中的消息，会发送给所有的消费者同时处理，只有在消费可以重复处理的业务场景中使用
>
> **PTP**
>
> ​	Point to Point消息模型，基于Queue实现的消息处理方式
>
> **PUB&SUB**
>
> ​	Publish&Subscribe，发布/订阅模型，基于Topic实现的消息处理方式

## 三、安装

​	解压即可

​	`activemq start/stop`启动/关闭activemq

​	`8161`默认后台端口（`jetty.xml`配置），`/admin`访问admin权限，默认用户配置在`jetty-realm.properties`文件中

```shell
lov@lov:/opt/apache-activemq-5.15.8/bin$ ./activemq start
INFO: Loading '/opt/apache-activemq-5.15.8//bin/env'
INFO: Using java '/usr/lib/jvm/jdk1.8.0_191/bin/java'
INFO: Starting - inspect logfiles specified in logging.properties and log4j.properties to get details
INFO: pidfile created : '/opt/apache-activemq-5.15.8//data/activemq.pid' (pid '7769')
lov@lov:/opt/apache-activemq-5.15.8/bin$ jps
7769 activemq.jar
7817 Jps
lov@lov:/opt/apache-activemq-5.15.8/bin$ ./activemq stop
```

![1549790481587](img/1549790455720.png)

## 四、应用

### PTP处理模式

​	消息生产者生产消息发送到queue中，然后消息消费者从queue】中获取 并且消费消息。消息被消费后，queue中就不再存储，所以消息消费者不可能消费到已经被消费的消息

​	Queue支持存在多个消费者，但是对于一个消息而言，只会有一个消费者可以消费，其他的则不能消费该消息。当消费者不存在时，消息会一直保存直到有消费消息

#### 主动消费

TextProducer

```java
package com.lov.ptp_1;

import javax.jms.Connection;
import javax.jms.ConnectionFactory;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageProducer;
import javax.jms.Session;

import org.apache.activemq.ActiveMQConnectionFactory;

public class TextProducer {

	/*
	 * 发送消息到ActiveMQ中，具体的消息内容为参数信息
	 * 使用的接口类型都是javax.jms下的类型
	 */
	public void sendTextMessage(String data) {
		
		ConnectionFactory factory = null;
		Connection connection = null;
		Destination destination = null;
		Session session = null;
		MessageProducer producer = null;
		Message message = null;
			
		try {
			/*
			 * 创建连接工厂，连接ActiveMQ服务的连接工厂
			 * 构造方法参数：用户名，密码，连接地址
			 * 无参构造：有默认的链接地址，本地连接
			 * 单参数构造：无验证模式，没有用户的认证
			 * 三参数构造：右验证与指定地址，默认端口为61616，在ActiveMQ的conf/activemq.xml配置文件中查看
			 */
			factory = new ActiveMQConnectionFactory("guest","guest","tcp://localhost:61616");
			
			/*
			 * 通过工厂，创建连接对象
			 * 创建连接的方法右重载
			 * 可以在创建连接工厂时，只传递连续地址，不传递用户信息
			 */
			connection = factory.createConnection();
			/*
			 * 建议启动连接，消息的发送者不是必须启动连接，消息的消费者必须启动连接
			 * producer在发送消息时，会检查是否启动连接，如果没有，自动启动
			 * 如果有特殊的配置，建议配置完毕后再启动连接
			 */
			connection.start();
			
			/*
			 * 通过connection对象，创建session对象，必须绑定目的地
			 * 
			 * 创建session时，必须传递两个参数，分别代表支持事务和如何确认消息处理
			 * transacted：是否支持事务
			 * 		true - 支持事务，第二个参数对producer来书默认无效（对于Producer），建议传递的数据为 Session.CLIENT_ACKNOWLEDGE
			 * 		false - 不支持事务，常用参数，第二个参数必须传递，且必须有效
			 * acknowledgeMode：如何确认消息的处理，使用确认机制实现
			 * 		AUTO_ACKNOWLEDGE - 自动确认消息，消息的消费者处理消息后，自动确认，常用，商业开发不推荐
			 * 		CLIENT_ACKNOWLEDGE - 客户端手动确认，消息的消费者处理后，必须手动确认
			 * 		DUPS_OK_ACKNOWLEDGE - 有副本的客户端手动确认
			 * 			一个消息可以多次处理
			 * 			可以降低Session的消耗，在可以容忍重复消息时使用（不推荐）
			 */
			session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
			//参数时Destination名称，时Destination的唯一标记
			destination = session.createQueue("first-mq");
			
			/*
			 * 通过session对象，创建消息的发送者Producer
			 * 发送的消息一定到指定的Destination中
			 * 创建Producer时，可以不提供Destination，在发送时再指定Destination
			 */
			producer = session.createProducer(destination);
			//作为具体数据内容的载体
			message = session.createTextMessage(data);
			
			//使用Producer，发送消息到ActiveMQ中的Destination，如果发送失败，抛出异常
			producer.send(message);
			System.out.println("send success");
		} catch (JMSException e) {
			
			e.printStackTrace();
		}finally {
			//回收资源
			if (producer != null) {
				try {
					producer.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
			if (session != null) {
				try {
					session.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
			if (connection != null) {
				try {
					connection.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
		}
	}
	
	public static void main(String[] args) {
		
		TextProducer producer = new TextProducer();
		producer.sendTextMessage("test text");
	}
	
}
```

![1549802259643](img/1549802259643.png)

TextConsumer

```java
package com.lov.ptp_1;

import javax.jms.Connection;
import javax.jms.ConnectionFactory;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageConsumer;
import javax.jms.Session;
import javax.jms.TextMessage;

import org.apache.activemq.ActiveMQConnectionFactory;

public class TextConsumer {

	public String receiveTextMessage() {
		
		String resultCode = "";
		ConnectionFactory factory = null;
		Connection connection = null;
		Session session = null;
		Destination destination =null;
		//用于接收消息的对象
		MessageConsumer consumer = null;
		Message message = null;
		
		try {
			factory = new ActiveMQConnectionFactory("admin", "admin", "tcp://localhost:61616");
			connection = factory.createConnection();
			//消息的消费者必须启动连接，否则无法处理消息
			connection.start();
			session  = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
			destination = session.createQueue("first-mq");
			//创建消息消费者对象，在指定的Destination中获取消息
			consumer = session.createConsumer(destination);
			//receive是主动获取消息的方法，执行一次，获取一次。开发少用
			message = consumer.receive();
			
			resultCode = ((TextMessage)message).getText();
		} catch (JMSException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			//回收资源
			if (consumer != null) {
				try {
					consumer.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
			if (session != null) {
				try {
					session.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
			if (connection != null) {
				try {
					connection.close();
				} catch (JMSException e) {
					
					e.printStackTrace();
				}
			}
		}
		
		return resultCode;
	}
	
	public static void main(String[] args) {
		
		TextConsumer consumer = new TextConsumer();
		String receiveTextMessage = consumer.receiveTextMessage();
		
		System.out.println(receiveTextMessage);
		
	}
	
}
```

![](img/DeepinScrot-4048.png)

#### 观察者消费

ConsumerListener

```java
/*
 * 使用监听器的方式，实现消息的处理
 */
public class ConsumerListener {

	
	/**
	 * 处理消息
	 */
	public void consumMessage(){
		
		ConnectionFactory factory =null;
		Connection connection = null;
		Session session =null;
		Destination destination = null;
		MessageConsumer consumer = null;
		
		try {
			factory = new ActiveMQConnectionFactory("admin", "admin", "tcp://localhost:61616");
			connection = factory.createConnection();
			
			connection.start();
			session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
			destination = session.createQueue("test-listener");
			consumer = session.createConsumer(destination);
			
			//注册监听器，注册成功后，队列中的消息变化会自动触发监听器代码。接收消息并处理
			consumer.setMessageListener(message->{
				/*
				 * 监听器一旦注册，永久有效
				 * 永久-consumer线程不关闭
				 * 处理消息的方式：只要有消息未处理，自动调用该方法，处理消息
				 * 监听器可以注册多个，注册多个监听器，相当于集群
				 * ActiveMQ自动循环调用多个监听器，处理队列中的消息，实现并行处理
				 */
				
				Object data;
				try {
					//acknowledge方法，确认方法。代表consumer已经收到消息，确认后，MQ删除对应的消息
					message.acknowledge();
					ObjectMessage om = (ObjectMessage) message;
					data = om.getObject();
					System.out.println(data);
				} catch (JMSException e) {
					e.printStackTrace();
				}
			});
			
			//阻塞当前代码，保证listener代码未结束。如果代码结束，监听器自动关闭
			System.in.read();
		} catch (JMSException e) {
			
			e.printStackTrace();
		} catch (IOException e) {

			e.printStackTrace();
		}finally {
			if (consumer != null) {
				try {
					consumer.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (session != null) {
				try {
					session.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (connection != null) {
				try {
					connection.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
		
		
	}
	
	public static void main(String[] args) {
		
		new ConsumerListener().consumMessage();
		
	}
	
}
```

ObjectProducer

```java
public class ObjectProducer {

	public void sendMessage(){
		ConnectionFactory factory = null;
		Connection connection = null;
		Session session = null;
		Destination destination = null;
		MessageProducer producer = null;
		Message message = null;
		
		try {
			factory = new ActiveMQConnectionFactory("admin", "admin", "tcp://localhost:61616");
			connection = factory.createConnection();
			connection.start();
			session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
			destination = session.createQueue("test-listener");
			producer = session.createProducer(destination);
			connection.start();
			
			for (int i = 0; i < 100; i++) {
				Integer data = i;
				message = session.createObjectMessage(data);
				producer.send(message);
			}
		} catch (JMSException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			if (producer != null) {
				try {
					producer.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (session != null) {
				try {
					session.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (connection != null) {
				try {
					connection.close();
				} catch (JMSException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
		
	}
	
	public static void main(String[] args) {
		
		new ObjectProducer().sendMessage();
	}
	
}

```

​	开启两个consumerlistener监听器，producer发送0-99的数，监听器获取处理，查看两个监听器的console输出

![](img/p2p.png)

### PUB&SUB处理模式

## 五、安全认证

## 六、持久化