
# 🌍 NASL – Nessus Attack Scripting Language

Penetration testing (or pen-testing) is a process where **exploits** are run on a machine to determine its vulnerability to attacks. If a pen-test fails, it is highly likely that a hacker could exploit your computer resources. 💻 As the number of security exploits grows rapidly, so does the need for frequent pen-testing. 🔒

One of the most popular penetration-testing tools in the world today is **Nessus**, available for both Linux/Unix and Windows systems. 🐧 **Nessus** is a shining example of the power of **open-source tools**. 🌐 With over 8,000 pen tests (and more being created daily), Nessus uses **NASL** (Nessus Attack Scripting Language) to write these tests. NASL is a simple yet powerful scripting language, created specifically for writing pen tests. 🚀

Our goal is to help you learn **how to write the most complex pen tests** using NASL. Let’s dive in! 🤿

---

## 🛠️ Setup: Getting Started with NASL

### Step 1: Create a Directory
First, create a directory where you'll store your NASL scripts:

```bash
C:\nasl
```

### Step 2: Set the Path
Set the `path` variable to:

```bash
C:\program files\tenable\newt
```

This is where the NASL interpreter (`nasl.exe`) has been installed.

### Step 3: Write Your First Program
Now, write your first NASL script in `a.nasl`:

```nasl
display("hi", "\n");
```

### Step 4: Run the NASL Interpreter
To run the script, execute the following command:

```bash
nasl c:\nasl\a.nasl
```

You should see the following output:

```text
hi
hi
```

🎉 **Congratulations!** You’ve just written and run your first NASL script!

---

## 📝 NASL Basics: Understanding Syntax and Functions

NASL is a scripting language, which means it is simpler than compiled languages like C or C++. 💡 Here’s an introduction to some key concepts.

### **The `display` Function** 
In NASL, the `display()` function is similar to the `printf()` function in C. It outputs text to the screen. For example:

```nasl
display("hi", "\n");
```

Output:

```text
hi hi
```

> **Note**: The `display()` function prints the text **twice** by default. 🧐

### **Semicolons** and Errors
NASL, like many programming languages, uses a **semicolon** `;` to mark the end of a statement. Without it, you’ll receive a **parse error**. 😱

For example:

```nasl
display("hi", "\n")
```

Will result in:

```text
parse error, unexpected $, expecting ';'
```

---

## 🧮 Variables in NASL

In NASL, you don’t need to explicitly declare variables. You can simply use them! Variables can even change types mid-program. Let’s explore this:

```nasl
i = 20;
display("Value of i is ", i, "\n");
i = i + 10;
display("Value of i is ", i, "\n");
```

Output:

```text
Value of i is 20
Value of i is 30
```

Later in the program, you can even change `i` from an integer to a string! 🧙‍♂️

```nasl
i = "Jenil Vekariya";
display("Value of i is ", i, "\n");
```

Output:

```text
Value of i is Jenil Vekariya
```

---

## 🌐 Network Functions: Open Sockets & Port Scanning

NASL provides powerful networking features, such as opening sockets and scanning ports. 📡

Here’s how you can open a socket on **port 79**:

```nasl
i = open_sock_tcp(79);
display("The value of i is ", i, "\n");
```

Since there’s no service running on port 79, the result will be:

```text
The value of i is 0
```

Try **port 80** (HTTP):

```nasl
i = open_sock_tcp(80);
display("The value of i is ", i, "\n");
```

This will return:

```text
The value of i is 1
```

This tells us there is a web server running on **port 80**. 🌐

---

### 🔄 Loops: Repeating Code with `for`

You can use **for loops** in NASL just like in C. Loops allow you to repeat code multiple times. Let’s iterate over a set of ports:

```nasl
for (i = 10; i <= 12; i++) {
    display("Value of i is ", i, "\n");
}
```

Output:

```text
Value of i is 10
Value of i is 11
Value of i is 12
```

---

## 🛠️ Building a Simple Port Scanner

Want to write your own port scanner? Here's a basic NASL script that checks whether ports are open or closed:

```nasl
for (i = 20; i <= 25; i++) {
    sock = open_sock_tcp(i);
    if (sock) {
        display("Port no ", i, " is Open\n");
    } else {
        display("Port no ", i, " is Closed\n");
    }
}
```

When you run this, you’ll see which ports are open and which are closed between **20** and **25**. 🔓🔐

---

## 🔐 Checking for Anonymous FTP Access

Here’s a handy script to check if **anonymous FTP access** is allowed on a system (this is often a security risk 🚨):

```nasl
sock = open_sock_tcp(21);
i = ftp_log_in(socket:sock, user:"anonymous", pass:"vijay@mukhi.com");
if (i == 1) {
    display("Anonymous FTP access is allowed. A bad thing! 🚫\n");
} else {
    display("Anonymous FTP access is not allowed. A good thing! ✅\n");
}
```

---

## 🛠️ Practical Exploits: Exploiting a Microsoft FTP Service

Want to see a simple exploit in action? Here's how you can send commands to a **Microsoft FTP Service**:

```nasl
sock = open_sock_tcp(21);
r = recv(socket:sock, length:1024);
display(r, "\n");
req = string("USER anonymous\r\n");
send(socket:sock, data:req);
r = recv(socket:sock, length:1024);
display(r, "\n");
```

Output:

```text
220 Microsoft FTP Service Version 5.0
331 Anonymous access allowed, send identity (email) as password.
```

This script connects to an FTP server and logs in using **anonymous** credentials. 😈

---

## 🌍 HTTP Exploits: Detecting Web Servers

NASL also allows you to interact with **web servers**. Here’s how to check whether the server is running **Apache** or **IIS**:

```nasl
include("http_func.inc");
sock = open_sock_tcp(80);
req = string("GET / HTTP/1.0\r\n","Accept: */*\r\n","\r\n");
send(socket:sock, data:req);
r = recv(socket:sock, length:4096);
if ("Server: Apache" >< r) {
    display("Apache Server running on host\n");
} else if ("Server: Microsoft-IIS" >< r) {
    display("IIS Server running on host\n");
}
http_close_socket(sock);
```

This script sends an HTTP request to **port 80** and checks the response to determine if it’s an **Apache** or **IIS** server. 🌐

---

### 🚀 Conclusion

With **NASL**, you can create powerful scripts to perform a wide range of penetration tests, from basic port scanning to identifying security vulnerabilities. 🔐✨ By mastering this scripting language, you can harness the full power of **Nessus** and ensure robust security for systems.

Happy scripting! 🧑‍💻🎉

---
