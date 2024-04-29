

1. **为什么要使用Kafka？**
   - Kafka是一个分布式发布-订阅消息系统，适用于处理高吞吐量的流式数据。它主要用于缓冲和削峰、解耦和扩展性、冗余、健壮性以及异步通信。

2. **Kafka中的ISR和AR代表什么？**
   - ISR代表In-Sync Replicas，即副本同步队列，AR代表Assigned Replicas，即所有副本。ISR是活跃副本的集合，由leader维护，而AR包括ISR和OSR（Out-of-Sync Replicas）。

3. **Kafka中的broker是做什么的？**
   - Broker是消息的代理，Producer将消息写入Broker中的Topic，Consumer从Broker中拉取Topic的消息进行业务处理。

4. **Kafka中的Zookeeper起到什么作用？**
   - ZooKeeper是一个分布式协调组件，早期Kafka版本使用ZK存储元信息，如消费状态、group管理等。新版本减少了对ZK的依赖，但broker依然依赖于ZK进行如选举controller和检测broker存活等操作。

5. **Kafka的Message结构是怎样的？**
   - Kafka的Message由固定长度的header和变长的消息体body组成，header包含magic和CRC32，body包含具体的key/value消息。

6. **Kafka中consumer group是什么概念？**
   - Consumer group是Kafka实现单播和广播消息模型的手段。同一个topic的数据可以广播给不同的group，但同一个group中的worker只有一个能拿到数据。
- Consumer Group是一组共享一个Group ID的Consumer，它们协调消费订阅Topic的所有Partition。每个Partition只能由同一个Consumer Group内的一个Consumer消费。
   
7. **Kafka的消息确认机制有哪几种？**
   - Kafka的消息确认机制有三种，分别代表不同的数据持久性保证：acks=0表示不进行消息接收确认；acks=1表示Leader接收成功时确认；acks=-1表示Leader和所有Follower都接收成功时确认。

8. **Kafka生产者运行流程是怎样的？**
   - 生产者将消息封装成ProducerRecord对象，进行序列化处理，分区后放入缓存区，Sender线程将批次发送到服务端。

**Kafka中的消费者是如何工作的？**

- Kafka中的消费者通过订阅一个或多个Topic的Partition来接收消息，消费者组内的不同消费者可以并行处理消息。

10. **Kafka为什么要把消息分区？**
    - 消息分区便于在集群中扩展，提高并发处理能力，因为可以以Partition为单位进行读写。

2. **Kafka中的unclean.leader.election配置代表什么？**
   - 当设置为true时，允许非ISR集合的broker参与选举，可能会导致数据丢失。设置为false时，会等待旧leader恢复正常，降低了可用性。

3. **Kafka可接收的消息最大默认多少字节，如何修改？**
   - Kafka默认可接收的最大消息大小为1000000字节。可以通过修改Broker配置参数`Message.max.bytes`来调整。

4. **Kafka Producer API的作用是什么？**
   - Kafka Producer API允许应用程序将记录流发布到一个或多个Kafka主题。

1. **Kafka中的日志存储是如何工作的？**
   - Kafka将消息存储在磁盘上的日志中，这些日志持久化存储于Broker服务器。【3】
2. **Kafka中的消费者偏移量是什么？**
   - 消费者偏移量是消费者在Topic分区中读取位置的记录，它允许消费者从上次消费的位置继续读取消息。【3】

1. **Kafka中的ISR（In-Sync Replicas）列表是什么？**
   - ISR列表是一组副本，它们与Leader保持同步，并且可以成为新的Leader。【1】
2. **Kafka中的压缩和序列化是如何工作的？**
   - Kafka支持消息的压缩，以减少网络传输的大小。同时，Kafka使用序列化器将对象转换为字节，以便发送和存储