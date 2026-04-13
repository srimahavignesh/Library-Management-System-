# Library-Management-System-
This project is a great beginner-friendly implementation of a real-world system using C programming. It strengthens understanding of file handling, data structures, and system logic design.
# 📚 Library Management System in C

A **console-based Library Management System** developed using the C programming language.
This project demonstrates **file handling, structures, and basic system design** concepts in C.

---

## 🎯 Project Overview

This system helps manage a small library by allowing administrators and students to perform essential operations like:

* Managing books 📖
* Managing users 👥
* Issuing and returning books 🔄
* Tracking due dates ⏳

All data is stored using **binary files**, making the system persistent even after program exit.

---

## ✨ Key Features

### 👨‍💼 Admin Panel

The administrator has full control over the system:

* ➕ Add new books
* ❌ Delete books
* 📋 View all books
* 🔍 Search books by ID
* 👤 Add users
* 🗑 Delete users
* 📄 View all users
* 📚 Issue books to users
* 🔁 Accept returned books

---

### 🎓 Student Panel

Students have limited access:

* 📖 View available books
* 🔍 Search books
* 📚 Issue a book
* 🔁 Return a book

---

## 🧠 Concepts Used

This project is built using core C programming concepts:

* **Structures (****`struct`****)** for Book and User data
* **File Handling** (`fopen`, `fread`, `fwrite`)
* **Binary Files** for efficient storage
* **Conditional Statements & Loops**
* **Function-based modular programming**

---

## 🗂 File Structure

```
Library-Management-System/
│
├── main.c          # Main source code
├── books.dat       # Stores book records (auto-created)
├── users.dat       # Stores user records (auto-created)
└── README.md       # Project documentation
```

---

## 🔐 Login System

The system includes a simple login mechanism:

### 👨‍💼 Admin Login

* Username: `admin`
* Password: `1234`

### 🎓 Student Login

* Username: `student`
* Password: `1234`

---

## ⚙️ How to Compile & Run

### 🖥 Using GCC Compiler

1. Compile the program:

   ```
   gcc main.c -o library
   ```

2. Run the program:

   ```
   ./library
   ```

---

## 🔄 System Workflow

### 📚 Book Issue Process

1. Enter Book ID
2. Enter User ID
3. System checks:

   * Book availability
   * User eligibility
4. Set due date (in days)
5. Book is issued

---

### 🔁 Book Return Process

1. Enter User ID
2. Enter Book ID
3. Enter number of days taken
4. System checks:

   * If returned late → ⚠ OVERDUE alert
   * Otherwise → ✅ On-time return

---

## ⚠️ Rules & Constraints

* ❌ Duplicate Book IDs are not allowed
* ❌ Duplicate User IDs are not allowed
* 👤 One user can issue only **one book at a time**
* 📚 A book must be **available** to be issued
* 🔁 Only issued books can be returned

---

## 💾 Data Storage Details

* All records are stored in **binary format**
* Files used:

  * `books.dat`
  * `users.dat`
* Data remains saved even after closing the program

---

## 📸 Sample Output (Console)

```
===== LOGIN =====
1. Admin
2. Student

===== ADMIN PANEL =====
1. Add Book
2. View Books
3. Delete Book
...
```

---

## 🧑‍💻 Author

**Srimahavignesh**

---

## 📌 Conclusion

This project is a great beginner-friendly implementation of a **real-world system using C programming**.
It strengthens understanding of **file handling, data structures, and system logic design**.

---
