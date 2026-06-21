
print("Program Started...\n")

import mysql.connector

# ---------- CONNECTION ----------
try:
    con = mysql.connector.connect(
        host="localhost",
        user="root",
        password="Your password",
        auth_plugin="mysql_native_password",
        use_pure=True
    )

    cursor = con.cursor()
    print("✅ Connected to MYSQL\n")

except Exception as e:
    print("Connection Error:", e)
    exit()

# ----------- DATABASE SETUP -------

try:
    cursor.execute("CREATE DATABASE IF NOT EXISTS library_db")
    cursor.execute("USE library_db")

    cursor.execute("""
    CREATE TABLE IF NOT EXISTS library (
        title VARCHAR(100),
        author VARCHAR(100),
        book_id VARCHAR(20) PRIMARY KEY,
        available BOOLEAN,
        borrower VARCHAR(50)
    )
    """)

    print("✅ Database & Table Ready\n")

except Exception as e:
    print("Database Error:", e)
    exit()

# --------- FUNCTIONS ---------

# ----------- ADD BOOK ----------
def add_book():
    title = input("Enter title: ")
    author = input("Enter author: ")
    book_id = input("Enter book ID: ")

    cursor.execute(
        "SELECT * FROM library WHERE book_id=%s",
        (book_id,)
    )

    if cursor.fetchone():
        print("❌ Book already exists.\n")
        return

    cursor.execute(
        "INSERT INTO library VALUES (%s,%s,%s,%s,%s)",
        (title, author, book_id, True, None)
    )

    con.commit()
    print("✅ Book added successfully.\n")


# ----------- VIEW BOOKS ----------
def view_books():
    cursor.execute("SELECT * FROM library")
    data = cursor.fetchall()

    if not data:
        print("No books found.\n")
    else:
        print("\n--- BOOK LIST ---")

        for row in data:
            status = "Available" if row[3] else "Borrowed"
            print(row[0], "-", row[1], "-", status)

        print()


# ----------- SEARCH BOOK ----------
def search_book():
    title = input("Enter title: ")

    cursor.execute(
        "SELECT * FROM library WHERE title=%s",
        (title,)
    )

    row = cursor.fetchone()

    if row:
        status = "Available" if row[3] else "Borrowed"

        print("\nTitle:", row[0])
        print("Author:", row[1])
        print("Book ID:", row[2])
        print("Status:", status, "\n")

    else:
        print("Book not found.\n")


# ----------- BORROW BOOK ----------
def borrow_book():
    book_id = input("Enter Book ID: ")

    cursor.execute(
        "SELECT * FROM library WHERE book_id=%s",
        (book_id,)
    )

    row = cursor.fetchone()

    if row:
        if row[3]:
            name = input("Enter borrower name: ")

            cursor.execute(
                "UPDATE library SET available=%s, borrower=%s WHERE book_id=%s",
                (False, name, book_id)
            )

            con.commit()
            print("✅ Book borrowed successfully.\n")

        else:
            print("❌ Book already borrowed.\n")

    else:
        print("Book not found.\n")


# ----------- RETURN BOOK ----------
def return_book():
    book_id = input("Enter Book ID: ")

    cursor.execute(
        "SELECT * FROM library WHERE book_id=%s",
        (book_id,)
    )

    row = cursor.fetchone()

    if row:
        if not row[3]:

            cursor.execute(
                "UPDATE library SET available=%s, borrower=%s WHERE book_id=%s",
                (True, None, book_id)
            )

            con.commit()
            print("✅ Book returned successfully.\n")

        else:
            print("❌ This book was not borrowed.\n")

    else:
        print("Book not found.\n")


# ----------- DELETE BOOK ----------
def delete_book():
    book_id = input("Enter Book ID: ")

    cursor.execute(
        "SELECT * FROM library WHERE book_id=%s",
        (book_id,)
    )

    if cursor.fetchone():

        cursor.execute(
            "DELETE FROM library WHERE book_id=%s",
            (book_id,)
        )

        con.commit()
        print("🗑️ Book deleted successfully.\n")

    else:
        print("Book not found.\n")


# ----------- TOTAL BOOKS ----------
def total_books():
    cursor.execute("SELECT COUNT(*) FROM library")

    print(
        "Total books:",
        cursor.fetchone()[0],
        "\n"
    )


# ----------- AVAILABLE BOOKS ----------
def available_books():
    cursor.execute(
        "SELECT * FROM library WHERE available=TRUE"
    )

    data = cursor.fetchall()

    if data:
        print("\n--- AVAILABLE BOOKS ---")

        for row in data:
            print(
                row[0],
                "-",
                row[1],
                "- ID:",
                row[2]
            )

        print()

    else:
        print("No available books.\n")


# ----------- BORROWED BOOKS ----------
def borrowed_books():
    cursor.execute(
        "SELECT * FROM library WHERE available=FALSE"
    )

    data = cursor.fetchall()

    if data:
        print("\n--- BORROWED BOOKS ---")

        for row in data:
            print(
                row[0],
                "-",
                row[1],
                "- ID:",
                row[2],
                "- Borrower:",
                row[4]
            )

        print()

    else:
        print("No borrowed books.\n")


# ----------- LIBRARY STATUS ----------
def library_status():

    cursor.execute(
        "SELECT COUNT(*) FROM library WHERE available=TRUE"
    )
    available = cursor.fetchone()[0]

    cursor.execute(
        "SELECT COUNT(*) FROM library WHERE available=FALSE"
    )
    borrowed = cursor.fetchone()[0]

    print("\nLibrary Status")
    print("Available Books:", available)
    print("Borrowed Books:", borrowed)
    print()


# ----------- MENU ----------

while True:

    print("====== LIBRARY MENU ======")
    print("1. Add Book")
    print("2. View Books")
    print("3. Search Book")
    print("4. Borrow Book")
    print("5. Return Book")
    print("6. Delete Book")
    print("7. Total Books")
    print("8. Available Books")
    print("9. Borrowed Books")
    print("10. Library Status")
    print("11. Exit")

    choice = input("Enter choice: ")

    if choice == "1":
        add_book()

    elif choice == "2":
        view_books()

    elif choice == "3":
        search_book()

    elif choice == "4":
        borrow_book()

    elif choice == "5":
        return_book()

    elif choice == "6":
        delete_book()

    elif choice == "7":
        total_books()

    elif choice == "8":
        available_books()

    elif choice == "9":
        borrowed_books()

    elif choice == "10":
        library_status()

    elif choice == "11":
        print("Exiting...")
        break

    else:
        print("Invalid choice.\n")

cursor.close()
con.close()
