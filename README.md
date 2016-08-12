# RPC-Theory-Choice-exerice



在rpc 通过反射拿到代理对象 调用对应代理对象的方法 基于反射性能的考虑
    1 jdk 反射 （高版本jdk）
    2 CGlib 动态代理
    3 ASM 动态代理
    4 JAVAASSISt
 jdk CGlib ASM  动态代理时通过字节码 生成代理
     ASM 性能最快 但是开发控制的难度和其他几种方式编程难度不在一个级别
     CGlib 动态代理方式编码操作容易同时他生成的字节码较小。
网上同样给出对应的性能比较。因此采用Cglib 中的Proxy.newInstanc(class<?>,class[],Involtation);

   通过代理类（proxy） 拿到代理对象 然后代理对象调用对应的方法 这时触发代理对象（继承Invocation） 的invoke(class<?> ,object ,object[]) 方法




rpc 服务的注册发现 
    rpc 服务的注册发现比较方便的了采用 redis zookeeper 对数据的操作都是原子的 redis 需要额外的一些辅助（元数据写入redis 相关数据的发现注销操作参考dubbo源码）。 目前采用 zookeeper

    采用zookeeper 主要利用他的 watch 机制 client zookeeper  可以异步通信（zookeeper 中的znode 数据 发生改变 会自动通知 他的client 连接程序） zookeeper相关知识 自行恶补
    /data/通信的IP:端口（TCP:port ）或者 /data/interface(接口)/(providers|routers|consumers) 类似dubbo 树结构
服务与spring 结合
    为了方便管理，服务的发现管理（把对应的服务接口通过spring 的包扫描 或者 bean 标签）有助于通过反射获取定义的接口
     server 端 通过ApplicationContextAware 中的 setApplicationContext() 方法 获取自己定义的注解的相关类和对象接口名与服务对象之间的映射关系。通过 InitializingBean 接口的afterPropertiesSet方法启动 TCP
     client 端 通过自定义的标签 （dubbo dubbo:consumer 类似的标签） 通过通过ApplicationContextAware 中的 setApplicationContext() 方法 获取自己定义的标签





RPC  中核心思想是动态代理（和 单机动态代理 不同 ，他是多台机器直接通信，访问 server 节点，）
动态代理 需要通过TCP 把 客户端调用的类 参数，结果 等封装成对象 
通信部分 可以使用 netty 

具体详解图解


序列化部分  protobuf  fastjson avro kyro  java nio 序列化 源码的解读

dubbo 把各种类型数据通过压缩，在发送包之前打上对应的标签，在client 反序列化通过特定标签进行数据的解析
fastjson avro kyro 是在 InputStream OutputStream 流上进行封装，把支持的数据类型 进行按照特定编码序列化，在writeObject 之前进行数据的序列化 在readObject 对数据进行反序列化。
JAVA NIO  + 序列化   ：java 序列化 通过继承 Serivalize 和实现 Externalizable接口    Serivalize 实现 write，read 自定义序列化 在ObjectInputStream ObjectOutputStream 时会自动调用对象的write read 方法 ，如果没有 会自动调用流中对应的write read 方法 反序列话 Externalizable 类 实现 writeObject readObject 

protobuf 序列化 原理相关的源码没有研究 可以使用 protobufStaff 利用他对对象进行protobuf 序列化

RPC代码研究 ： motan  dubbo   https://github.com/Micheal-Bigmac/rpc_learn  hadoop 通信  相关源码的解读。
   
