---
title: How to install
layout: plain
top-nav-title: How to install
top-nav-group: quickstart
top-nav-pos: 3
sub-nav-title: How to install
sub-nav-group: quickstart
sub-nav-pos: 3
---
## Installation Steps
* Download released package from [Downloads]({{site.baseurl}}/downloads)
* Deploy Zookeeper cluster
* Install Python 2.6
* Install JDK
* Install JStorm
* Deploy web UI
* Start JStorm cluster

## Deploy Zookeeper cluster
* Please refer [ZooKeeper Getting Started Guide](http://zookeeper.apache.org/doc/trunk/zookeeperStarted.html)
* Please google "How to install Zookeeper"

## Deploy JStorm cluster

### Install python 2.6
If the python version in current system is 2.4 or higher, please skip this section.

You can also use https://github.com/utahta/pythonbrew to install python
```
> curl-kL http://xrl.us/pythonbrewinstall | bash -s $HOME/.pythonbrew/etc/bashrc && source $HOME/.pythonbrew/etc/bashrc
pythonbrew install 2.6.7
pythonbrew switch 2.6.7
```

### Install java
For JStorm versions lower than 2.1.0(including 2.1.0), please use jdk1.7.
For JStorm 2.1.1 or later, please use jdk 1.7 or 1.8.

Note that,  if the current OS is 64-bit, please install 64-bit jdk; and if OS is a 32-bit system, then download one jdk for 32 bit.


### Install JStorm
Take jstorm-2.1.1.zip as an example

```
unzip jstorm-2.1.1.zip
vi ~/.bashrc
```

If you want to build from source, you can clone the source code to your local machine, then go to the root directory of sources, let's say `~/jstorm`, then use:
`mvn clean package assembly:assembly -Dmaven.test.skip=true`
to build, you can find the tgz file under `~/jstorm/target` directory.

```
export JSTORM_HOME=/XXXXX/XXXX
export PATH=$PATH:$JSTORM_HOME/bin
```

Then edit $JSTORM_HOME/conf/storm.yaml,
please refer to [JStorm Configuration]({{site.baseurl}}/quickstart/configuration.html) for more details.

### Install JStorm Web UI
Note: Web UI can run on the node different from the Nimbus node (on any machine that can connect the cluster nodes)

Download tomcat 7.x (take apache-tomcat-7.0.37 as an example)

```
mkdir ~/.jstorm 
cp -f $JSTORM_HOME/conf/storm.yaml ~/.jstorm
tar -xzf apache-tomcat-7.0.37.tar.gz
cd apache-tomcat-7.0.37
cd webapps
cp $JSTORM_HOME/jstorm-ui-2.1.1.war ./
mv ROOT ROOT.old
ln -s jstorm-ui-2.1.1 ROOT
cd ../bin
./startup.sh
```

### Start JStorm
* Run `nohup jstorm nimbus &` in the nimbus node, view $JSTORM_HOME/logs/nimbus.log to check if any errors.
* Run `nohup jstorm supervisor >/dev/null 2>&1 &` in the supervisor node, and keep an eye on  $JSTORM_HOME/logs/supervisor.log to check if any errors.

* You can also use `$JSTORM_HOME/bin/start.sh` to start nimbus & supervisor(within the same machine) simultaneously. Use `$JSTORM_HOME/bin/stop.sh` to stop nimbus & supervisor simultaneously.
