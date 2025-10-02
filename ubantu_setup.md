
## 🔧 Prerequisites

### ✅ 1. **Install WSL and Ubuntu on Windows**

1. Open **PowerShell as Administrator**, and run:

   ```powershell
   wsl --install
   ```

2. Restart your computer if prompted.

3. After restart, **Ubuntu** will install automatically (or you can install it from the **Microsoft Store**).

4. Once installed, open **Ubuntu terminal** and set your **username and password**.

---

### ✅ 2. **Update Ubuntu Packages**

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🧰 Step-by-Step Hadoop Installation on Ubuntu (in WSL)

---

### ✅ 3. **Install Java (Hadoop requires Java)**

```bash
sudo apt install openjdk-8-jdk -y
```

Verify:

```bash
java -version
```

---

### ✅ 4. **Create Hadoop User (Optional but recommended)**

```bash
sudo addgroup hadoop
sudo adduser --ingroup hadoop hduser
```

Set password when prompted.

Give sudo privileges:

```bash
sudo usermod -aG sudo hduser
```

Switch to the new user:

```bash
su - hduser
```

---

### ✅ 5. **Download and Install Hadoop**

1. Download Hadoop (Stable version e.g., 3.3.6):

```bash
wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
```

2. Extract it:

```bash
tar -xzf hadoop-3.3.6.tar.gz
```

3. Move it to `/usr/local`:

```bash
sudo mv hadoop-3.3.6 /usr/local/hadoop
```

4. Change ownership:

```bash
sudo chown -R hduser:hadoop /usr/local/hadoop
```

---

### ✅ 6. **Set Hadoop Environment Variables**

Edit `.bashrc`:

```bash
nano ~/.bashrc
```

Add the following at the end:

```bash
# Java
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# Hadoop
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native

export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

Reload:

```bash
source ~/.bashrc
```

---

### ✅ 7. **Configure Hadoop for Single-Node Cluster**

Edit config files inside `/usr/local/hadoop/etc/hadoop`

#### 1. `hadoop-env.sh`

```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```

Set:

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

---

#### 2. `core-site.xml`

```bash
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

Replace with:

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

---

#### 3. `hdfs-site.xml`

```bash
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

Replace with:

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///home/hduser/hadoopdata/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///home/hduser/hadoopdata/hdfs/datanode</value>
    </property>
</configuration>
```

Create the directories:

```bash
mkdir -p ~/hadoopdata/hdfs/namenode
mkdir -p ~/hadoopdata/hdfs/datanode
```

---

#### 4. `mapred-site.xml`

Copy template:

```bash
cp $HADOOP_HOME/etc/hadoop/mapred-site.xml.template $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

Edit:

```bash
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

Replace with:

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

---

#### 5. `yarn-site.xml`

```bash
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

Add:

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

---

### ✅ 8. **Format the Namenode**

```bash
hdfs namenode -format
```

---

### ✅ 9. **Start Hadoop Daemons**

```bash
start-dfs.sh
start-yarn.sh
```

To verify, use:

```bash
jps
```

Expected output:

```
NameNode
DataNode
ResourceManager
NodeManager
SecondaryNameNode
```

---

### ✅ 10. **Access Hadoop Web Interfaces**

Open your browser and visit:

* NameNode: [http://localhost:9870](http://localhost:9870)
* ResourceManager: [http://localhost:8088](http://localhost:8088)

---

## 🧪 Test HDFS

Create directory in HDFS:

```bash
hdfs dfs -mkdir /input
hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml /input
```

List files:

```bash
hdfs dfs -ls /input
```

---

## ✅ Optional: Run Example MapReduce Job

```bash
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep /input /output 'dfs[a-z.]+'
```

View output:

```bash
hdfs dfs -cat /output/*
```

---

## 🔚 Done!

You now have a fully running local Hadoop setup on Windows using Ubuntu (via WSL).

---

