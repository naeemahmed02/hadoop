# 🚀 Hadoop Basic Commands Tutorial

Apache Hadoop is an open-source framework for distributed storage (HDFS) and processing (MapReduce/YARN). Here’s a guide to its most commonly used commands.

---

## 1. Checking Hadoop Version

```bash
hadoop version
```

👉 Shows the installed Hadoop version.

---

## 2. Starting and Stopping Hadoop Services

### Start HDFS and YARN

```bash
start-dfs.sh
start-yarn.sh
```

### Stop HDFS and YARN

```bash
stop-dfs.sh
stop-yarn.sh
```

👉 These commands start/stop Hadoop’s core services:

* **NameNode** (manages metadata)
* **DataNode** (stores actual data)
* **ResourceManager** (job scheduling)
* **NodeManager** (executes tasks)

---

## 3. HDFS File System Commands

Hadoop uses its own file system **HDFS (Hadoop Distributed File System)**.
To interact with it, use:

```bash
hdfs dfs -command [options]
```

---

### 🗂 File & Directory Management

* **Create a directory**

```bash
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/yourname
```

* **Create parent directories automatically**

```bash
hdfs dfs -mkdir -p /user/yourname/input
```

* **List files/directories**

```bash
hdfs dfs -ls /
hdfs dfs -ls /user/yourname
```

* **Check directory tree**

```bash
hdfs dfs -ls -R /
```

---

### 📂 Uploading & Downloading Files

* **Put local file into HDFS**

```bash
hdfs dfs -put localfile.txt /user/yourname/input/
```

* **Copy from local to HDFS**

```bash
hdfs dfs -copyFromLocal localfile.txt /user/yourname/input/
```

* **Get file from HDFS to local**

```bash
hdfs dfs -get /user/yourname/input/localfile.txt /home/user/
```

* **Copy from HDFS to local**

```bash
hdfs dfs -copyToLocal /user/yourname/input/localfile.txt /home/user/
```

---

### 📝 Viewing Files

* **View contents of a file**

```bash
hdfs dfs -cat /user/yourname/input/localfile.txt
```

* **View first few lines**

```bash
hdfs dfs -head /user/yourname/input/localfile.txt
```

* **View last few lines**

```bash
hdfs dfs -tail /user/yourname/input/localfile.txt
```

---

### 🔧 File Management

* **Remove file**

```bash
hdfs dfs -rm /user/yourname/input/localfile.txt
```

* **Remove directory**

```bash
hdfs dfs -rm -r /user/yourname/input
```

* **Move file**

```bash
hdfs dfs -mv /user/yourname/input/file1.txt /user/yourname/output/
```

* **Copy file within HDFS**

```bash
hdfs dfs -cp /user/yourname/input/file1.txt /user/yourname/backup/
```

---

### 📊 File & Storage Info

* **Check HDFS space usage**

```bash
hdfs dfs -du -h /user/yourname
```

* **Check overall HDFS health**

```bash
hdfs dfsadmin -report
```

---

## 4. Running a Sample MapReduce Job

Hadoop comes with example JARs. Try running **WordCount**:

```bash
hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /user/yourname/input /user/yourname/output
```

👉 This will count word frequency in the input file and save results in the output directory.

---

## 5. Viewing Job Output

After running a job:

```bash
hdfs dfs -ls /user/yourname/output
hdfs dfs -cat /user/yourname/output/part-r-00000
```

---

✅ That’s the **Hadoop basics toolkit**:

* Start/stop services
* Manage files in HDFS
* Run simple jobs

---

