資料來源:

https://docs.google.com/document/d/1TVhEHhh7Q-MJIZY6IpM9PJkJzC0RSolL9yI80PcDvBY/edit?tab=t.0

## **第一章：JSP 基礎**

## **🌐 JSP 是什麼？**

**JSP (JavaServer Pages)** 是一種 **伺服端網頁技術**，由 Sun Microsystems（現屬 Oracle）推出。
 它的主要目的，是讓開發者能夠在 **HTML 頁面中直接嵌入 Java 程式碼**，用來產生動態網頁內容。

簡單說：

- **HTML** → 靜態頁面，內容固定。

  

- **JSP** → 動態頁面，可以根據資料庫或使用者輸入來生成不同的內容。

  

放進webapps資料夾

```java
sudo find / -type d -name webapps 2>/dev/null
```



- /var/lib/tomcat9/webapps/ROOT
- sudo chmod 777 index.jsp
- sudo nano /var/lib/tomcat9/webapps/ROOT/index.jsp
- webapps底下
  - mkir myapp
  - touch hello.jsp 

例如：

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<body>
  <h1>Hello, <%= request.getParameter("name") %>!</h1>
</body>
</html>
```



輸入網址 hello.jsp?name=Terry → 顯示 **Hello, Terry! ？後的為參數**



------



## **⚙️ JSP 與 Servlet 的關係**

JSP 本質上就是 **Servlet 的一種特殊寫法**。

- Servlet 是用 **Java 程式碼**寫的伺服端邏輯。

  

- JSP 是用 **HTML 為主，搭配 Java 程式碼** 的頁面。

  

### **工作流程（JSP → Servlet → HTML）**

1. 使用者請求 xxx.jsp

   

2. **JSP 引擎**（Tomcat 等 Servlet 容器）會 **把 JSP 翻譯成 Servlet**

   

   - JSP → Java Servlet 程式

     

3. Servlet 被編譯成 .class 檔，執行後產生 HTML

   

4. 把結果回傳給使用者瀏覽器

   

👉 所以 **JSP 最後一定會變成 Servlet 執行**。

# **⚙️ JSP 的工作原理**

當使用者在瀏覽器輸入網址請求 xxx.jsp 時，整個流程如下：



------



## **1️⃣ 使用者請求 JSP 頁面**

- 瀏覽器發送 HTTP 請求給 Web 伺服器 (Tomcat、Jetty 等)。

  

例如輸入：

 http://localhost:8080/myapp/hello.jsp



------



## **2️⃣ JSP 引擎翻譯 JSP → Servlet**

- Tomcat 內部有 JSP 引擎，會將 hello.jsp **轉換成一個 Java Servlet 程式**。

  

轉換後的 Servlet 檔案通常放在：

 /work/Catalina/localhost/myapp/org/apache/jsp/hello_jsp.java

- 這個 .java 檔就是 Servlet 的程式碼，裡面會把 HTML 轉換成 out.print("...");，把 Java 程式碼放進去執行。

  

例如：

```java
<html>
<body>
  Hello, <%= request.getParameter("name") %>
</body>
</html>
```



 例如:http://localhost:8080/index.jsp?name=Uriel

會被翻譯成類似：

```java
out.write("<html><body>");
out.write("Hello, " + request.getParameter("name"));
out.write("</body></html>");
```





------



## **3️⃣ Servlet 編譯成 .class**

- JSP 引擎呼叫 Java 編譯器（javac），將 hello_jsp.java 編譯成 hello_jsp.class。

  

- 這是一個標準的 Java Servlet 類別。

  



------



## **4️⃣ Servlet 載入並執行**

- Servlet 容器（Tomcat）將編譯好的 .class 載入 JVM。

  

- 執行 service() 方法，處理 request / response。

  

- 程式碼中 out.write() 的內容會組合成 HTML。

當然可以，以下是 **JVM、JRE、JDK** 的繁體中文對照與解釋：



------



## **🧠 1. JVM（Java 虛擬機）**

| **項目**           | **說明**                                                     |
| ------------------ | ------------------------------------------------------------ |
| 是什麼？           | JVM 是 Java 的**核心執行環境**，負責執行 Java 編譯後的位元碼（.class 檔案）。 |
| 主要作用           | 將 Java 位元碼**轉換為本地機器可執行的機器碼**，達到跨平台的目的。 |
| 是否跨平台？       | ✅ 是的。Java 所謂的「一次編寫，到處執行」，就是靠 JVM 實現。 |
| 內含內容           | 類別載入器（Class Loader）、位元碼驗證器、直譯器（Interpreter）、即時編譯器（JIT）、垃圾回收器等。 |
| 是否需要單獨安裝？ | ❌ 不需要。JVM 是包含在 JRE 或 JDK 中的。                     |



------



## **📦 2. JRE（Java 執行環境）**

| **項目** | **說明**                                                     |
| -------- | ------------------------------------------------------------ |
| 是什麼？ | JRE 是 JVM 的**執行包裝環境**，提供執行 Java 應用程式所需的類庫與資源。 |
| 主要作用 | 讓你能夠**執行 Java 程式**，但不能用來開發或編譯程式。       |
| 內含內容 | JVM + Java 標準類別庫（例如：java.lang、java.util）+ 支援檔案。 |
| 適用情境 | 僅需**執行 Java 應用程式**時，可安裝 JRE 即可。              |



------



## **💻 3. JDK（Java 開發工具包）**

| **項目** | **說明**                                                     |
| -------- | ------------------------------------------------------------ |
| 是什麼？ | JDK 是完整的**Java 開發工具包**，適用於撰寫與開發 Java 應用程式。 |
| 主要作用 | 用來**撰寫、編譯、除錯與執行** Java 程式。                   |
| 內含內容 | JRE（含 JVM）+ 開發工具，如：• javac（編譯器）• java（程式啟動器）• javadoc• jdb（除錯器）等。 |
| 適用情境 | 進行 Java 應用開發時，必須安裝 JDK。                         |



------



## **🔁 總覽對照表**

| **功能／組件**     | **JVM** | **JRE** | **JDK** |
| ------------------ | ------- | ------- | ------- |
| 可否執行 Java 程式 | ✅       | ✅       | ✅       |
| 是否包含 JVM       | ✅       | ✅       | ✅       |
| 是否包含標準類別庫 | ❌       | ✅       | ✅       |
| 是否包含開發工具   | ❌       | ❌       | ✅       |
| 適合執行程式       | ✅       | ✅       | ✅       |
| 適合開發程式       | ❌       | ❌       | ✅       |



------



## **🎯 一句話總結：**

- **JVM**：執行 Java 程式的虛擬機器。

  

- **JRE**：讓你可以**執行 Java 程式**的環境（包含 JVM 和類別庫）。

  

- **JDK**：用來**開發 Java 程式**的完整工具包（包含 JRE 和開發工具）。







------



## **5️⃣ 回傳 HTML 給瀏覽器**

- Servlet 執行後產生的 **純 HTML**，會透過 HTTP 回傳給瀏覽器。

  

- 瀏覽器只看到結果（HTML），而不會看到 JSP 或 Java 程式碼。

  



------



## **🔄 總結流程（第一次請求 JSP）**

```
Browser (HTTP Request)
    ↓
Tomcat
    ↓
[JSP 引擎]
    1. 翻譯 JSP → Servlet (xxx_jsp.java)
    2. 編譯 Servlet → xxx_jsp.class
    3. 載入 JVM 執行
    ↓
產生 HTML 回傳
    ↓
Browser (顯示頁面)
```



⚡ **注意：**

- **第一次請求 JSP** → 一定會經過「翻譯 → 編譯 → 執行」。

  

- **之後再請求同一個 JSP** → 如果 JSP 檔案沒有修改過，就直接使用已經編譯好的 Servlet .class，省去翻譯與編譯的步驟 → 效率更高。

# **📊 JSP vs HTML / PHP / ASP.NET 比較**

| **技術**                         | **說明**                                         | **語言基礎**                     | **執行方式**                                 | **適合用途**                                             | **特點**                                                     |
| -------------------------------- | ------------------------------------------------ | -------------------------------- | -------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| **HTML**                         | **超文字標記語言，用來建立靜態網頁**             | **無（標記語言，不是程式語言）** | **瀏覽器直接解析**                           | **靜態頁面、公司簡介、靜態資訊**                         | **簡單、快速，但無法產生動態內容**                           |
| **JSP (JavaServer Pages)**       | **Java EE 技術，允許在 HTML 中嵌入 Java 程式碼** | **Java**                         | **翻譯成 Servlet → 編譯成 class → JVM 執行** | **Java Web 應用程式，需與 Servlet / JDBC / Spring 整合** | **強大、跨平台、與 Java 生態系整合良好**                     |
| **PHP (Hypertext Preprocessor)** | **輕量級伺服端腳本語言**                         | **PHP**                          | **直譯式（伺服器逐行執行 PHP 腳本）**        | **中小型網站、CMS（WordPress、Drupal）**                 | **入門快，生態豐富，與 LAMP（Linux+Apache+MySQL+PHP）相容性佳** |
| **ASP.NET**                      | **微軟推出的伺服端技術，可用 C# / VB.NET 撰寫**  | **C# / VB.NET**                  | **編譯成 .NET 中間語言 (IL) → CLR 執行**     | **Windows 平台的 Web 應用程式**                          | **與 Visual Studio 整合度高，支援 WebForms、MVC、Blazor**    |



------



## **🔑 差異分析**

1. **語言基礎**

   

   - **HTML：不是程式語言，只能靜態排版。**

     

   - **JSP：基於 Java，可直接呼叫 Java 類別、JDBC 操作資料庫。**

     

   - **PHP：獨立語言，常見於 LAMP 架構。**

     

   - **ASP.NET：基於 .NET 平台，支援多種語言（C# 最常用）。**

- **JDBC 操作資料庫：**負責連接資料庫+MySQL Workbench+DBeaver Community**，**JDBC **Driver 以前都須上網找再自己抓**

1. **執行模式**

   

   - **JSP：第一次請求會被「翻譯成 Servlet → 編譯 → JVM 執行」。**

     

   - **PHP：直譯式，逐行執行，部署快速。**

     

   - **ASP.NET：先編譯成 IL(Intermediate Language，中介語言），再交給 CLR(Common Language Runtime） 執行。**

     

   - **HTML：不需編譯，瀏覽器直接渲染。**

     

2. **跨平台性**

   **

   - **JSP：跨平台（依靠 JVM），可跑在任何支援 Servlet 的伺服器（Tomcat、Jetty、WebLogic）。**

     

   - **PHP：跨平台，依賴伺服器（Apache/Nginx）。**

     

   - **ASP.NET：最佳化於 Windows + IIS，但近年支援 .NET Core，可跨平台。**

     

   - **HTML：所有瀏覽器皆支援。**

     

3. **適合場景**

    

   - **HTML → 靜態公司官網 / 個人名片頁。**

     

   - **JSP → 大型 Java Web 系統（銀行、電商、企業內部系統）。**

     

   - **PHP → CMS（WordPress）、中小型網站。**

     

   - **ASP.NET → 企業級解決方案（ERP、政府系統、微軟技術堆疊）。**

     



------



## **✅ 總結**

- **如果你在 Java 生態系（Spring、Hibernate、Maven），選 JSP。**

  

- **如果要快速建站、用現成 CMS → PHP。**

  

- **如果公司是 微軟生態（Windows Server + SQL Server + Visual Studio），選 ASP.NET。**

  

- **如果只是做靜態網頁 → HTML。**

# **🚀 建立第一個 JSP 頁面（Hello JSP）**

## **1️⃣ 準備環境**

- 已安裝 **JDK**（Java 開發環境）

  

- 已安裝 **Tomcat**（或任何 Servlet 容器）

  - /var/lib/tomcat9/webapps/ROOT
  - sudo chmod 777 index.jsp
  - sudo nano /var/lib/tomcat9/webapps/ROOT/index.jsp
  - webapps底下
    - mkir myapp
    - touch hello.jsp 





專案目錄結構（Tomcat 預設）：

```
 apache-tomcat/
