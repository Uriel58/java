# 常見的 Linux 操作指令：

### **1. 文件與目錄操作**

| **指令**                        | **作用**                        |
| ------------------------------- | ------------------------------- |
| ls                              | 查看目錄內容                    |
| ls -l  或者 ll                  | 查看詳細目錄                    |
| cd 目錄名稱                     | 進入指定目錄                    |
| pwd （Print Working Directory） | 顯示當前所在目錄                |
| mkdir 目錄名稱                  | 建立新目錄                      |
| rmdir 目錄名稱                  | 刪除空目錄                      |
| rm -r 目錄名稱                  | 刪除目錄及其內容                |
| touch 文件名稱                  | 建立空文件                      |
| rm 文件名稱                     | 刪除文件remove                  |
| cp 原文件 目標文件              | 複製文件                        |
| mv 原文件 目標文件              | 移動或重新命名文件              |
| cat 文件名稱                    | 查看文件內容，全名concatenation |
| more 文件名稱 / less 文件名稱   | 分頁查看文件內容                |
| head -n 文件名稱                | 查看文件前 n 行                 |
| tail -n 文件名稱                | 查看文件後 n 行                 |

![188842_0.jpg](C:\Users\USER\Downloads\188842_0.jpg)

### **2. 使用者與權限管理**

| **指令**                   | **作用**                      |
| -------------------------- | ----------------------------- |
| whoami                     | 查看當前使用者                |
| who                        | 查看當前登入的使用者          |
| id 使用者名稱              | 查看使用者的 UID、GID 等資訊  |
| chmod 777 文件名稱         | 修改文件權限（數字/符號模式） |
| chown 使用者:群組 文件名稱 | 更改文件擁有者                |
| sudo 指令                  | 以管理員身份執行指令          |

**補充:**

每個檔案或目錄都有三組權限,分別是針對!

1. User (u) - 檔案擁有者

2. Group (g) -搬有該檔案所属群組的使用者

3. Others (o) -所有其他使用者

4. r讀取=4

5. w寫入=2

6. x執行=1

7. -沒有這權限=0

   ​	例如：drwxrwxr-x

   ​	**修改權限**

1. chmod u+r 1.txt(filename)增加r
2. chmod u-r 1.txt(filename)減少r
3. chmod u=r 1.txt(filename)只有r

![188843_0.jpg](C:\Users\USER\Downloads\188843_0.jpg)

### **3. 程序管理**

| **指令**    |        **作用**         |
| ----------- | :---------------------: |
| ps aux      |  查看所有運行中的程序   |
| top         |  實時查看系統運行狀態   |
| kill PID    |   終止指定 PID 的程序   |
| kill -9 PID | 強制終止指定 PID 的程序 |
| jobs        |   查看後台運行的程序    |
| bg %n       | 將後台程序轉為背景運行  |
| fg %n       | 將後台程序轉為前台運行  |

### **4. 磁碟與存儲管理**

| **指令**    | **作用**         |
| ----------- | ---------------- |
| df -h       | 查看磁碟使用情況 |
| du -sh 目錄 | 查看目錄大小     |
| mount       | 查看已掛載的磁碟 |
| umount 目標 | 卸載掛載點       |

### **5. 網絡管理**

| **指令**        | **作用**           |
| --------------- | ------------------ |
| ifconfig / ip a | 查看網絡介面資訊   |
| ping 目標地址   | 測試網絡連通性     |
| netstat -tulnp  | 查看正在監聽的端口 |
| telnet 目標地址 | 看狀態             |
| curl 網址       | 請求網頁內容       |
| wget 網址       | 下載文             |

關於子網遮罩 255.255.255.0 的說明：

✅ 子網遮罩：255.255.255.0
CIDR 表示法：/24

二進位表示：11111111.11111111.11111111.00000000

總 IP 數量：256 個

可用 IP 數量：254 個
（其中 1 個是網路位址，1 個是廣播位址，不可分配）

📌 範例：如果你的網段是 192.168.1.0/24
類別 位址
網路位址（Network） 192.168.1.0
第一個可用 IP 192.168.1.1
最後一個可用 IP 192.168.1.254
廣播位址（Broadcast） 192.168.1.255

- <u>192.0.0.1：3306的3306是port</u>

### **6. Ubuntu 軟體管理**

Ubuntu 使用 **APT（Advanced Package Tool）** 來管理軟體，以下是 Ubuntu 特有或常用的 APT 指令：

| **指令**                   | **作用**                                   |
| -------------------------- | ------------------------------------------ |
| sudo apt update            | 更新軟體包索引（不會升級軟體）             |
| sudo apt upgrade           | 升級所有已安裝的軟體                       |
| sudo apt full-upgrade      | 進行完整的系統升級，可能會移除不必要的軟體 |
| sudo apt install <package> | 安裝指定的軟體包                           |
| sudo apt remove <package>  | 移除指定的軟體（但保留設定檔）             |
| sudo apt purge <package>   | 完全移除指定軟體，包括設定檔               |
| apt-cache search <keyword> | 搜尋軟體包                                 |
| apt list --installed       | 列出所有已安裝的軟體                       |
| sudo apt autoremove        | 移除不再需要的套件                         |
| sudo dpkg -i <package>.deb | 手動安裝 .deb 軟體包                       |
| sudo dpkg -r <package>     | 手動移除 .deb 軟體包                       |

#### <u>小技巧:</u>

- 點兩下滑鼠按滾輪可以直接貼選取的字
- 按tab補全檔名
- ifconfig常用可能需要sudo apt install net-tools
- Putty是很適合搭配vitualbox
- vim很方便
- 1.設定語系為canada，此時terminal可以打開，但不能使用sudo命令

  2.開啟使用者可以使用sudo命令

   在terminal輸入

  ![ex1.png](C:\Users\USER\Downloads\ex1.png)

  加入自已帳號如下terry

  ![ex2.png](C:\Users\USER\Downloads\ex2.png)

  Ctrl + o 存檔

  Crtl + x 離開

錯誤訊息如下

tomcat java.lang.UnsupportedClassVersionError: org/eclipse/jdt/internal/compiler/env/INameEnvironment has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0

解決方法 

1.依錯誤所示之版本，安裝對應版本的JAVA JDK。

 class file版本與JDK版本對應關係可參照網址，https://javaalmanac.io/bytecode/versions/

2.安裝任意版本JDK後，下JDK切換指令，設定回JAVA8





1.BookController 無法正確執行修改操作的解決方法

line 35  } **else** **if** ("update".equals(action)) 

修改為 } **else** **if** ("edit".equals(action))

2.BookController 無法正確執行刪除操作的解決方法

刪除流程的觸發不在doPost，而是doGet。

<a href="javascript:void(0);" onclick="confirmDelete(<%= book.getBookID() %>)">🗑 刪除</a>

<form id=*"deleteForm"* action=*"BookController"* method=*"post"* style="display: *none*;">

  <input type=*"hidden"* name=*"action"* value=*"delete"*>

  <input type=*"hidden"* name=*"bookID"* id=*"bookID"*>

</form>

<script>

  **function** confirmDelete(bookID) {

​    **if** (confirm('確定要刪除嗎？')) {

​      document.getElementById('bookID').value = bookID;

​      document.getElementById('deleteForm').submit();

​    }

  }

</script>
