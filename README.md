# hadoop-lzo
A refactory from https://github.com/cloudera/hadoop-lzo to solve build problems.
The main difference from the source repository is the adjustment proposed in the issue response from https://github.com/twitter/hadoop-lzo/issues/35 

## Adding LZO compress support to Hbase
The reference for the info below and troubleshooting is http://opentsdb.net/setup-hbase.html

### Pre-requisites 
```
apt-get install ant liblzo2-dev # Debian/Ubuntu 
yum install ant ant-nodeps lzo-devel.x86_64 # RedHat/CentOS/Fedora 
brew install lzo # Mac OS X
```

### Compile & Deploy
```
git clone git://github.com/venidera/hadoop-lzo.git
export HBASE_HOME=/path/to/hbase
cd hadoop-lzo
CLASSPATH=$HBASE_HOME/lib CFLAGS=-m64 CXXFLAGS=-m64 ant compile-native tar
mkdir -p $HBASE_HOME/lib/native
cp build/hadoop-lzo-*/hadoop-lzo-*.jar $HBASE_HOME/lib
cp -a build/hadoop-lzo-*/lib/native/* $HBASE_HOME/lib/native
```