└── webapps/
    └── myapp/
        └── hello.jsp
```





------



## **2️⃣ 撰寫 hello.jsp**

在 webapps/myapp/ 底下新增一個 hello.jsp 檔案，內容如下：

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head>
    <title>Hello JSP</title>
</head>
<body>
    <h1>Hello JSP!</h1>

    <p>現在時間是： <%= new java.util.Date() %></p>
</body>
</html>
```



### **📌 說明：**

- <%@ page contentType="text/html; charset=UTF-8" %>
   設定網頁輸出的 MIME 類型與編碼。

  

- <%= new java.util.Date() %>
   這是一個 JSP **表達式（Expression）**，會即時顯示伺服器時間。

- JSP比較慢?

- ## **🚀 解決或替代方案**

| **替代技術**                                       | **說明**                                                     |
| -------------------------------------------------- | ------------------------------------------------------------ |
| **Thymeleaf**                                      | 現代 Java 模板引擎，語法更直覺，支援 Spring MVC，取代 JSP。  |
| **Spring Boot + REST API + 前端框架（Vue/React）** | 常見的前後端分離架構。                                       |
| **Freemarker / Velocity**                          | 舊版但仍被使用的模板技術，效能比 JSP 好一些。                |
| **JSP + JSTL + MVC 架構優化**                      | 若無法完全換掉 JSP，可以搭配 JSTL 標籤與 MVC 分離改善效能與結構。 |



------



## **3️⃣ 啟動 Tomcat**

在 Tomcat 的 bin/ 目錄下執行：

```bash
./startup.sh   # Linux / macOS
startup.bat    # Windows
```





------



## **4️⃣ 測試 JSP 頁面**

打開瀏覽器，輸入：

http://localhost:8080/myapp/hello.jsp

就會看到：

Hello JSP!

現在時間是：Fri Sep 26 12:34:56 CST 2025

## **第二章：JSP 環境建置**

# **☕ 安裝 JDK 教學**

## **1️⃣ 確認是否已安裝 JDK**

**在終端機 / 命令提示字元輸入：**

```bash
java -version
javac -version
```



- **如果出現版本號（例如** **java version "17.0.12"****），代表已安裝。

  

- **如果顯示「command not found」或「未被識別」，就需要安裝。**

  



------



## **2️⃣ 選擇 JDK 版本**

**目前主流有兩種：**

- **Oracle JDK（官方版，商業授權需注意授權條款）**

  

- **OpenJDK（開源版，功能一樣，免費）**

  

**👉 對於學習 JSP，推薦使用 OpenJDK 17（LTS 長期支援版）。**



------



## **3️⃣ 安裝方式**

### **📍 Windows**

1. **到 Adoptium 下載 Temurin JDK 17**

   

2. **安裝完成後，設定環境變數：**

   

**JAVA_HOME → 指到 JDK 安裝路徑，例如：**

****

```
C:\Program Files\Eclipse Adoptium\jdk-17
```



**編輯 Path，新增：**

****

```
 %JAVA_HOME%\bin
```



**重新打開 cmd，輸入：**

****

```bash
java -version
javac -version
```





------



### **📍 macOS**

**使用 Homebrew 安裝：**

```bash
 brew install openjdk@17
```



****


1. 

**安裝完成後，設定環境變數（寫入** **~/.zshrc** **或** **~/.bash_profile**）：

****

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH=$JAVA_HOME/bin:$PATH
```





1. 

**驗證：**

****
```bash
 java -version
```



**📍 Linux（Ubuntu / Debian）**

**更新套件庫：**

```bash
 sudo apt update
```



1. 

**安裝 OpenJDK 17：****
****
```bash
sudo apt install openjdk-17-jdk -y
```



**-y 會自動幫您回答yes**

1. 

**驗證：**

****
**java -version**

```bash
javac -version
```



# **🐱 安裝 Tomcat（Apache Tomcat）**

## **1️⃣ 下載 Tomcat**

1. **進入官方網站：**
   **👉 https://tomcat.apache.org/**

   

2. **建議使用 Tomcat 9 或 10（支援 Servlet 4.0 / JSP 2.3）**

   

   - **Tomcat 9 → 支援 Java 8 ~ 17**

     

   - **Tomcat 10 → 需 Java 11+（package 名稱從** **javax.\*** **改成** **jakarta.\*****）

     

**⚡ 如果你是初學 JSP，推薦 Tomcat 9 + JDK 17，最穩定。**



------



檢查tomcat和jvm版本

```bash
cd /usr/share/tomcat9/bin
./version.sh
```

結果:

```
uriel@uriel-BM6875-BM6675-BP6375:/usr/share/tomcat9/bin$ ./version.sh
Using CATALINA_BASE:  /usr/share/tomcat9
Using CATALINA_HOME:  /usr/share/tomcat9
Using CATALINA_TMPDIR: /usr/share/tomcat9/temp
Using JRE_HOME:    /usr
Using CLASSPATH:    /usr/share/tomcat9/bin/bootstrap.jar:/usr/share/tomcat9/bin/tomcat-juli.jar
Using CATALINA_OPTS:  
NOTE: Picked up JDK_JAVA_OPTIONS: --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
Server version: Apache Tomcat/9.0.58 (Ubuntu)
Server built:  Dec 30 1969 06:13:17 UTC
Server number: 9.0.58.0
OS Name:    Linux
OS Version:   6.8.0-40-generic
Architecture:  amd64
JVM Version:  11.0.28+6-post-Ubuntu-1ubuntu122.04.1
JVM Vendor:   Ubuntu
```



## **2️⃣ 安裝步驟**

### **📍 Windows**

1.  **下載 Windows Service Installer 或 zip 檔。**

   

**解壓縮到一個目錄，例如：**

****
```
C:\apache-tomcat-9.0.91
```



2. **設定環境變數：**



**CATALINA_HOME → Tomcat 目錄，例如：**

****
```
C:\apache-tomcat-9.0.91
```



**修改 Path，加入：**
****

```
%CATALINA_HOME%\bin
```



**啟動 Tomcat：**

****
```
startup.bat
```



3. **瀏覽器輸入：**

****
* http://localhost:8080/

4. **出現 Tomcat 預設首頁 = 安裝成功 🎉**



------



### **📍 macOS / Linux**

**下載 tar.gz 版本：**

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz
```



1. 

**解壓縮：**

****

```bash
tar -xvzf apache-tomcat-9.0.91.tar.gz
mv apache-tomcat-9.0.91 /usr/local/tomcat9
```

2. 

**設定環境變數（加到** **~/.bashrc** **或** **~/.zshrc**）：

****

```bash
export CATALINA_HOME=/usr/local/tomcat9
export PATH=$CATALINA_HOME/bin:$PATH
```

tomcat9開關使用

```bash
sudo systemctl start tomcat9
sudo systemctl stop tomcat9
sudo systemctl status tomcat9
sudo systemctl enable tomcat9
sudo systemctl disable tomcat9
```



3. 

**啟動 Tomcat：**
****

```bash
startup.sh
```



4. 

**測試：**

****

```
http://localhost:8080/
```

5. 

| apt                             | **wget**                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| 有幫忙打包不須設定環境變數      | **解壓縮後須設定環境變數**                                   |
| sudo apt install openjdk-11-jdk | wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz |



------



## **3️⃣ Tomcat 目錄結構**

**常用的目錄：**

```
tomcat/
 ├── bin/           # 啟動與停止腳本 (startup.sh, shutdown.sh)
 ├── conf/          # 設定檔 (server.xml, web.xml)
 ├── logs/          # Log 檔
 ├── webapps/       # Web 應用程式 (放 JSP / Servlet)
 │    ├── ROOT/     # 預設應用程式
 │    ├── myapp/    # 你自己的應用程式 (hello.jsp 放這裡)
 └── work/          # 編譯後的 Servlet 類別
```



**webapps裡面檔案比較**

| **裡面的其他資料夾**                   | **ROOT 資料夾**                              |
| -------------------------------------- | -------------------------------------------- |
| **檔案須打前面的資料夾才可存取並連網** | **檔案不須資料夾不用額外資料夾存取直接連網** |
| **webapps/myapp/oo.jsp**               | **webapps/root/oo.jsp**                      |
| **https://example.com/myapp/oo.jsp**   | **https://example.com/oo.jsp**               |



------



## **4️⃣ 部署 JSP 測試**

- 部署 ➨自動化

**在 webapps/myapp/ 建立 hello.jsp:**



```java
<html>
<body>
  <h1>Hello JSP with Tomcat!</h1>
  <p>現在時間是：<%= new java.util.Date() %></p>
</body>
</html>
```





1. 

**瀏覽器輸入：**

****
```
http://localhost:8080/myapp/hello.jsp
```

2. **✅ 看到頁面就成功了！**



# **📂 JSP / Servlet 專案目錄結構**

假設你的專案名稱叫 **myapp**，在 tomcat/webapps/ 底下建立：

```
myapp/                       ← 專案根目錄
 ├── index.jsp               ← JSP 預設首頁
 ├── hello.jsp               ← 測試用 JSP
 ├── WEB-INF/                ← Web 應用程式的核心目錄（使用者不能直接訪問）
 │    ├── web.xml            ← Web 應用程式的部署描述檔 (必須有)
 │    ├── classes/           ← 編譯後的 .class 檔（Servlet、Java 程式）
 │    └── lib/               ← 專案需要的第三方 JAR (例如 JDBC Driver)
```





------



## **1️⃣ JSP 放置位置**

- 放在 myapp/ 底下（例如 index.jsp, hello.jsp）。

  

瀏覽器可直接訪問：



 http://localhost:8080/myapp/hello.jsp



------



## **2️⃣ Servlet / Java 程式**

- Java 程式編譯後放在 WEB-INF/classes/


- 用eclipse
- 把eclipse-workspace放方便的地方

1. windows->prefrences->server->Routine Envirement->Add…->apache->apache tomcatV9.0->

- name:Apache Tomcat v9.0 
- tomcat installationdirectory:/home/uriel/文件/apache-tomcat-9.0.95(需選方便的位址)
- 按download and install -> i accept ->finish
- JRE:Java-11-openjdk-amd64

2. file->new->dynamic web project->

