# 🚨 CVE Proof-of-Concept – SQL Injection in `ScriptRunner` (jfaster/mango)

This repository demonstrates a **SQL Injection vulnerability** in the `ScriptRunner` class of the [jfaster/mango](https://github.com/jfaster/mango) project. Using a Java Swing demo application (`LoginApp`), this PoC shows how an attacker can inject SQL, bypass authentication, and extract database content.

---

## 📦 Affected Product

- **Vendor**: jfaster  
- **Product**: mango  
- **Version**: 2.0.1  
- **Component**: `org.jfaster.mango.util.ScriptRunner`  
- **Type**: SQL Injection  
- **Impact**: Authentication Bypass, Information Disclosure, Arbitrary Query Execution  

---

## 🧪 Step-by-Step Exploit Guide

### ✅ Step 1 – Clone the Repository
```bash
git clone https://github.com/jfaster/mango.git
cd mango
```

### 🖥️ Step 2 – Install and Setup Eclipse
- Download [Eclipse IDE for Java Developers](https://www.eclipse.org/downloads/)
- Install Eclipse and open it.
- Click `File → Import → Maven → Existing Maven Projects`
- Select the cloned `mango` folder and click `Finish`

### 📁 Step 3 – Add the Vulnerable Demo (`LoginApp`)
- In Eclipse project tree, open `mango/src/test/java/org/jfaster/mango/util`
- Right-click → `New → Class`
- Name it `LoginApp`
- Paste provided `LoginApp.java` code below and save

### ▶️ Step 4 – Run the Demo
- Right-click `LoginApp.java` → `Run As → Java Application`
- A Swing login window titled **Login Form (SQL Injection Test)** will appear

### 🧨 Test Inputs

#### ✅ Valid Login
- Username: `admin`
- Password: `admin123`

#### ❌ Invalid Login
- Username: `user1`
- Password: `wrong`

#### 🔥 SQL Injection Bypass
- Username: `' OR 1=1 --`
- Password: `[anything]`

This generates:
```sql
SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = '';
```

The `--` disables password checking, successfully bypassing authentication.

---

## 🔎 Vulnerability Description
The `ScriptRunner` class executes SQL passed via `Reader` without validation, enabling attackers to inject SQL through user inputs.

### 🔥 Vulnerable Code Pattern
```java
String sql = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "';";
scriptRunner.runScript(new StringReader(sql));
```

### 🔐 Impact
- ✅ Authentication bypass
- ✅ Database content exposure
- ⚠️ Execution of arbitrary SQL statements

---

## ✅ Recommendations
- **DO NOT** use `ScriptRunner` with untrusted strings
- Use **PreparedStatement** with proper parameter binding
- Always validate and sanitize inputs
- Restrict DB permissions in production

---

## 🧾 CVE Submission
- Status: Pending CVE ID assignment

### 👤 Discoverer
- Reporter: `fbhmida@umich.edu`
- Purpose: Responsible disclosure & educational demonstration

---

## 📚 References
- [jfaster/mango GitHub](https://github.com/jfaster/mango)
- [Vulnerable ScriptRunner class](https://github.com/jfaster/mango/blob/master/mango-core/src/main/java/org/jfaster/mango/util/ScriptRunner.java)

---

## 💻 `LoginApp.java` (Full Source Code)

*[Full source code provided in the original repository, included above.]*

---

## 🔧 Maven `pom.xml` Snippet (for H2 dependency)
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.1.214</version>
    <scope>test</scope>
</dependency>
```

---

**Need a PDF version or zipped project structure?**

- Contact for additional support.

