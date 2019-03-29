数据迁移 前面是源，后面是目标，从A拷贝到B  前面是A，后面是B

cd node\_modules/elasticdump/bin

./elasticdump  --input=http://10.80.178.251:9200/ca4b6a427e5a4a3b8c68d514f6114cf8 --output=http://10.45.54.174:9266/ca4b6a427e5a4a3b8c68d514f6114cf8 --type=data

./elasticdump  --input=http://10.47.109.85:9200/district\_dict --output=http://10.80.226.191:9200/district\_dict --type=data







