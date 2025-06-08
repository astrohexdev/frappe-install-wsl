# 🛠️ Frappe Development Environment Setup (Python 3.12, WSL/Linux)

> **Requirements**: Python >= 3.10  
> This guide uses **Python 3.12**

---

## ⚠️ Warning for WSL/Linux Users

If you are running as `root` or don’t have a normal user, **create a new user**.  
This is especially important when using **WSL**.

### ➕ Add New User

```bash
root@pc:~$ adduser helen
root@pc:~$ usermod -aG sudo helen
```

### 🔄 Switch to New User

```bash
root@pc:~$ su - helen
helen@pc:~$ whoami   # should print: helen
```

---

## 🧩 [0] Exit Root (Optional Cleanup)

```bash
root@pc:~$ whoami   # should print: root
root@pc:~$ exit     # exits root
helen@pc:~$ whoami  # helen
```

---

## 🧱 System Setup

### [1] Update APT

```bash
sudo apt update
sudo apt upgrade
```

### [2] Install Git

```bash
sudo apt-get install git
```

### [3] Install Python Dependencies

```bash
sudo apt-get install python3-setuptools python3-pip
```

### [4] Install Virtual Environment

```bash
sudo apt install python3.12-venv
```

---

## 💾 Database Setup

### [5] Install MariaDB

```bash
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo mysql_secure_installation
```

**Recommended `mysql_secure_installation` Options:**

```txt
Enter current password for root: (Press Enter)
Switch to unix_socket authentication: Y
Change the root password: Y
    New password: 1234
    Re-enter: 1234
Remove anonymous users: Y
Disallow root login remotely: Y
Remove test database: Y
Reload privilege tables: Y
```

---

### [6] Install MySQL Development Files

```bash
sudo apt-get install libmysqlclient-dev
```

---

### [7] Configure MariaDB

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Add or ensure the following values exist:

```ini
[server]
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log

[mysqld]
innodb-file-format = barracuda
innodb-file-per-table = 1
innodb-large-prefix = 1
character-set-client-handshake = FALSE
collation-server = utf8mb4_unicode_ci      

[mysql]
default-character-set = utf8mb4
```

Save with `Ctrl+X`, then press `Y`, then `Enter`.

---

### [8] Restart MySQL

```bash
sudo service mysql restart
```

---

## 🔧 Dependencies

### [9] Install Redis

```bash
sudo apt-get install redis-server
```

---

### [10] Install Node.js via NVM

```bash
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
```

---

### [11] Install Yarn

```bash
sudo apt-get install npm
sudo npm install -g yarn
```

---

### [12] Install wkhtmltopdf

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

---

### [13] Install Bench CLI

```bash
pip3 install frappe-bench
```

---

## 🧪 Bench Setup

### [14] Initialize Bench (v15 + Python 3.12)

```bash
bench init frappe-bench --frappe-branch version-15 --python python3.12
cd frappe-bench/
bench new-site frappe.dev
```

---

### [15] Create Current Site File

```bash
echo "frappe.dev" > sites/currentsite.txt
```

---

### [16] Set Current Site & Start Bench

```bash
bench use frappe.dev
bench start
```

---

### [17] Open in Browser

Visit: [http://localhost:8000](http://localhost:8000)

---

## 🧪 Default Testing Values

| Item               | Value        |
|--------------------|--------------|
| Linux user pass    | `1234`       |
| SQL root password  | `1234`       |
| Frappe username    | `administrator` |
| Frappe password    | `1234`       |

---

## ✅ Done!  
You’re now ready to develop with Frappe on your local Linux/WSL system.
