#  Sqlite C/C++ for Beginners 

```
https://www.youtube.com/watch?v=-C5HSdPU3TI
```

#  C/C++ SQLite Tutorial: Including In Your Project 

```
https://www.youtube.com/watch?v=jIFflT4mW5c
```

Got it 👍 You want to use **SQLite with C/C++ in Visual Studio** (probably Windows) using the **sqlite-amalgamation-3500400.zip** package. Let me walk you through it step by step:

---

## 🔹 Step 1: Download SQLite

* Go to: [SQLite Download Page](https://www.sqlite.org/download.html)
* Download:

  * **sqlite-amalgamation-3500400.zip** → contains `sqlite3.c` and `sqlite3.h`.
  * **sqlite-dll-win64-x64-3500400.zip** (optional, for DLL use).
  * **sqlite-tools-win32-x86-3500400.zip** (optional, for command-line).

Unzip them somewhere, e.g. `C:\sqlite`.

---

## 🔹 Step 2: Create a Visual Studio Project

1. Open **Visual Studio** → Create a **Console App (C/C++)** project.
2. Name it something like `SQLiteDemo`.

---

## 🔹 Step 3: Add SQLite to the Project

1. Copy `sqlite3.c` and `sqlite3.h` into your project folder.
2. In Visual Studio:

   * Right click **Source Files** → **Add → Existing Item…** → add `sqlite3.c`.
   * Right click **Header Files** → **Add → Existing Item…** → add `sqlite3.h`.

---

## 🔹 Step 4: Write a Simple Program

Here’s a **basic example** to create a database, a table, insert, and read data:

```c
#include <stdio.h>
#include "sqlite3.h"

int main() {
    sqlite3 *db;
    char *err_msg = 0;

    // Open database (creates test.db if not exists)
    int rc = sqlite3_open("test.db", &db);

    if (rc != SQLITE_OK) {
        fprintf(stderr, "Cannot open database: %s\n", sqlite3_errmsg(db));
        return 1;
    }

    // Create SQL statement
    const char *sql = "CREATE TABLE IF NOT EXISTS Users(Id INT, Name TEXT);"
                      "INSERT INTO Users VALUES(1, 'Alice');"
                      "INSERT INTO Users VALUES(2, 'Bob');";

    // Execute SQL
    rc = sqlite3_exec(db, sql, 0, 0, &err_msg);

    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", err_msg);
        sqlite3_free(err_msg);
    } else {
        printf("Table created and data inserted!\n");
    }

    // Read data
    const char *select_sql = "SELECT * FROM Users;";
    sqlite3_stmt *stmt;

    rc = sqlite3_prepare_v2(db, select_sql, -1, &stmt, 0);

    while ((rc = sqlite3_step(stmt)) == SQLITE_ROW) {
        int id = sqlite3_column_int(stmt, 0);
        const unsigned char *name = sqlite3_column_text(stmt, 1);

        printf("ID: %d | Name: %s\n", id, name);
    }

    sqlite3_finalize(stmt);
    sqlite3_close(db);

    return 0;
}
```

---

## 🔹 Step 5: Build & Run

* Press **Ctrl+F5** in Visual Studio.
* It will create `test.db` in your project folder.
* You should see:

  ```
  Table created and data inserted!
  ID: 1 | Name: Alice
  ID: 2 | Name: Bob
  ```

---

✅ Now you have a working **SQLite + C/C++ setup in Visual Studio** using `sqlite-amalgamation-3500400.zip`.

---

Do you want me to also show you how to **structure a reusable SQLite helper class in C++** (so you don’t write raw `sqlite3_exec` every time)?