- project name :myapp
- Target Luntine:Apache Tomcat v9.0 
- dynamic web module version:4.0

->next->next->打勾 Grenerate web.xml deployment descriptor

3. 檢查myapp properties的設定（dynamic web module,java,javascript version）

4. 創myapp/Java resource/src/main/java/com.example.serlvet/HelloServlet.java

- myapp->java resources->src/main/java->New->class->

  - **package:com.example.servlet**

  - **Name:HelloServlet**

​	->finish

- HelloServlet.class(tomcat9)

```java
package com.example.servlet;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.setContentType("text/html;charset=UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<h1>Hello Servlet!</h1>");
    }
}
```





 編譯後的 .class 檔要放在：

 myapp/WEB-INF/classes/com/example/servlet/HelloServlet.**class**



------



## **3️⃣ web.xml（部署描述檔）**

放在 WEB-INF/，告訴 Tomcat 你的 Servlet 如何對應 URL：

- myapp/src/main/webapp/WEB-INF/web.xml


```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" version="3.1">
  
  <!-- 註冊一個 Servlet -->
  <servlet>
    <servlet-name>helloServlet</servlet-name>
    <servlet-class>com.example.servlet.HelloServlet</servlet-class>
  </servlet>

  <!-- 設定 URL 對應 -->
  <servlet-mapping>
    <servlet-name>helloServlet</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>

  <!-- 預設首頁 -->
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```



👉 這樣在瀏覽器輸入：

http://localhost:8080/myapp/hello

就會執行 HelloServlet。



------



## **4️⃣ lib/ (可選)**

- 如果要連資料庫（MySQL、PostgreSQL 等），要把 JDBC driver JAR 放到這裡。

  

例如：

 myapp/WEB-INF/lib/mysql-connector-j-9.0.0.jar



------



✅ 總結：

- **JSP** → 放 myapp/ 根目錄（使用者可直接訪問）。

  

- **Servlet .class** → 放 WEB-INF/classes/（Tomcat 會自動載入）。

  

- **第三方 JAR** → 放 WEB-INF/lib/。

  

- **web.xml** → 控制專案設定、Servlet 對應 URL

# **🚀 部署 JSP 頁面流程（Tomcat）**

假設專案名稱為 myapp，目標是部署一個 hello.jsp 頁面。



------



## **1️⃣ 專案結構**

放在 Tomcat 的 webapps/ 底下：

```
tomcat/
 └── webapps/
     └── myapp/
         ├── hello.jsp
         └── WEB-INF/
             └── web.xml  (可選，沒有也可運行 JSP)
```



**hello.jsp 範例**：

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>Hello JSP</title>
</head>
<body>
    <h1>Hello JSP!</h1>
    <p>現在時間是： <%= new java.util.Date() %></p>
</body>
</html>
```





------



## **2️⃣ 啟動 Tomcat**

1. 進入 Tomcat bin 目錄：

   

   - Windows：startup.bat

     

   - Linux/macOS：./startup.sh

     

2. 確認啟動成功：

   

瀏覽器輸入：

- http://localhost:8080/

- 出現 Tomcat 預設首頁表示成功。

  



------



## **3️⃣ 部署 JSP**

- 部署 ➨自動化

方式很簡單：

1. 將 myapp 資料夾直接放入 webapps/ 目錄。

   

2. Tomcat 會自動偵測並部署，第一次訪問 JSP 時：

   

   - JSP 會被 **翻譯成 Servlet**

     

   - 編譯成 .class

     

   - 載入 JVM 執行

     



------



## **4️⃣ 訪問 JSP**

在瀏覽器輸入：

http://localhost:8080/myapp/hello.jsp

✅ 頁面會顯示：

Hello JSP!

現在時間是：Fri Sep 26 12:34:56 CST 2025



------



## **5️⃣ 注意事項**

1. **JSP 放置位置**：

   

   - 直接放在 myapp/ 根目錄，使用者可以直接訪問。

     

   - 如果放在 WEB-INF/ 下，使用者無法直接訪問（適合用作 include 或保護頁面）。

     

2. **更新 JSP**：

   

   - 修改 JSP 後，Tomcat 會自動重新編譯，再次訪問即可看到變更。

     

3. **Servlet & JSP 同時使用**：

   

   - Servlet 放 WEB-INF/classes/

     

   - JSP 放根目錄

     

   - 用 web.xml 或註解設定 URL 映射。

     

# **第三章：**JSP 指令（Directives）

JSP 指令用來 **設定 JSP 編譯器或容器的行為**，它不會直接輸出到瀏覽器，而是影響頁面如何被翻譯成 Servlet。

### **語法範例：**

```java
<%@ directive attribute="value" %>
```





------



## **1️⃣** **<%@ page %>** **— 設定頁面屬性**

**用途**：設定 JSP 頁面屬性，例如編碼、內容類型、錯誤頁面等。

- 這四個名詞（ASCII、UTF-8、Big5、MS950）都是**文字編碼系統（Character Encoding）**，用來把「字元」轉成電腦可以儲存與傳輸的「位元組（bytes）」。
- 編碼很重要



## **✅ 總體比較表**

| **項目**         | **ASCII**      | **UTF-8**             | **Big5**              | **MS950**                     |
| ---------------- | -------------- | --------------------- | --------------------- | ----------------------------- |
| 🎯 主要用途       | 英文           | 全球語言（Unicode）   | 繁體中文              | 繁體中文（Windows）           |
| 📦 編碼範圍       | 7-bit（0–127） | 可變長度：1–4 bytes   | 雙位元組（1–2 bytes） | Big5 延伸版                   |
| 🈳 中文支援       | ❌ 不支援       | ✅ 完整支援            | ✅ 支援部分繁體字      | ✅ 支援更多繁體字（Big5 擴充） |
| 🌍 相容性         | 最廣泛         | 現代主流              | 台灣早期使用          | 台灣 Windows 專用             |
| 🏛 標準制定       | ANSI X3.4      | Unicode Consortium    | 台灣教育部 + 廠商     | Microsoft                     |
| 📁 常見檔案副檔名 | .txt           | .txt, .html, .json 等 | .txt, .html（舊）     | .txt, .html（Windows 舊版）   |



## **🔍 各編碼系統詳細說明**

### **1. ASCII（美國資訊交換標準碼）**

- **位元數：** 7 bits（可表示 128 個字元）

  

- **包含內容：**

  

  - 英文字母 A–Z, a–z

    

  - 數字 0–9

    

  - 基本符號（例如 !@#$%^&*()）

    

  - 控制字元（例如換行 LF、回車 CR）

    

- ✅ 最簡單，廣泛相容

  

- ❌ **不支援中文或其他非英語語系字元**

  

### **2. UTF-8（Unicode Transformation Format 8-bit）**

- **位元數：** 可變長度（1～4 bytes）

  

- **特點：**

  

  - ASCII 字元（0–127）只佔 1 byte（與 ASCII 相容）

    

  - 中文通常佔 3 bytes

    

- ✅ 支援全球所有語言（Unicode 全字符集）

  

- ✅ 網頁與現代程式語言的預設編碼（如 HTML、Python）

  

- ✅ 跨平台相容性極佳

  

- ❌ 檔案大小稍大（比 Big5 稍大）

  

### **3. Big5**

- **位元數：** 1–2 bytes（主要是 2 bytes）

  

- **由來：** 台灣早期標準，由中華民國教育部與廠商合作制訂

  

- **支援內容：**

  

  - 常用繁體中文字（13,000 多個）

    

- ✅ 台灣早期網站與系統常用

  

- ❌ 不支援所有 Unicode 字元

  

- ❌ 不支援簡體中文

  

- ❌ 有些冷僻字缺失（例如人名、地名）

  

### **4. MS950**

- **位元數：** 和 Big5 相同（雙位元組為主）

  

- **由來：** Microsoft 為 Windows 系統設計的 Big5 延伸版

  

- **特點：**

  - 基於 Big5 編碼

    

  - 加入更多繁體字與符號

    

  - **支援造字區**

    

- ✅ 台灣 Windows 系統中的預設繁體中文編碼

  

- ✅ 能處理比 Big5 更多的字

  

- ❌ 非正式標準，跨平台支援不穩定

  

- ❌ 相容性不如 UTF-8




## **✅ 建議使用時機**

| **使用情境**          | **建議編碼**             |
| --------------------- | ------------------------ |
| 現代網站或 app        | ✅ UTF-8                  |
| 舊系統或 Windows 軟體 | ✅ MS950 / Big5（視需求） |
| 僅限英文處理          | ✅ ASCII                  |
| 涉及多語言混用        | ✅ UTF-8（最佳選擇）      |



-------------------------



### **常用屬性：**

| **屬性**    | **說明**                                         |
| ----------- | ------------------------------------------------ |
| contentType | 設定輸出 MIME 類型，例如 text/html;charset=UTF-8 |
| language    | 預設 Java (java)                                 |
| import      | 匯入 Java 類別或套件，例如 java.util.*           |
| errorPage   | 發生錯誤時跳轉的頁面                             |
| isErrorPage | 設定該頁面是否作為錯誤頁面使用                   |



### **範例：**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" import="java.util.*" %>

<html>
<body>
<p>現在時間：<%= new Date() %></p>
</body>
</html>
```





------



## **2️⃣** **<%@ include %>** **— 靜態包含**

**用途**：將另一個檔案的內容 **在編譯時直接插入** JSP。

- 編譯前就合併 → **靜態包含**

  

- 適合放共用頁面（如 header/footer）

  

### **語法：**

```java
<%@ include file="header.jsp" %>
```



### **範例：**

**header.jsp**

```java
<div style="background-color: #ddd; padding: 10px;">
    <h1>我的網站標題</h1>
</div>
```



**index.jsp**

```java
<%@ include file="header.jsp" %>
<p>首頁內容</p>
```

瀏覽器結果會自動顯示 header + index 內容



------



## **3️⃣** **<%@ taglib %>** **— 引用標籤庫**

**用途**：使用 JSP 標籤庫（如 JSTL），簡化流程控制、格式化、資料庫操作等。

### **語法：**

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

- prefix → 標籤前綴，之後用 <c:forEach> 等方式呼叫

  

- uri → 標籤庫的唯一識別

  

- JSTL 是最常用的標準標籤庫（Core, Formatting, SQL, XML）

  

### **範例：**

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>//不建議使用

<ul>
<c:forEach var="item" items="${list}">
    <li>${item}</li>
