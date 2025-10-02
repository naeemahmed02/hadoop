# 📘 Hadoop HDFS Tutorial: How to Write and Read Files

> ✅ This tutorial covers how to **upload (write)** a file to HDFS and **read** it back using simple commands.

---

## 🧠 Prerequisites

Make sure the following are already set up:

* Hadoop is installed and running on your system (pseudo-distributed or cluster mode).
* HDFS services are running: `NameNode`, `DataNode`.
* You can run `hdfs dfs` or `hadoop fs` commands from the terminal.
* Your user (e.g., `hduser`) is able to access and write to HDFS.

---

## 📁 Step 1: Create a Local File

First, create a simple file in your **local filesystem** (not HDFS):

```bash
echo "Hello, Hadoop HDFS!" > hello.txt
```

You now have a file `hello.txt` in your current directory.

---

## 📂 Step 2: Create a Directory in HDFS

HDFS does **not** auto-create directories. You must manually create the target directory before uploading.

```bash
hdfs dfs -mkdir -p /user/hduser
```

* `-p` ensures intermediate directories like `/user` are created if they don’t already exist.
* Replace `hduser` with your actual Hadoop username if different.

---

## ⬆️ Step 3: Write (Upload) the File to HDFS

Now copy the file from your local system to the HDFS directory:

```bash
hdfs dfs -put hello.txt /user/hduser/
```

✅ If successful, there will be no output (silent success).

---

## 📄 Step 4: List Files in HDFS

To verify your file is in HDFS, list the contents of the target directory:

```bash
hdfs dfs -ls /user/hduser/
```

Sample output:

```
Found 1 items
-rw-r--r--   1 hduser supergroup        20 2025-10-02 17:42 /user/hduser/hello.txt
```

---

## 📖 Step 5: Read the File from HDFS

To view the contents of the file stored in HDFS:

```bash
hdfs dfs -cat /user/hduser/hello.txt
```

Output:

```
Hello, Hadoop HDFS!
```

---

## ✅ Additional Useful HDFS Commands

| Command                         | Description                             |
| ------------------------------- | --------------------------------------- |
| `hdfs dfs -mkdir -p /path`      | Create directory in HDFS                |
| `hdfs dfs -put local.txt /path` | Upload local file to HDFS               |
| `hdfs dfs -get /path/file.txt`  | Download file from HDFS to local system |
| `hdfs dfs -cat /path/file.txt`  | View file contents                      |
| `hdfs dfs -rm /path/file.txt`   | Delete file from HDFS                   |
| `hdfs dfs -ls /path`            | List directory contents                 |
| `hdfs dfs -tail /path/file.txt` | View end of a file                      |

---

## 🌐 Bonus: View Files in Web UI

You can also browse files in HDFS using the Hadoop NameNode Web UI:

📍 Open in browser:

```
http://localhost:9870/
```

1. Click **"Utilities" > "Browse the file system"**
2. Navigate to `/user/hduser/` to see your uploaded file.

---

## 🧪 Practice Challenge

Try this on your own:

1. Create a file `poem.txt` with some text.
2. Create a new directory in HDFS: `/data/poems/`
3. Upload the file.
4. Read it back and confirm contents.

```bash
echo "Roses are red, violets are blue." > poem.txt
hdfs dfs -mkdir -p /data/poems
hdfs dfs -put poem.txt /data/poems/
hdfs dfs -cat /data/poems/poem.txt
```

---

## 🎓 Summary

| Task           | Command                               |
| -------------- | ------------------------------------- |
| Write to HDFS  | `hdfs dfs -put localfile /hdfs/path/` |
| Read from HDFS | `hdfs dfs -cat /hdfs/path/file`       |

You now know how to upload and read files using HDFS like a pro!

---

