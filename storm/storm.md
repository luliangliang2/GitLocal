### strom编写手册
- 数据的流入是spout  
 - spout继承BaseRichSpout
- 数据的留出是bolt  
 - bolt集成 BaseBasicBolt
- 构建topology的是主类  

### 构建数据流入和数据留出
- 多个spout给一个bolt处理  
  ```java
    topologyBuilder.setSpout("CountStrSpout", new CountStrSpout(), 1);
    topologyBuilder.setSpout("CountStrSpout2", new CountStrSpout2(), 1);
    // 设置数据处理节点并分配并发数。指定该节点接收喷发节点的策略为随机方式。
    topologyBuilder.setBolt("CountStrBolt", new CountStrBolt(), 3)
    .shuffleGrouping("CountStrSpout").shuffleGrouping("CountStrSpout2");
  ```

-  一个spout给一个bolt处理  
 ```java
 topologyBuilder.setBolt("CountStrBolt2", new CountStrBolt2(), 3).shuffleGrouping("CountStrSpout");
 ```
- 添加数据的id字段来构建复杂的topology
```java
public void nextTuple() {
    try {
        String msg = info[random.nextInt(11)];
        // 调用发射方法
        collector.emit(new Values(msg,msg));
        // 模拟等待100ms
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
/**
 * 定义字段id，该id在简单模式下没有用处，但在按照字段分组的模式下有很大的用处。
 * 该declarer变量有很大作用，我们还可以调用declarer.declareStream();来定义stramId，该id可以用来定义更加复杂的流拓扑结构
 */
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
      declarer.declare(new Fields("test","sb")); //collector.emit(new Values(msg));参数要对应
  }
  可以通过id字段来获取想要的数据
if(fields.contains("sb")) {
	String msg = input.getStringByField("sb");
	System.out.println("CountStrBolt  >>sb  >>"+msg);  
}
```
