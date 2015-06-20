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
Here an example to add LZO support in hbase:
```
wget http://www.eu.apache.org/dist/hbase/stable/hbase-1.0.1.1-bin.tar.gz
tar xvf hbase-1.0.1.1-bin.tar.gz
ln -s hbase-1.0.1.1 hbase

cat >conf/hbase-site.xml <<EOF
<?xml version="1.0"?> 
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?> 
<configuration> 
  <property> 
    <name>hbase.rootdir</name> 
    <value>file:////tmp/hbase</value> 
  </property>
</configuration> 
EOF

git clone git://github.com/venidera/hadoop-lzo.git
export HBASE_HOME=/path/to/hbase
cd hadoop-lzo
CLASSPATH=$HBASE_HOME/lib CFLAGS=-m64 CXXFLAGS=-m64 ant compile-native tar
mkdir -p $HBASE_HOME/lib/native
cp build/hadoop-lzo-*/hadoop-lzo-*.jar $HBASE_HOME/lib
cp -a build/hadoop-lzo-*/lib/native/* $HBASE_HOME/lib/native
```
