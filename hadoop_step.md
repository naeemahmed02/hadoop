
---

# 📝 Complete Guide: Installing & Configuring Hadoop 3.4.1 on Windows 10/11

---

## 1. 📦 Prerequisites

1. **Install Java 8 (JDK)**

   * Download from [Adoptium Temurin JDK 8](https://adoptium.net/temurin/releases/?version=8).
   * Install (e.g. `C:\Program Files\Eclipse Adoptium\jdk-8.0.462.8-hotspot`).

2. **Verify Java**

   ```cmd
   java -version
   ```

   Should show:

   ```
   openjdk version "1.8.0_462"
   ```

---

## 2. 📂 Download Hadoop

1. Get Hadoop **3.4.1 binary**:
   👉 [Apache Hadoop Releases](https://hadoop.apache.org/releases.html)

2. Extract to:

   ```
   C:\hadoop
   ```

---

## 3. ⚙️ Setup Environment Variables

Go to **System Properties → Advanced → Environment Variables** and add:

* **JAVA_HOME**

  ```
  C:\Progra~1\Eclipse Adoptium\jdk-8.0.462.8-hotspot
  ```

* **HADOOP_HOME**

  ```
  C:\hadoop
  ```

* **HADOOP_COMMON_HOME**

  ```
  C:\hadoop
  ```

* **HADOOP_HDFS_HOME**

  ```
  C:\hadoop
  ```

* **HADOOP_MAPRED_HOME**

  ```
  C:\hadoop
  ```

* **HADOOP_YARN_HOME**

  ```
  C:\hadoop
  ```

* **Path (add)**

  ```
  %JAVA_HOME%\bin
  %HADOOP_HOME%\bin
  %HADOOP_HOME%\sbin
  ```

✅ Verify:

```cmd
echo %JAVA_HOME%
echo %HADOOP_HOME%
hadoop version
```

---

## 4. 🖥️ Fix Windows Compatibility (`winutils.exe`)

Hadoop needs **winutils.exe** on Windows.

1. Download from GitHub:
   👉 [winutils for Hadoop 3.3.6+](https://github.com/cdarlint/winutils/tree/master/hadoop-3.3.6/bin)

2. Copy `winutils.exe` →

   ```
   C:\hadoop\bin\
   ```

3. Also copy `hadoop.dll` (if provided in repo) to the same folder.

✅ Now Hadoop won’t complain about `FileNotFoundException: winutils.exe`.

---

## 5. 📑 Configure Hadoop XML Files

Inside `C:\hadoop\etc\hadoop` update:

### **core-site.xml**

```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

---

### **hdfs-site.xml**

```xml
<configuration>
  <property>
    <name>dfs.permissions.enabled</name>
    <value>false</value> <!-- needed on Windows -->
  </property>

  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>

  <property>
    <name>dfs.namenode.name.dir</name>
    <value>file:/C:/hadoop/data/namenode</value>
  </property>

  <property>
    <name>dfs.datanode.data.dir</name>
    <value>file:/C:/hadoop/data/datanode</value>
  </property>
</configuration>
```

---

### **mapred-site.xml**

(Rename `mapred-site.xml.template` → `mapred-site.xml`)

```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

---

### **yarn-site.xml**

```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>

  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>localhost</value>
  </property>
</configuration>
```

---

## 6. 🏗️ Format the NameNode

```cmd
hdfs namenode -format
```

Expected output:

```
Storage directory C:\hadoop\data\namenode has been successfully formatted.
```

---

## 7. 🚀 Start Hadoop Services

Start DFS:

```cmd
start-dfs.cmd
```

Start YARN:

```cmd
start-yarn.cmd
```

Check running Java processes:

```cmd
jps
```

You should see:

```
NameNode
DataNode
ResourceManager
NodeManager
Jps
```

---

## 8. 🌐 Verify Hadoop UI

* HDFS NameNode UI → [http://localhost:9870](http://localhost:9870)
* YARN ResourceManager UI → [http://localhost:8088](http://localhost:8088)

---

## 9. 📂 Test HDFS

```cmd
hdfs dfs -mkdir /test
hdfs dfs -ls /
```

---

## ⚠️ Common Errors & Fixes

* **Error: JAVA_HOME incorrectly set**
  → Use `C:\Progra~1\...` short path in `hadoop-env.cmd`.

* **Error: Could not locate winutils.exe**
  → Copy `winutils.exe` to `C:\hadoop\bin\`.

* **UnsatisfiedLinkError (NativeIO$Windows.access0)**
  → Add in `hdfs-site.xml`:

  ```xml
  <property>
    <name>dfs.permissions.enabled</name>
    <value>false</value>
  </property>
  ```

* **Too many failed volumes**
  → Ensure directories `C:\hadoop\data\namenode` and `C:\hadoop\data\datanode` exist (create manually if needed).

---

✅ After this, you’ll have a **working single-node Hadoop cluster on Windows**.

---

