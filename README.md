Build the EMQ broker
-------------

1. clone emq-relx project
```	
git clone https://github.com/emqx/emqx.git
```
2. Add DEPS of the plugin in the Makefile
```
DEPS += emqx_kafka_bridge
dep_emqx_kafka_bridge = git https://github.com/araditc/emqx_kafka_bridge.git release
```
3. Add load plugin in relx.config
```
{emqx_kafka_bridge, load},
 ```
4. Build
```
cd emq-relx && make
```  
Configuration
----------------------
You will have to edit the configurations of the bridge to set the kafka Ip address and port.

Edit the file /emqx_kafka_bridge/etc/emqx_kafka_bridge.config
```
 emqx.kafka.bridge.broker = 172.19.16.67:19092, 172.19.16.68:19092, 172.19.16.69:19092
 emqx.kafka.bridge.partition = 10
 emqx.kafka.bridge.client.flag = auto_start_producers:true, allow_topic_auto_creation:false, query_api_versions:false
 emqx.kafka.bridge.client.integer = reconnect_cool_down_seconds:10
 emqx.kafka.bridge.regex = ^(client|device|paas)/products/(\\S+)/devices/(\\S+)/(command)(/\\S+)*$
 emqx.kafka.bridge.topic = device:saas_device_downstream, client:saas_client_downstream, paas: paas_sqdata_upstream
 emqx.kafka.bridge.hook.client.connected.topic     = mqtt_client_connected
 emqx.kafka.bridge.hook.client.disconnected.topic  = mqtt_client_disconnected
```

Start the EMQ broker and load the plugin 
-----------------
1) cd /emqx
2) ./bin/emqx start
3) ./bin/emqx_ctl plugins load emqx_kafka_bridge




