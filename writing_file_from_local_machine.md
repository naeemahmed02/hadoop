## 🧭 FULL GUIDE: Writing a `.txt` File from Your Local Machine to HDFS on Ubuntu (Hadoop)

---

### 🧱 Prerequisites

Make sure the following are true:

✅ You are using **WSL (Ubuntu)** on Windows.

✅ Hadoop is installed and configured correctly in WSL under user `hduser`.

✅ You have a `.txt` file located on your **Windows filesystem** (e.g., in Downloads).

✅ You have internet or local file access in WSL.

---

## 🪜 STEP 1: Open WSL and Switch to Hadoop User

From your Windows Terminal or Ubuntu shell, run:

```bash
su hduser
```

Enter your password for `hduser` if prompted.

---

## ⚙️ STEP 2: Start Hadoop Services

Start the Hadoop daemons:

```bash
start-dfs.sh
```

(Optional if using YARN):

```bash
start-yarn.sh
```

Check they're running:

```bash
jps
```

Expected output includes:

* `NameNode`
* `DataNode`
* `SecondaryNameNode`
* `ResourceManager` (optional)
* `NodeManager` (optional)

---

## 🗂️ STEP 3: Navigate to Your File in Windows via WSL

The Windows filesystem is mounted under `/mnt` in WSL.

Example:

If your file is located at:
`C:\Users\Lab-2 Instructor PC\Downloads\myfile.txt`

Then in WSL, run:

```bash
cd "/mnt/c/Users/Lab-2 Instructor PC/Downloads"
```

Or using escaped spaces:

```bash
cd /mnt/c/Users/Lab-2\ Instructor\ PC/Downloads
```

Then confirm your file is there:

```bash
ls myfile.txt
```

---

## 📤 STEP 4: Upload the File to HDFS

First, ensure your target HDFS directory exists:

```bash
hdfs dfs -mkdir -p /user/hduser
```

Then upload the file:

```bash
hdfs dfs -put myfile.txt /user/hduser/
```

---

## 🔍 STEP 5: Verify the Upload

Check that your file was uploaded:

```bash
hdfs dfs -ls /user/hduser/
```

You should see something like:

```
Found 1 items
-rw-r--r--   1 hduser supergroup     1234 2025-10-04 13:50 /user/hduser/myfile.txt
```

---

## 🧪 Optional: Read File Contents in HDFS

To view the content of the file stored in HDFS:

```bash
hdfs dfs -cat /user/hduser/myfile.txt
```

---

## 🧹 STEP 6: (Optional) Stop Hadoop After Use

To stop the Hadoop daemons:

```bash
stop-dfs.sh
```

And optionally:

```bash
stop-yarn.sh
```

---

## ✅ Summary Commands Cheat Sheet

| Action                   | Command Example                          |
| ------------------------ | ---------------------------------------- |
| Switch to Hadoop user    | `su hduser`                              |
| Start Hadoop services    | `start-dfs.sh` / `start-yarn.sh`         |
| Navigate to Windows path | `cd "/mnt/c/Users/.../Downloads"`        |
| List files               | `ls`                                     |
| Create HDFS directory    | `hdfs dfs -mkdir -p /user/hduser`        |
| Upload file to HDFS      | `hdfs dfs -put myfile.txt /user/hduser/` |
| List files in HDFS       | `hdfs dfs -ls /user/hduser/`             |
| Read file in HDFS        | `hdfs dfs -cat /user/hduser/myfile.txt`  |
| Stop Hadoop              | `stop-dfs.sh` / `stop-yarn.sh`           |

---