</c:forEach>
</ul>
```



list 來自 Servlet 或 JSP 設定的屬性，標籤庫會自動處理迴圈。



------



## **🔑 小結**

| **指令**       | **用途**                     | **特點**                                   |
| -------------- | ---------------------------- | ------------------------------------------ |
| <%@ page %>    | 設定 JSP 頁面屬性            | 影響編譯器、輸出格式、例外處理             |
| <%@ include %> | 靜態包含其他 JSP / HTML      | 編譯時合併，適合共用頁面                   |
| <%@ taglib %>  | 引入標籤庫（JSTL、自訂標籤） | 用於流程控制、迴圈、格式化，減少 Scriptlet |

# **📌 JSP 宣告（Declarations）**

### **1️⃣ 語法**

<%! Java 程式碼 %>

- 用於 **在 JSP 生成的 Servlet 中宣告成員變數或方法**

  - **Servlet 本質上是一個「Java 類別」**

- <%! Java 程式碼 %> 的！不要省略

- 這些變數或方法會 **成為 Servlet 類別的成員**，可被 JSP 頁面內的其他程式碼呼叫

  

##  **✅** **Servlet 成員變數/方法 vs OOP：對照總表**

| **項目**             | **OOP 中的意義**         | **在 Servlet 中的實踐**                    | **注意事項**                           |
| -------------------- | ------------------------ | ------------------------------------------ | -------------------------------------- |
| **成員變數（欄位）** | **封裝狀態**             | **保存共享資料（如服務、資源）**           | **必須處理執行緒安全**                 |
| **成員方法**         | **封裝行為、模組化邏輯** | **實作商業邏輯、工具方法**                 | **可拆分成獨立類別（服務層）**         |
| **繼承**             | **重用功能、擴充行為**   | **繼承** **HttpServlet**                   | **覆寫** **doGet()****,** **doPost()** |
| **多型**             | **相同介面，不同實作**   | **使用介面（如** **GreetingService****）** | **提高擴充性與測試性**                 |
| **責任分離**         | **單一職責原則**         | **把邏輯搬到 Service 類別**                | **建立清楚分層架構（MVC）**            |

------



### **2️⃣ 與其他 JSP 程式碼區別**

| **JSP 區塊** | **說明**         | **放置內容**                               |
| ------------ | ---------------- | ------------------------------------------ |
| <%! %>       | **Declarations** | 宣告成員變數、方法                         |
| <% %>        | **Scriptlet**    | Java 程式碼，可在 _jspService() 方法中執行 |
| <%= %>       | **Expression**   | 計算結果直接輸出到頁面                     |

⚡ 重點：

- <%! %> → **成員級別**（Servlet class）

  

- <% %> → **方法級別**（_jspService() 方法裡）

  

- <%= %> → **輸出值**

  

| **Scriptlet**        | **Expression**                                               |
| -------------------- | ------------------------------------------------------------ |
| **需要告訴它要顯示** | **Expression 直接顯示**                                      |
| X                    | **不建議在 JSP 中使用成員變數** ，**因為是共用** ，**預設是多執行緒** |



------



### **3️⃣ 範例：宣告變數與方法**



```java
<%! 
    // 成員變數
    int counter = 0;

    // 成員方法
    public String getGreeting(String name) {
        return "Hello, " + name + "!";
    }
%>

<html>
<body>
<h1><%= getGreeting("Terry") %></h1>
<p>訪問次數：<%= ++counter %></p>
</body>
</html>
```



### **解釋：**

1. counter → 成為 Servlet 的成員變數，每次 JSP 生成的 Servlet 都有這個變數

   

2. getGreeting(String) → 成員方法，可在 <%= %> 或 <% %> 中呼叫

   

3. <%= getGreeting("Terry") %> → 呼叫方法，輸出結果到頁面

   

4. <%= ++counter %> → 修改成員變數，計算訪問次數

   - **在 Java 中，****++c** **是 前置遞增運算子，意思是：**

     ```java
     在 Java 中，++c 是 前置遞增運算子，意思是：
     int c = 5;
     int result = ++c; // c 先加 1，然後再將值賦給 result
     // result = 6, c = 6
     
     對比：
     int c = 5;
     int result = c++; // 先將 c 賦值給 result，再將 c 加 1
     // result = 5, c = 6
     ```

     

   


⚠️ 注意：

- 由於 JSP 的 Servlet 是 **多執行緒**，如果使用成員變數來計數，需要考慮 **同步問題**，否則不同使用者訪問可能干擾彼此。

  



------



### **4️⃣ 使用場景**

- 宣告 **共用方法**（在 JSP 中重複使用）

  

- 宣告 **全局變數或常數**（但要注意多執行緒安全）

  

- 對小型測試或教學示範很方便，但大型專案通常 **不建議在 JSP 中使用成員變數**，建議放在 Servlet 或 Java 類中

# 📌 JSP Scriptlet (<% %>)

### **1️⃣ 語法**

```java
<%

  // Java 程式碼片段

%>
```



- **Scriptlet 的程式碼會被放到 JSP 生成的 Servlet** **_jspService()** **方法中**

  

- **可以做 流程控制、計算、呼叫方法 等操作**

  



------



### **2️⃣ 與其他 JSP 元素比較**

| **JSP 區塊** | **用途**                                        | **範例**                    |
| ------------ | ----------------------------------------------- | --------------------------- |
| **<%! %>**   | **宣告成員變數 / 方法（Servlet class 層級）**   | **<%! int counter = 0; %>** |
| **<% %>**    | **Scriptlet，程式碼片段（_jspService 方法內）** | **<% counter++; %>**        |
| **<%= %>**   | **表達式，輸出結果到頁面**                      | **<%= counter %>**          |

**⚡ 重點：**

- **Scriptlet 可以讀寫** **<%! %>** **宣告的成員變數**

  

- **Scriptlet 不能放在類別成員層級，只能在方法內**

  



------



### **3️⃣ 範例：Scriptlet 使用**



```java
<%@ page contentType="text/html;charset=UTF-8" %>
<%! int counter = 0; %>

<html>
<body>
<%
    // Scriptlet 程式碼
    counter++;  // 每次訪問加 1
    String user = "Terry";
%>

<h1>Hello, <%= user %>!</h1>
<p>訪問次數：<%= counter %></p>

</body>
</html>
```





#### **解釋：**

1. **<%! int counter = 0; %>** **→ 宣告成員變數**

   

2. **<% counter++; String user = "Terry"; %>** **→ Scriptlet，執行邏輯**

   

3. **<%= user %>** **和** **<%= counter %>** **→ 輸出結果到 HTML**

   



------



### **4️⃣ Scriptlet 常見用途**

- **流程控制：**if, **for**, **while**

  

- **呼叫方法**

  

- **計算或資料處理**

  

- **與 Expression (**<%= %>) 搭配輸出結果

  



------



### **5️⃣ 注意事項**

1. **多執行緒問題**

   

   - **JSP Servlet 預設是多執行緒，一個成員變數被不同使用者同時讀寫，可能產生 race condition**

     

   - **大型專案建議 不要在 JSP Scriptlet 中使用成員變數，把邏輯放到 Servlet 或 Java 類中**

     

2. **可讀性與維護性**

   

   - **Scriptlet 會混合 Java 與 HTML，專案變大後難維護**

     

   - **現代 JSP 開發建議使用 JSTL + EL 表達式，減少 Scriptlet**

     



------



### **6️⃣ 範例：流程控制**



```java
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<body>
<ul>
<%
    String[] fruits = {"Apple", "Banana", "Cherry"};
    for(int i=0; i<fruits.length; i++) {
%>
    <li><%= fruits[i] %></li>
<%
    }
%>
</ul>
</body>
</html>
```





# **📌 JSP 表達式（Expression** **<%= %>**）

### **1️⃣ 語法**

<%= Java 表達式 %>

- 會 **直接將運算結果輸出到 HTML 頁面**

  

- 表達式內可以使用任何有效的 Java 變數、方法呼叫或運算式

  

- JSP 生成的 Servlet 會把 <%= %> 的內容放到 _jspService() 方法中，並透過 out.print() 輸出

  



------



### **2️⃣ 與其他 JSP 元素比較**

| **JSP 區塊** | **用途**              | **放置位置**       | **輸出**                     |
| ------------ | --------------------- | ------------------ | ---------------------------- |
| <%! %>       | 宣告成員變數 / 方法   | Servlet 類別層級   | 不輸出                       |
| <% %>        | Scriptlet，程式碼片段 | _jspService() 方法 | 不輸出（除非用 out.print()） |
| <%= %>       | 表達式，輸出結果      | _jspService() 方法 | 會輸出到 HTML 頁面           |

⚡ 重點：

- <%= %> 只用來輸出結果，不適合放邏輯運算

  

- 可以搭配 Scriptlet 或 Declarations 使用

  



------



### **3️⃣ 範例 1：輸出文字**

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<body>
<h1>當前使用者：<%= "Terry" %></h1>
</body>
</html>
```



輸出結果：
 當前使用者：Terry



------



### **4️⃣ 範例 2：輸出變數與方法**

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<%! 
    int counter = 0;
    public String greet(String name) {
        return "Hello, " + name + "!";
    }
%>

<%
    counter++;
    String user = "Terry";
%>

