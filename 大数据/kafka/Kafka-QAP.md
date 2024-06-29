Q: kafka 的 batch_size 和 linger.ms 实际多少才能发挥最佳性能

这个问题属于想过还没实践过，初步的想法

- 最佳状态
- 最好linger.ms 刚到，batch-size 也满了
- 看生产者和消费者的速率之差，kafka的topic大小刚好在安全范围内。

做法

JMX 监控 `batch-size-avg` 和 `batch-size-max` 指标

计算平均每条消息大小，生产者消费者的速率曲线，kafka可用的存储。

