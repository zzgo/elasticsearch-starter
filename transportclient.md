## TransportClient

这么拿到TransportClient对象

```java
// 设置集群名称
// 自动嗅探整个集群的状态，把集群中其它机器的ip地址加到客户端中
// ES 2.2.0
// Settings settings = Settings.settingsBuilder().put("cluster.name", CLUSTER_NAME).put("tclient.transport.sniff", true).build();
// ES 5.2.x
// sniff=true代表自动把集群下的机器添加到列表中CLUSTER_NAME=这个与你es启动时配置集群名称一致
Settings settings = Settings.builder().put("cluster.name", CLUSTER_NAME).put("client.transport.sniff", true).build();
// ES 2.2.0
// transportClient = TransportClient.builder().settings(settings).build();
// ES 5.2.x
transportClient = new PreBuiltTransportClient(settings);
//这里的host格式，单个集群：192.168.0.1:9300 , 多个集群：192.168.0.1:9300,192.168.0.2:9300
String[] nodes = host.split(",");
for (String node : nodes) {
    // 跳过为空的node（当开头、结尾有逗号或多个连续逗号时会出现空node）
    if (node.length() > 0) {
        String[] hostPort = node.split(":");
        ransportClient
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName(hostPort[0]), Integer.parseInt(hostPort[1])));
    }
}
```

因为进行一次es链接是非常消耗性能的

所以将创建的好的TransportClient进行保存

```java
private static Map<String, TransportClient> transportClientMap = new HashMap<String, TransportClient>();


public void init(){
    TransportClient transportClient = null;
    try {
        transportClient = transportClientMap.get(host);
        if (transportClient == null) {
            synchronized (transportClientMap) {
                // 设置集群名称
                // 自动嗅探整个集群的状态，把集群中其它机器的ip地址加到客户端中
                // ES 2.2.0
                // Settings settings = Settings.settingsBuilder().put("cluster.name", CLUSTER_NAME).put("tclient.transport.sniff", true).build();
                // ES 5.2.x
                // sniff=true代表自动把集群下的机器添加到列表中
                Settings settings = Settings.builder().put("cluster.name", CLUSTER_NAME).put("client.transport.sniff", true).build();
                // ES 2.2.0
                // transportClient = TransportClient.builder().settings(settings).build();
                // ES 5.2.x
                transportClient = new PreBuiltTransportClient(settings);
                //这里的host格式，单个集群：192.168.0.1:9300 , 多个集群：192.168.0.1:9300,192.168.0.2:9300
                String[] nodes = host.split(",");
                for (String node : nodes) {
                    // 跳过为空的node（当开头、结尾有逗号或多个连续逗号时会出现空node）
                    if (node.length() > 0) {
                        String[] hostPort = node.split(":");
                        ransportClient
                        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName(hostPort[0]), Integer.parseInt(hostPort[1])));
                    }
                }
            }
            transportClientMap.put(host, transportClient);
        }
    } catch (Exception e) {
        // TODO:改成日志
        e.printStackTrace();
    }
}

```