<h1><%= greet(user) %></h1>
<p>訪問次數：<%= counter %></p>
```



#### **解釋：**

1. <%! %> → 宣告成員變數 counter 和方法 greet()

   

2. <% %> → Scriptlet，修改 counter 和設定變數 user

   

3. <%= %> → 將方法結果與變數直接輸出到頁面

   

輸出結果：

Hello, Terry!

訪問次數：1



------



### **5️⃣ 範例 3：表達式運算**

<p>5 + 3 = <%= 5 + 3 %></p>

<p>今天日期：<%= new java.util.Date() %></p>

- <%= 5 + 3 %> → 直接計算並輸出 8

  

- <%= new java.util.Date() %> → 輸出伺服器當前時間

  



------



### **6️⃣ 注意事項**

1. **只用於輸出結果**

   

   - 不建議在 <%= %> 做太複雜的邏輯或流程控制

     

2. **搭配 Scriptlet / Declarations****

   

   - 可以輸出成員變數或方法結果

     

3. **現代 JSP 開發**

   

   - 推薦使用 **EL (Expression Language)**

     

   - EL 可以直接使用 ${} 取得屬性，比 <%= %> 更安全、簡潔

   - Linux F12 是在 **Firefox / Chrome**，這就是開發者工具。

4. ### **Redirect（重導向）跟 Forward （轉發）：**

   - **Forward 是在 server 端內部轉發，URL 不會變，請求和響應物件是同一個。**

     

   - **Redirect 是瀏覽器接收到響應後，再向新的 URL 發送請求，會有兩個請求。**

   ## **簡單比較**

   - | **特性**             | **Redirect (重導向)**          | **Forward (轉發)**                      |
     | -------------------- | ------------------------------ | --------------------------------------- |
     | **URL 是否變化**     | **會變化（瀏覽器地址列更新）** | **不變（瀏覽器地址列不變）**            |
     | **請求是否重新發送** | **會重新發送新的請求**         | **只在伺服器內部傳遞請求**              |
     | **使用場景**         | **網頁搬家、跳轉外部網站**     | **內部資源調用、轉交給 JSP 或 Servlet** |
     | **客戶端是否知曉**   | **知曉**                       | **不知曉**                              |

   5.補充:

- **如果有遇到有寫好程式用錯 不要幫它釘正 debug有問題的地方就好**
- MIME（**Multipurpose Internet Mail Extensions，多用途網際郵件延伸協定**）最早是為了在電子郵件中傳送非純文字內容（例如圖片、聲音、影片、附件）而設計的，但它的概念後來也廣泛應用在 **HTTP、Web API、檔案上傳** 等地方。
  1. MIME 在電子郵件中的用法
     - 電子郵件原本只能傳送 ASCII 純文字，MIME 擴充後能傳送：
       - 二進位檔案（圖片、壓縮檔、程式）
       - 多國語言（UTF-8 編碼）
       - 多媒體內容（音訊、影像）
  2. MIME 在 HTTP/網頁中的用法
     - MIME Type（又稱 **Content-Type**）用來告訴瀏覽器或應用程式「這個資料的型態」。
  3. MIME 在檔案上傳（Web API）
     - 每個 `boundary` 區塊代表一個表單欄位或檔案。
       - 例如使用 `curl` 上傳檔案：
  4. 常見應用
     - **Email** → 傳送附件、多國語言內容
     - **Web server** → 正確設定檔案 MIME Type，讓瀏覽器能正確顯示或下載
     - **API** → 指定 JSON、XML、圖片、影片等格式
     - **檔案上傳** → 使用 `multipart/form-data`

# **第四章： JSP 內建物件（Implicit Objects）**

JSP 提供了 **9 個常用內建物件**，用來方便操作請求、回應、Session 等功能，而 **不需要額外宣告或初始化**。

| **物件**    | **類型**            | **說明**                                           |
| ----------- | ------------------- | -------------------------------------------------- |
| request     | HttpServletRequest  | 封裝 HTTP 請求資料，例如表單提交、URL 參數、Header |
| response    | HttpServletResponse | 封裝 HTTP 回應資料，設定 MIME、輸出內容、Cookie 等 |
| out         | JspWriter           | 輸出內容到瀏覽器（類似 System.out）                |
| session     | HttpSession         | 與特定使用者相關的會話資料(**控制時間**)           |
| application | ServletContext      | 全域應用程式範圍資料                               |
| config      | ServletConfig       | Servlet 配置資訊                                   |
| pageContext | PageContext         | JSP 頁面範圍內的所有物件封裝                       |
| page        | Object              | 目前 JSP 頁面本身的參考（this）                    |
| exception   | Throwable           | 當頁面作為錯誤頁面時，用來取得例外物件             |

- [**OSI模型**]([**https://claire-chang.com/2022/08/01/%E7%B6%B2%E8%B7%AF%E6%A6%82%E5%BF%B5%E6%A8%A1%E5%9E%8B%E4%BB%8B%E7%B4%B9/**](https://claire-chang.com/2022/08/01/網路概念模型介紹/)

------

![voH73Eu.webp](D:\sss\java\voH73Eu.webp)

- **都有通訊協定**
- **大部分在第五層**
- **偶爾寫第三層第四層**

##  1. 實體層（Physical Layer）

- **功能**：傳輸 **實體訊號**（0/1 bit 電壓、光訊號、無線電波）
- **硬體相關**：網路線、集線器（Hub）、網路卡的實體介面
- **協定/技術範例**：RJ-45、Ethernet 物理規範、光纖規範

------

## 2. 資料鏈結層（Data Link Layer）

- **功能**：把實體訊號轉換成 **資料幀（Frame）**，並提供錯誤偵測、媒體存取控制（MAC）
- **設備**：交換器（Switch）、橋接器（Bridge）
- **協定範例**：
  - Ethernet (IEEE 802.3)
  - PPP（Point-to-Point Protocol）
  - ARP（Address Resolution Protocol）
- **地址型態**：**MAC 位址**（硬體地址）

------

## 3. 網路層（Network Layer）

- **功能**：決定資料如何在網路中**尋址與轉送**（Routing）
- **設備**：路由器（Router）
- **協定範例**：
  - IP（IPv4/IPv6）
  - ICMP（Ping）
  - OSPF、BGP（路由協定）
- **地址型態**：**IP 位址**

------

## 4. 傳輸層（Transport Layer）

- **功能**：提供**端對端傳輸**、分段與重組、流量控制、可靠傳輸
- **協定範例**：
  - TCP（可靠，連線導向）
  - UDP（不可靠，無連線）
- **識別方式**：**Port（埠號）**

------

## 5. 會議層（Session Layer）

- **功能**：建立、管理與終止應用之間的**會話（Session）**
- **應用範例**：
  - RPC（Remote Procedure Call）
  - NetBIOS
  - TLS/SSL 的部分功能（在傳輸層與應用層之間）

------

## 6. 表達層（Presentation Layer）

- **功能**：資料的轉換、壓縮、加密/解密
- **範例**：
  - JPEG、GIF（影像格式）
  - MP3、MP4（媒體格式）
  - SSL/TLS（加密）
  - 編碼轉換（ASCII ↔ Unicode）

------

## 7. 應用層（Application Layer）

- **功能**：直接面向使用者，提供網路應用服務
- **協定範例**：
  - HTTP/HTTPS（網頁）
  - FTP（檔案傳輸）
  - SMTP/POP3/IMAP（電子郵件）
  - DNS（網域名稱解析）
  - SSH、Telnet（遠端連線）

------

# 📊 OSI 七層模型對照表（速查）

| 層級 | 名稱       | 功能           | 協定 / 範例          | 對應設備        |
| ---- | ---------- | -------------- | -------------------- | --------------- |
| 7    | 應用層     | 提供應用服務   | HTTP, FTP, SMTP, DNS | -               |
| 6    | 表達層     | 格式轉換、加密 | JPEG, MP3, SSL/TLS   | -               |
| 5    | 會議層     | 建立/管理會話  | NetBIOS, RPC         | -               |
| 4    | 傳輸層     | 可靠傳輸、分段 | TCP, UDP             | 防火牆          |
| 3    | 網路層     | 尋址與路由     | IP, ICMP, OSPF, BGP  | Router          |
| 2    | 資料鏈結層 | 成幀、MAC 管理 | Ethernet, ARP, PPP   | Switch, Bridge  |
| 1    | 實體層     | 傳輸比特流     | RJ-45, 光纖, Wi-Fi   | Hub, 網卡, 線材 |

## **1️⃣** **request** **物件**

- 類型：javax.servlet.http.HttpServletRequest

  

- 用途：**取得瀏覽器端送來的請求資料**（GET/POST 表單資料、URL 參數、Header 等）

  



------



### **2️⃣ 常用方法**

| **方法**                                | **說明**                     |
| --------------------------------------- | ---------------------------- |
| getParameter(String name)               | 取得表單或 URL 參數值        |
| getParameterValues(String name)         | 取得同名多個參數的值（陣列） |
| getAttribute(String name)               | 取得伺服器端設定的屬性       |
| setAttribute(String name, Object value) | 在 request 範圍內設定屬性    |
| getMethod()                             | 取得 HTTP 方法（GET, POST）  |
| getRequestURI()                         | 取得請求的 URI               |
| getHeader(String name)                  | 取得 HTTP Header             |



------



### **3️⃣ 範例 1：取得表單參數**

**form.html**

```java
<form action="hello.jsp" method="post">
    姓名：<input type="text" name="username">
    <input type="submit" value="送出">
</form>
```



**hello.jsp**

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<%
    String name = request.getParameter("username");  // 取得表單參數
%>
<html>
<body>
<p>你好，<%= name %>!</p>
</body>
</html>
```



- 使用者輸入 Terry → 輸出 你好，Terry!

  



------



### **4️⃣ 範例 2：設定與取得 request 屬性**

**先在 Servlet 設定屬性：**

```java
request.setAttribute("greeting", "Hello JSP!");
request.getRequestDispatcher("/show.jsp").forward(request, response);
```





**show.jsp**

```java
<p>訊息：<%= request.getAttribute("greeting") %></p>
```



<p>訊息：<%= request.getAttribute("greeting") %></p>

輸出結果：
 訊息：Hello JSP!



------



### **5️⃣ 小結**

- request 是 **JSP 與使用者請求之間的橋樑**

  

- 用途：

  

  - 取得表單、URL 或 Header 資料

    

  - 設定與取得 request 範圍內的屬性

    

- 注意：

  

  - request 物件只在單次請求有效，請不要存放長期資料（Session 或 Application 適合長期資料）

# **第五章：** **JSP 與 JavaBean**

### **1️⃣ JavaBean 概念**

JavaBean 是 **可重複使用的 Java 類別**，遵循一定規範，可讓 JSP 頁面透過標籤存取資料與方法，實現 **資料與邏輯分離**。



------



### **2️⃣ JavaBean 規範**

**有 public 的無參構造函式**
****

```java
 public class User {
    public User() {}  // 必須有無參建構子
}
```



- **public** **User**() {} 如果沒被加 compiler會幫你加無參建構子

1. 

**屬性私有，提供 getter / setter 方法**

****


```java
private String name;

public String getName() { return name; }
public void setName(String name) { this.name = name; }
```





2. **可序列化（實務中常加 implements Serializable）**



3. **不含大量業務邏輯**（建議只存放屬性與簡單方法）



4. Variable（變數）

## **Java 中四種變數的比較表**

| **類型**           | **定義位置**      | **存活時間（生命週期）** | **使用範圍（作用域）** | **關鍵字** | **是否有預設值** | **需要手動初始化** |
| ------------------ | ----------------- | ------------------------ | ---------------------- | ---------- | ---------------- | ------------------ |
| **Block**          | 區塊 {} 裡        | 區塊執行期間             | 區塊內                 | 無         | ❌ 沒有           | ✅ 是               |
| **Local**          | 方法或建構子裡    | 方法執行期間             | 方法內                 | 無         | ❌ 沒有           | ✅ 是               |
| **Instance**       | 類別中，方法外    | 物件活著期間             | 整個物件               | 無         | ✅ 有             | ❌ 否               |
| **Static (Class)** | 類別中，加 static | 整個程式期間             | 整個類別（共享）       | static     | ✅ 有             | ❌ 否               |

## **重點解析**

### **🔹 Block 變數**

- 在大括號 {} 中定義，像 if, for, while 區塊

  

- **只能在區塊內使用**

  

- 區塊結束就會銷毀

  

- 一種特別狹窄的 **local 變數**

  

### **🔸 Local 變數**

- 在方法或建構子中定義

  

- **執行方法時建立，結束就消失**

  

- **必須初始化，否則不能使用**

  

- 不能加 public、private 等修飾詞

  

### **🔹 Instance 變數**

- 宣告在類別中，但不加 static

  

- **每個物件都有自己的副本**

  

- 可以設定 public、private 修飾詞

  

- 有預設值（數字是 0，布林是 false，物件是 null）

- 屬於物件本身，用 this.變數名 存取

  

### **🔸 Static (Class) 變數**

- 宣告在類別中，加上 static

  

- **屬於類別，不屬於物件**

  

- 所有物件 **共享** 一份

  

- 通常用於統計類資料，例如計算所有物件數量

  



------



## **📌 使用場景建議**

| **目的 / 狀況**                      | **建議使用變數** |
| ------------------------------------ | ---------------- |
| 只在某個區塊內暫時存值               | block            |
| 一個方法內處理資料                   | local            |
| 每個物件要有自己的資料               | instance         |
| 所有物件共享資料（例如：總計、設定） | static           |



------



## **✅ 小結（一句話記住）**

- Block：括號內的小變數

  

- Local：方法內的小兵

  

- Instance：每個物件自己的變數

  

