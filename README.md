# Splunk_lab
Splunk lab is a highly customizable docker based environment that can be spun on a single device as a training environment. The provided docker-compose file is build off of Splunk 9.0.0. 
This lab consists of:
  - 1 Cluster Manager
  - 1 Indexer
  - 1 Search Head 

Splunk docker utilzes a linux docker image and ansible to streamline installation. Typically the a provided default.yml configuration can be used to configure a singular docker instances. An example of the default.yml file is as follows:
<pre><code>
splunk:
  opt: /opt
  home: /opt/splunk
  user: splunk
  group: splunk
  exec: /opt/splunk/bin/splunk
  pid: /opt/splunk/var/run/splunk/splunkd.pid
  password: "{{ splunk_password | default(<password>) }}"
  svc_port: 8089
  s2s_port: 9997
  http_port: 8000
  hec:
    enable: True
    ssl: True
    port: 8088
    # hec.token is used only for ingestion (receiving Splunk events)
    token: <default_hec_token>
  smartstore: null
 </code></pre>
  
To expidite installations variables can be injected into the docker instance during the installation. First we must determine the type of role the docker instance will have. NOTE: Each docker instance can only have one role. Here is a list of roles.

<pre><code>
    splunk_standalone
    splunk_search_head
    splunk_indexer
    splunk_deployer
    splunk_license_master
    splunk_cluster_master
    splunk_heavy_forwarder
</code></pre>

Additionally there are a plethora of variables to inject into your docker instances to customize it to your needs. Some of the variables used in this lab environment are below. For a more extensive list see the documentation: https://splunk.github.io/docker-splunk/ADVANCED.html#global-variables. 

<pre><code>
 - SPLUNK_HTTP_ENABLESSL=true
 - SPLUNK_START_ARGS=--accept-license <required in order to start container>
 - SPLUNK_INDEXER_URL=<list of each indexer's hostname>
 - SPLUNK_SEARCH_HEAD_URL= <list of each search head's hostname>
 - SPLUNK_SEARCH_HEAD_CAPTAIN_URL=<hostname of which container to make the captain>
 - SPLUNK_CLUSTER_MASTER_URL=<hostname of the cluster master>
 - SPLUNK_ROLE=<what role to use for this container>
 - SPLUNK_DEPLOYER_URL=<hostname of the deployer>
 - SPLUNK_LICENSE_URI=<uri to your Splunk Enterprise license>
 - DEBUG=<true/false>
</code></pre>


