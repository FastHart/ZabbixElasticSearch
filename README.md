## Description
Here is a template (zabbix v.4.0) and a script for monitoring ElasticSearch by zabbix-agent.

Forked from: https://github.com/adel-s/zabbix.git

## Requirements
Python 2.7 + with modules: elasticsearch, json, os, sys, time

## Installation
- Copy files:

  - `elasticsearch.conf` - to the /etc/zabbix/zabbix_agentd.conf.d
  - `es_zabbix.py`       - to the /etc/zabbix/scripts

- Edit variable `es_host` in `es_zabbix.py` if your elasticsearch is not listen on localhost

- Import zabbix template and add to the server in zabbix.

## Usage

    es_zabbix.py api metric

### Discovery mode

    es_zabbix.py discovery [nodes|node|indices]

   * **nodes** - return all nodes:

    { "data": [
        {"{#NODE}": "MyizaDUjRKiMiebzpRoNOg", "{#NAME}": "elastic-data1"},
        {"{#NODE}": "d73JzeMSRTKufO3gs1Ysdg", "{#NAME}": "elastic-master1"},
        {"{#NODE}": "g34SmibkR7C2mUKFMTkxbg", "{#NAME}": "elastic-client1"}
        ]}
   
   * **node** - return current local node:

    { "data": [
        {"{#NODE}": "g34SmibkR7C2mUKFMTkxbg",
        "{#NAME}": "elastic-client1"}
        ]}
    
   * **indices** - return all indexes in cluster:
   
    { "data": [
        {"{#NAME}": ".kibana"},
        {"{#NAME}": ".monitoring-es-6-2018.02.13"},
        {"{#NAME}": "my_cool_index"}
        ]}
    
### Metrics mode
    es_zabbix.py [cluster|health|indices|nodes] metric:submetric:submetric2

#### Example №1 - health metrics:
Command line:

    # es_zabbix.py health status
    green
    
Item in Zabbix:

    elasticsearch[health,status]
    
#### Example №2 - count of documents on node 
Command line:

    # es_zabbix.py nodes nodes:MyizaDUjRKiMiebzpRoNOg:indices:docs:count
    10028441640

Item in Zabbix:

    elasticsearch[nodes,nodes:MyizaDUjRKiMiebzpRoNOg:indices:docs:count]

#### Example №3 - index time
Command line:

    # es_zabbix.py indices indices:my_cool_index:primaries:indexing:index_time_in_millis
    240183
    
Item in Zabbix:

    elasticsearch[indices,indices:my_cool_index:primaries:indexing:index_time_in_millis]