- Static：全班共用的變數



------



### **3️⃣ JSP 與 JavaBean 標籤**

#### **①** **<jsp:useBean>**

- 作用：在 JSP 中 **建立或引用 JavaBean 物件**

- 有無參數建構子、private 變數 + getter/setter 標準

  

- 常用屬性：

  

  - id → JSP 內部變數名稱

    

  - class → Bean 類別全名

    

  - scope → 物件存放範圍（page, request, session, application）

    

範例：

```java
<jsp:useBean id="user" class="com.example.User" scope="session" />
```





------



#### **②** **<jsp:setProperty>**

- 作用：設定 Bean 屬性值

  

- 可從 request 參數自動綁定

  

範例：

```java
<!-- 設定單一屬性 -->
<jsp:setProperty name="user" property="name" value="Terry" />

<!-- 自動從 request 參數綁定 -->
<jsp:setProperty name="user" property="*" />
```



​	property="*" → 會自動把 request 裡的同名參數設定到 Bean 屬性



------



#### **③** **<jsp:getProperty>**

- 作用：取得 Bean 屬性值並輸出到頁面

  

範例：

```java
<p>使用者姓名：<jsp:getProperty name="user" property="name" /></p>
```





------



### **4️⃣ JSP + JavaBean 範例（完整）**

**User.java**

```java
package com.example;

public class User {
    private String name;
    private int age;

    public User() {}

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```





**form.html**

```java
<form action="showUser.jsp" method="post">
    姓名：<input type="text" name="name">
    年齡：<input type="text" name="age">
    <input type="submit" value="送出">
</form>
```



**showUser.jsp**

```java
<%@ page contentType="text/html;charset=UTF-8" %>
<jsp:useBean id="user" class="com.example.User" scope="request" />
<jsp:setProperty name="user" property="*" />

<html>
<body>
<h1>使用者資料</h1>
<p>姓名：<jsp:getProperty name="user" property="name" /></p>
<p>年齡：<jsp:getProperty name="user" property="age" /></p>
</body>
</html>
```



- 表單提交資料 → request → 自動設定到 Bean → JSP 頁面顯示

  



------



### **5️⃣ JSP 與 MVC 架構**

- **MVC（Model-View-Controller）設計模式**：

  

  - **Model** → 資料與邏輯（JavaBean）

    

  - **View** → 顯示頁面（JSP）

    

  - **Controller** → 控制流程（Servlet，處理表單、設定 Bean、導向 JSP）

    

#### **角色對應**

| **MVC 元件** | **JSP/Java 角色**                            |
| ------------ | -------------------------------------------- |
| Model        | JavaBean / POJO                              |
| View         | JSP 頁面                                     |
| Controller   | Servlet（接收請求，操作 Bean，再轉發到 JSP） |



| JavaBean                                          | POJO                               |
| ------------------------------------------------- | ---------------------------------- |
| 有無參數建構子、private 變數 + getter/setter 標準 | 簡單 Java 物件，沒有特殊依賴或規範 |

補充:

## **JSON vs JavaBean 對照表**

| **比較項目** | **JSON**                | **JavaBean**                |
| ------------ | ----------------------- | --------------------------- |
| 本質         | 資料格式（文字）        | 類別設計模式（Java 物件）   |
| 用途         | 傳輸、儲存資料          | 封裝資料、操作邏輯          |
| 表示形式     | 字串（通常為 .json 檔） | Java 類別                   |
| 是否可程式化 | ❌（不能執行程式邏輯）   | ✅（可以有方法、邏輯）       |
| 語言限定     | 無                      | Java 專用                   |
| 互轉         | 可以透過工具互轉        | 需用套件如 Jackson、Gson 等 |

## **JavaBean歷史**

| **名稱**                   | **說明**                                          |
| -------------------------- | ------------------------------------------------- |
| **普通 Java 類別（早期）** | 沒有標準 getter/setter 的類別                     |
| **JavaBean**               | 有無參數建構子、private 變數 + getter/setter 標準 |
| **POJO**                   | 簡單 Java 物件，沒有特殊依賴或規範                |

## **JSON歷史**

### **總結：**

| **名稱**              | **時間**   | **角色**           | **特點**                     |
| --------------------- | ---------- | ------------------ | ---------------------------- |
| XML                   | 1998 年    | 傳統資料交換格式   | 冗長、複雜但功能強大         |
| JavaScript 物件字面量 | 1990s 中期 | 內建語法表示資料   | 僅限 JS 語言，非標準交換格式 |
| **JSON**              | 2001 年    | 輕量級資料交換格式 | 簡潔、跨語言、易解析         |

#### OOP物件導向程式設計

## **總結圖解**

| **概念** | **意義**             | **目的**             |
| -------- | -------------------- | -------------------- |
| 封裝     | 隱藏細節，只提供介面 | 保護資料，減少耦合   |
| 繼承     | 類別之間的父子關係   | 程式碼重用           |
| 多型     | 同一行為不同實現     | 靈活設計、擴充功能   |
| 抽象     | 聚焦共通行為與特性   | 降低複雜度，增加彈性 |

#### **MVC 範例流程**

1. 使用者提交表單 → **Servlet (Controller)**

   

2. Servlet 取得 request 參數，操作 JavaBean → 設定屬性

   

3. Servlet 轉發到 JSP → **JSP 透過** **<jsp:getProperty>** **顯示 Bean 資料**

4. 為什麼用 MVC？

   - **分離責任**：讓程式碼更有組織、易維護

     - JavaBean + JSP 可以 **達到資料與畫面分離**
     - 避免全都用jsp會很難DEBEG

     

   - **提高可重用性**：View 與 Model 分離，可以換 UI 不改資料邏輯

     

   - **團隊合作方便**：前端設計師和後端工程師可以各司其職

   - **擴充性好**：方便新增功能與測試




------



✅ 小結：

- JavaBean + JSP 可以 **達到資料與畫面分離**

  

- JSP 標籤 <jsp:useBean>、<jsp:setProperty>、<jsp:getProperty> 是標準做法

  

- MVC 架構能讓專案 **邏輯清楚、易於維護**

  

- 改到java(.class)需重啟但改jsp則不用重啟



# **第六章：JSP 與資料庫（JDBC）**

## **1️⃣ 環境準備**

1. 安裝 **MySQL** 或 **PostgreSQL**

   

   - MySQL 預設埠號 3306

     

   - PostgreSQL 預設埠號 5432

     

建立資料庫與範例資料表

 **MySQL**

****

```sql
CREATE DATABASE testdb;
USE testdb;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');
```



****

**PostgreSQL**

```sql
CREATE DATABASE testdb;
\c testdb;
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');
```



 



2. 下載並放置 JDBC Driver



- MySQL → mysql-connector-j.jar

  

- PostgreSQL → postgresql-xx.xx.jar
   放到 **Tomcat 的** **lib/** **目錄**。
   
- maven mysql-connector

  



------



## **2️⃣ JSP 使用 JDBC 連線 MySQL / PostgreSQL**

**db_connect.jsp**

```java
<%@ page import="java.sql.*" %>
<html>
<head><title>JDBC 測試</title></head>
<body>
<h2>JSP JDBC 連線測試</h2>

<%
    // 切換使用 MySQL 或 PostgreSQL
    String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
    String user = "root";        // MySQL 使用者
    String password = "123456";  // MySQL 密碼

    // PostgreSQL 範例
    // String url = "jdbc:postgresql://localhost:5432/testdb";
    // String user = "postgres";
    // String password = "123456";

    try {
        // MySQL Driver: com.mysql.cj.jdbc.Driver
        // PostgreSQL Driver: org.postgresql.Driver
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection conn = DriverManager.getConnection(url, user, password);
        out.println("<p>✅ 資料庫連線成功！</p>");
        conn.close();//避免爆滿
    } catch (Exception e) {
        out.println("<p>❌ 連線失敗：" + e.getMessage() + "</p>");
    }
%>
</body>
</html>
```





------



## **3️⃣ 查詢資料並輸出表格**

**db_query.jsp**

```java
<%@ page import="java.sql.*" %>
<html>
<head>
    <title>使用者清單</title>
</head>
<body>
<h2>使用者清單</h2>

<table border="1" cellpadding="5">
<tr><th>ID</th><th>Name</th><th>Email</th></tr>

<%
    String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
    String user = "root";
    String password = "123456";

    try {
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection conn = DriverManager.getConnection(url, user, password);

        Statement stmt = conn.createStatement();//不建議使用 會有sql rejection
        ResultSet rs = stmt.executeQuery("SELECT * FROM users");

        while (rs.next()) {
            out.println("<tr>");
            out.println("<td>" + rs.getInt("id") + "</td>");
            out.println("<td>" + rs.getString("name") + "</td>");
            out.println("<td>" + rs.getString("email") + "</td>");
            out.println("</tr>");
        }

        rs.close();
        stmt.close();
        conn.close();
    } catch (Exception e) {
        out.println("<tr><td colspan='3'>錯誤：" + e.getMessage() + "</td></tr>");
    }
%>

</table>
</body>
</html>
```





------



## **4️⃣ 插入資料範例**

**db_insert.jsp**

```java
<%@ page import="java.sql.*" %>
<html>
<head><title>新增使用者</title></head>
<body>
<h2>新增使用者</h2>

<%
    String name = request.getParameter("name");
    String email = request.getParameter("email");

    if (name != null && email != null) {
        try {
            String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
            String user = "root";
            String password = "123456";

            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection conn = DriverManager.getConnection(url, user, password);

            PreparedStatement ps = conn.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)");///建議使用
            ps.setString(1, name);
            ps.setString(2, email);
            ps.executeUpdate();

            ps.close();
            conn.close();
            out.println("<p> 已新增使用者：" + name + "</p>");
        } catch (Exception e) {
            out.println("<p> 錯誤：" + e.getMessage() + "</p>");
        }
    }
%>

<form method="post">
    姓名: <input type="text" name="name"><br>
    信箱: <input type="text" name="email"><br>
    <input type="submit" value="新增">
</form>
</body>
</html>
```





------



## **5️⃣ 更新資料範例**

**db_update.jsp**

```java
<%@ page import="java.sql.*" %>
<html>
<head><title>更新使用者</title></head>
<body>
<h2>更新使用者</h2>

<%
    String id = request.getParameter("id");
    String email = request.getParameter("email");

    if (id != null && email != null) {
        try {
            String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
            String user = "root";
            String password = "123456";

            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection conn = DriverManager.getConnection(url, user, password);

            PreparedStatement ps = conn.prepareStatement("UPDATE users SET email=? WHERE id=?");
            ps.setString(1, email);
            ps.setInt(2, Integer.parseInt(id));
            int rows = ps.executeUpdate();//回傳更新的筆數

            ps.close();
            conn.close();
            out.println("<p>✅ 已更新 " + rows + " 筆資料。</p>");
        } catch (Exception e) {
            out.println("<p>❌ 錯誤：" + e.getMessage() + "</p>");
        }
    }
%>

<form method="post">
    使用者ID: <input type="text" name="id"><br>
    新信箱: <input type="text" name="email"><br>
    <input type="submit" value="更新">
</form>
</body>
</html>
```



------



## **6️⃣ 刪除資料範例**

**db_delete.jsp**

```java
<%@ page import="java.sql.*" %>
<html>
<head><title>刪除使用者</title></head>
<body>
<h2>刪除使用者</h2>

<%
    String id = request.getParameter("id");

    if (id != null) {
        try {
            String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
            String user = "root";
            String password = "123456";

            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection conn = DriverManager.getConnection(url, user, password);

            PreparedStatement ps = conn.prepareStatement("DELETE FROM users WHERE id=?");
            ps.setInt(1, Integer.parseInt(id));
            int rows = ps.executeUpdate();

            ps.close();
            conn.close();
            out.println("<p>✅ 已刪除 " + rows + " 筆資料。</p>");
        } catch (Exception e) {
            out.println("<p>❌ 錯誤：" + e.getMessage() + "</p>");
        }
    }
%>

<form method="post">
    使用者ID: <input type="text" name="id"><br>
    <input type="submit" value="刪除">
</form>
</body>
</html>
```

補充:

- connection pool管理避免queue炸掉



------



# **第七章：JSP 與表單處理**

## **1️⃣ 取得表單參數：**request.getParameter()

### **範例：簡單登入表單**

```java
<html>
<head><title>登入頁面</title></head>
<body>
<h2>登入</h2>
<form action="login.jsp" method="post">
    帳號: <input type="text" name="username"><br>
    密碼: <input type="password" name="password"><br>
    <input type="submit" value="登入">
</form>
</body>
</html>
```



**login.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>登入結果</title></head>
<body>
<%
    String user = request.getParameter("username");
    String pass = request.getParameter("password");

    if ("admin".equals(user) && "1234".equals(pass)) {
        out.println("<h3>✅ 歡迎，" + user + "</h3>");
    } else {
        out.println("<h3>❌ 帳號或密碼錯誤</h3>");
    }
%>
</body>
</html>
```





------



## **2️⃣ 表單驗證（前端 + 伺服端）**

### **(A) 前端基本驗證（JavaScript）**

**register.html**

```java
<html>
<head>
    <title>註冊頁面</title>
    <script>
    function validateForm() {
        var email = document.forms["regForm"]["email"].value;
        var pwd1 = document.forms["regForm"]["password"].value;
        var pwd2 = document.forms["regForm"]["confirm"].value;

        if (email == "" || pwd1 == "") {
            alert("⚠️ Email 與密碼不可空白");
            return false;
        }
        if (pwd1 != pwd2) {
            alert("⚠️ 兩次密碼不一致");
            return false;
        }
        return true;
    }
    </script>
</head>
<body>
<h2>會員註冊</h2>
<form name="regForm" action="register.jsp" method="post" onsubmit="return validateForm();">
    Email: <input type="text" name="email"><br>
    密碼: <input type="password" name="password"><br>
    確認密碼: <input type="password" name="confirm"><br>
    <input type="submit" value="註冊">
</form>
</body>
</html>
```



### **(B) 伺服端驗證（JSP）**

**register.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>註冊結果</title></head>
<body>
<%
    String email = request.getParameter("email");
    String pwd1 = request.getParameter("password");
    String pwd2 = request.getParameter("confirm");

    if (email == null || email.trim().isEmpty() ||
        pwd1 == null || pwd1.trim().isEmpty()) {
        out.println("<p>❌ Email 與密碼不可為空</p>");
    } else if (!pwd1.equals(pwd2)) {
        out.println("<p>❌ 兩次密碼不一致</p>");
    } else {
        out.println("<p>✅ 註冊成功！Email: " + email + "</p>");
    }
%>
</body>
</html>
```





------



## **3️⃣ 上傳檔案（Multipart）**

JSP 本身沒有內建檔案上傳功能，通常會用 **Apache Commons FileUpload** 套件。

### **(A) 準備**

1. 下載 commons-fileupload.jar 與 commons-io.jar

   

2. 放到 **Tomcat 的** **lib/** **目錄**

   



------



### **(B) 建立上傳表單**

**upload.html**

```java
<html>
<head><title>檔案上傳</title></head>
<body>
<h2>上傳檔案</h2>
<form action="upload.jsp" method="post" enctype="multipart/form-data">
    選擇檔案: <input type="file" name="file"><br>
    <input type="submit" value="上傳">
</form>
</body>
</html>
```





------



### **(C) JSP 處理上傳**

**upload.jsp**

```java
<%@ page import="java.util.*, java.io.*, org.apache.commons.fileupload.*, org.apache.commons.fileupload.disk.*, org.apache.commons.fileupload.servlet.*" %>
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>檔案上傳結果</title></head>
<body>
<h2>檔案上傳結果</h2>

<%
    DiskFileItemFactory factory = new DiskFileItemFactory();
    ServletFileUpload upload = new ServletFileUpload(factory);

    try {
        List<FileItem> items = upload.parseRequest(request);
        for (FileItem item : items) {
            if (!item.isFormField()) {
                String fileName = item.getName();
                File uploadFile = new File(application.getRealPath("/") + "uploads", fileName);
                uploadFile.getParentFile().mkdirs(); // 建立目錄
                item.write(uploadFile);
                out.println("<p>✅ 已上傳：" + fileName + "</p>");
            }
        }
    } catch (Exception e) {
        out.println("<p>❌ 上傳失敗：" + e.getMessage() + "</p>");
    }
%>

</body>
</html>
```



📌 上傳後的檔案會儲存在 Tomcat/webapps/yourapp/uploads/



- **JSP 範圍屬性比較：Page、Request、Session、Application**

| **範圍**        | **存活週期**                                           | **資料共享範圍**                                 | **用途範例**         | **儲存方式**               |
| --------------- | ------------------------------------------------------ | ------------------------------------------------ | -------------------- | -------------------------- |
| **Page**        | 當前 JSP 頁面請求結束即消失                            | 僅限當前 JSP 頁面                                | 暫存頁面內部變數     | pageContext.setAttribute() |
| **Request**     | 一次 HTTP 請求完整處理期間                             | 同一次請求中的所有資源（包含轉發的 JSP/Servlet） | 轉發頁面間共享資料   | request.setAttribute()     |
| **Session**     | 使用者瀏覽器與伺服器連線期間（默認30分鐘不活躍則失效） | 同一使用者的多次請求                             | 登入狀態、購物車資料 | session.setAttribute()     |
| **Application** | 伺服器啟動至停止（全應用程序範圍）                     | 所有使用者、所有請求                             | 系統參數、共用資源   | application.setAttribute() |

VS Code 官方的 Java Extension 有包含 Maven 

varchar vs char

## **主要差異比較：****VARCHAR** **vs** **CHAR**

| **特性**     | **VARCHAR(n)**                            | **CHAR(n)**                        |
| ------------ | ----------------------------------------- | ---------------------------------- |
| **長度**     | 可變長度，最多 n 字元                     | 固定長度，總是剛好 n 字元          |
| **儲存方式** | 儲存實際字串長度 + 內容（變動大小）       | 不足長度會用空白補滿（固定大小）   |
| **儲存空間** | **節省空間，只佔實際字數 + 額外位元組**   | 佔用固定空間（浪費空間但簡單）     |
| **查詢效能** | 通常較慢，尤其在頻繁修改的欄位            | **通常較快，適合長度一致的資料**   |
| **對比行為** | 根據 DBMS，空白可能影響比較               | 空白自動填充，但比對時通常忽略空白 |
| **最大長度** | 各 DBMS 不同（如 MySQL 最多 65,535 字元） | 限於 n 字元                        |

## **使用建議**

| **情境**                       | **推薦使用** |
| ------------------------------ | ------------ |
| 一般字串輸入（姓名、Email 等） | VARCHAR      |
| 固定長度編碼（郵遞區號、代碼） | CHAR         |
| 資料長度差異大                 | VARCHAR      |
| 高速查詢、密集比較的欄位       | CHAR         |



# **第八章：Session 與 Cookie 管理**

## **1. Cookie 與 Session 簡介**

- **Cookie**

  

  - 儲存在 **瀏覽器端**（Client）的資料（文字檔）

    

  - 每次請求時自動隨 HTTP Header 送到伺服器

    

  - 適合保存 **非敏感資訊**（例如：記住帳號、語系設定）

    

- **Session**

  

  - 儲存在 **伺服器端**，由伺服器維護一個 HttpSession 物件

    

  - 透過 **Session ID** 與瀏覽器關聯

    

  - 適合保存 **使用者狀態資訊**（例如：登入狀態、購物車）

    



------



## **2. 使用 Cookie 保存資料**

### **寫入 Cookie**

```java
<%
    Cookie userCookie = new Cookie("username", "Tom");
    userCookie.setMaxAge(60*60*24); // 1 天
    response.addCookie(userCookie);
%>
```



### **讀取 Cookie**

```java
<%
    Cookie[] cookies = request.getCookies();
    if (cookies != null) {
        for (Cookie c : cookies) {
            if ("username".equals(c.getName())) {
                out.print("歡迎回來，" + c.getValue());
            }
        }
    }
%>
```





------



## **3. 使用 Session 保存使用者狀態**

### **寫入 Session**

```java
<%
    HttpSession session = request.getSession();
    session.setAttribute("username", "Tom");
%>
```



Session 已設定！

### **讀取 Session**

```java
<%
    String username = (String) session.getAttribute("username");
    if (username != null) {
        out.print("目前使用者：" + username);
    } else {
        out.print("尚未登入！");
    }
%>
```



### **移除 Session（登出）**

```java
<%
    session.invalidate(); // 銷毀 Session
%>
```



您已登出！



------



## **4. 登入/登出系統範例**

### **(1)** **login.jsp**（登入頁面）



```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>會員登入</title></head>
<body>
    <h2>會員登入</h2>
    <form action="loginCheck.jsp" method="post">
        帳號：<input type="text" name="username"><br>
        密碼：<input type="password" name="password"><br>
        <input type="submit" value="登入">
    </form>
</body>
</html>
```





------



### **(2)** **loginCheck.jsp**（處理登入）

```java
<%@ page import="jakarta.servlet.http.*,jakarta.servlet.*" %>
<%
    String user = request.getParameter("username");
    String pass = request.getParameter("password");

    if ("admin".equals(user) && "1234".equals(pass)) {
        // 建立 Session
        session.setAttribute("username", user);
        response.sendRedirect("welcome.jsp");
    } else {
        out.print("帳號或密碼錯誤！<a href='login.jsp'>重新登入</a>");
    }
%>
```





------



### **(3)** **welcome.jsp**（登入後頁面）



```java
<%@ page contentType="text/html; charset=UTF-8" %>
<%
    String username = (String) session.getAttribute("username");
    if (username == null) {
        response.sendRedirect("login.jsp");
    }
%>
<html>
<head><title>會員中心</title></head>
<body>
    <h2>歡迎，<%= username %>！</h2>
    <a href="logout.jsp">登出</a>
</body>
</html>
```





------



### **(4)** **logout.jsp**（登出）

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<%
    session.invalidate(); // 銷毀 Session
    response.sendRedirect("login.jsp");
%>
```



# **第九章：JSP 與 EL（Expression Language）**

## **1️⃣ 基本語法：**${}

- **EL (Expression Language)** 是 JSP 提供的一種簡化語法，用來取代 <%= %>，讓程式碼更乾淨。
-  在 JSP 中使用 EL，可以直接存取 **變數、屬性值**，不需要 Java 程式碼。

### **範例：基本 EL**

**el_basic.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>EL 基本範例</title></head>
<body>
<%
    request.setAttribute("username", "Alice");
    request.setAttribute("age", 25);
%>

<p>傳統 JSP 表達式：<%= request.getAttribute("username") %></p>
<p>EL 表達式：${username}</p>
<p>EL 運算：${age + 5}</p>
</body>
</html>
```



輸出結果：

傳統 JSP 表達式：Alice

EL 表達式：Alice

EL 運算：30



------



## **2️⃣ EL 的運算子**

EL 支援 **算術運算子、比較運算子、邏輯運算子**。

### **(A) 算術運算子**

```java
${10 + 5}   → 15
${10 - 3}   → 7
${10 * 2}   → 20
${10 / 2}   → 5
${10 % 3}   → 1
```



### **(B) 比較運算子**

```java
${5 > 3}      → true
${5 lt 3}     → false   (lt = less than)
${5 >= 5}     → true
${5 eq 5}     → true    (eq = equal)
${5 ne 10}    → true    (ne = not equal)
```



### **(C) 邏輯運算子**

```java
${true && false}  → false
${true || false}  → true
${!false}         → true
```





------



## **3️⃣ 存取範圍變數**

EL 可以直接存取 JSP 四個範圍的變數：

- **pageScope**：只在單一 JSP 頁面有效

  

- **requestScope**：一次請求(request) 內有效（跨 JSP forward 有效）

  

- **sessionScope**：一次 Session（瀏覽器與伺服器的對話）有效

  

- **applicationScope**：整個 Web 應用程式共享

  

### **範例：存取不同 Scope**

**el_scope.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
<head><title>EL 範圍變數</title></head>
<body>
<%
    pageContext.setAttribute("msg", "Page 範圍");
    request.setAttribute("msg", "Request 範圍");
    session.setAttribute("msg", "Session 範圍");
    application.setAttribute("msg", "Application 範圍");
%>

<p>不指定範圍（會由小到大找）：${msg}</p>
<p>PageScope：${pageScope.msg}</p>
<p>RequestScope：${requestScope.msg}</p>
<p>SessionScope：${sessionScope.msg}</p>
<p>ApplicationScope：${applicationScope.msg}</p>
</body>
</html>
```





輸出結果：

不指定範圍（會由小到大找）：Page 範圍

PageScope：Page 範圍

RequestScope：Request 範圍

SessionScope：Session 範圍

ApplicationScope：Application 範圍



------



## **4️⃣ 與 JSTL 搭配使用**

**JSTL (JSP Standard Tag Library)** 搭配 EL 可以讓 JSP 更乾淨。
 常用的 JSTL 標籤庫：

- c:out：輸出變數

  

- c:forEach：迴圈

  

- c:if：條件判斷

  

### **(A) 設定 JSTL**

在 JSP 開頭引入 JSTL 標籤庫：

<**%**@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>



------



### **(B) JSTL + EL 範例**

**el_jstl.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head><title>JSTL + EL 範例</title></head>
<body>
<%
    java.util.List<String> users = new java.util.ArrayList<>();
    users.add("Alice");
    users.add("Bob");
    users.add("Charlie");
    request.setAttribute("userList", users);
%>

<h3>使用 JSTL + EL 輸出清單</h3>
<ul>
    <c:forEach var="u" items="${userList}">//建議使用
        <li>${u}</li>
    </c:forEach>
</ul>

<c:if test="${fn:length(userList) > 2}">
    <p>✅ 使用者數量超過 2</p>
</c:if>
</body>
</html>
```



輸出結果：

\- Alice

\- Bob

\- Charlie

✅ 使用者數量超過 2

# **第十章：JSP 標籤庫（JSTL）**

## **1. JSTL 簡介**

- **JSTL（JavaServer Pages Standard Tag Library） 是一組標準化的 JSP 標籤庫，提供常用功能（流程控制、迴圈、格式化、資料庫存取…）。****

  **

- **優點：****

  **

  - **減少 JSP 中 Java Scriptlet** **<% %>** **的使用。****

    **

  - **提升程式可讀性與維護性。****

    **

  - **搭配 EL（Expression Language） 使用更直覺。****

    **

**JSTL 的 Tag Library 宣告方式：**

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
```





------



## **2. Core 標籤**

### **2.1 條件判斷** **<c:if>**

```java
<c:if test="${param.user == 'admin'}">
    歡迎管理員！
</c:if>
```



### **2.2 多分支** **<c:choose>**

```java
<c:choose>
    <c:when test="${param.score >= 90}">優秀</c:when>
    <c:when test="${param.score >= 60}">及格</c:when>
    <c:otherwise>不及格</c:otherwise>
</c:choose>
```



### **2.3 迴圈** **<c:forEach>**

```java
<c:forEach var="item" items="${requestScope.products}">
    商品：${item.name}, 價格：${item.price}<br>
</c:forEach>
```





------



## **3. Formatting 標籤**

**需要引入：**

```java
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```



### **3.1 日期格式化**

```java
<fmt:formatDate value="${now}" pattern="yyyy-MM-dd HH:mm:ss"/>
```



### **3.2 數字格式化**

```java
<fmt:formatNumber value="12345.678" type="currency"/>
```



### **3.3 國際化（i18n）**

```java
<fmt:setLocale value="zh_TW"/>
<fmt:message key="welcome.message"/>
```





------



## **4. SQL 標籤（僅適合教學，實務上建議用 DAO/Service）**

### **4.1 資料來源設定**

```sql
<sql:setDataSource var="db" driver="com.mysql.cj.jdbc.Driver"
    url="jdbc:mysql://localhost:3306/testdb"
    user="root"  password="1234"/>
```



### **4.2 查詢**

```sql
<sql:query var="result" dataSource="${db}">
    SELECT * FROM users;
</sql:query>
```



```sql
<c:forEach var="row" items="${result.rows}">
    使用者：${row.username}, 信箱：${row.email}<br>
</c:forEach>
```



### **4.3 更新（INSERT/UPDATE/DELETE）**

```sql
<sql:update dataSource="${db}">
    INSERT INTO users (username, email) VALUES ('Tom', 'tom@test.com');
</sql:update>
```





------



## **5. 自訂標籤（Custom Tag）**

### **5.1 建立 TLD (Tag Library Descriptor)**

**WEB-INF/tags/mytag.tld**

```java
<taglib>
  <tlib-version>1.0</tlib-version>
  <uri>http://example.com/mytag</uri>
  <tag>
    <name>hello</name>
    <tag-class>com.example.HelloTag</tag-class>
    <body-content>empty</body-content>
  </tag>
</taglib>
```



### **5.2 Java 標籤類別**

```java
package com.example;

import jakarta.servlet.jsp.tagext.TagSupport;
import jakarta.servlet.jsp.JspWriter;

public class HelloTag extends TagSupport {
    @Override
    public int doStartTag() {
        try {
            JspWriter out = pageContext.getOut();
            out.print("Hello, Custom Tag!");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return SKIP_BODY;
    }
}
```



### **5.3 JSP 使用**

```java
<%@ taglib prefix="my" uri="http://example.com/mytag" %>
<my:hello/>
```



# **第十一章：MVC 與 JSP**

## **1. MVC 架構簡介**

- **MVC（Model-View-Controller） 是常見的 Web 架構設計模式：**

  

  - **Model（資料/商業邏輯層）**

    

    - **負責資料處理、商業邏輯**

      

    - **通常包含 JavaBean（資料物件）與 DAO（資料存取物件）**

      

  - **View（視圖層）**

    

    - **JSP 負責畫面呈現**

      

    - **透過 EL 或 JSTL 顯示資料**

      

  - **Controller（控制層）**

    

    - **Servlet 負責接收請求（request）、呼叫 Model 處理資料，最後轉交給 View 呈現**

      



------



## **2. MVC 與 JSP 的角色分工**

### **(1) Controller (Servlet)**

- **接收瀏覽器送來的 HTTP Request**

  

- **驗證參數、呼叫 Model（JavaBean/DAO）**

  

- **把處理後的結果傳給 JSP**

  

**範例：**

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
            throws IOException, ServletException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        UserDAO userDAO = new UserDAO();
        User user = userDAO.login(username, password);

        if (user != null) {
            request.setAttribute("user", user);
            request.getRequestDispatcher("welcome.jsp").forward(request, response);
        } else {
            request.setAttribute("error", "帳號或密碼錯誤！");
            request.getRequestDispatcher("login.jsp").forward(request, response);
        }
    }
}
```





------



### **(2) View (JSP)**

- **JSP 只負責顯示資料，不直接處理商業邏輯**

  

- **搭配 EL 與 JSTL 來呈現**

  

**範例** **welcome.jsp**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head><title>會員中心</title></head>
<body>
    <h2>歡迎 ${user.username}！</h2>
    <p>Email: ${user.email}</p>
    <a href="logout.jsp">登出</a>
</body>
</html>
```

- <%@ taglib %> 很重要

------



### **(3) Model (JavaBean / DAO)**

#### **JavaBean（封裝資料）**

```java
public class User {
    private String username;
    private String password;
    private String email;

    // Getter & Setter
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```



#### **DAO（資料存取層）**

```java
import java.sql.*;

public class UserDAO {
    private final String URL = "jdbc:mysql://localhost:3306/testdb";
    private final String USER = "root";
    private final String PASSWORD = "1234";

    public User login(String username, String password) {
        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD)) {
            String sql = "SELECT * FROM users WHERE username=? AND password=?";
            PreparedStatement stmt = conn.prepareStatement(sql);
            stmt.setString(1, username);
            stmt.setString(2, password);

            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                User user = new User();
                user.setUsername(rs.getString("username"));
                user.setEmail(rs.getString("email"));
                return user;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null; // 登入失敗
    }
}
```





------



## **3. 範例：小型會員系統**

### **功能**

1. **使用者登入**

   

2. **成功 → welcome.jsp**

   

3. **失敗 → 回到 login.jsp 顯示錯誤訊息**

   

### **(1)** **login.jsp** **(表單)**

```java
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head><title>會員登入</title></head>
<body>
    <h2>會員登入</h2>
    <form action="login" method="post">
        帳號：<input type="text" name="username"><br>
        密碼：<input type="password" name="password"><br>
        <input type="submit" value="登入">
    </form>
    <c:if test="${not empty error}">
        <p style="color:red">${error}</p>
    </c:if>
</body>
</html>
```



### **(2)** **LoginServlet.java** **(Controller)**

**👉 如前面 Controller 範例**

### **(3)** **User.java** **(JavaBean)**

**👉 封裝會員資料**

### **(4)** **UserDAO.java** **(Model/DAO)**

**👉 負責資料庫操作**

### **(5)** **welcome.jsp** **(View)**

**👉 成功登入後顯示會員資訊**