#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// ---------------- STRUCTURES ----------------
struct Book {
    int id;
    char name[50];
    char author[50];
    int available;
};

struct User {
    int userId;
    char name[50];
    int issuedBookId;
    int dueDate;
};

// ---------------- FUNCTION DECLARATIONS ----------------
int login();
void addBook();
void displayBooks();
void searchBook();
void deleteBook();

void addUser();
void displayUsers();
void deleteUser();

void issueBook();
void returnBook();

// ---------------- LOGIN ----------------
int login() {
    int role;
    char user[30], pass[30];

    printf("\n===== LOGIN =====\n");
    printf("1. Admin\n2. Student\nChoice: ");
    scanf("%d", &role);

    printf("Username: ");
    scanf("%s", user);
    printf("Password: ");
    scanf("%s", pass);

    if (role == 1 && strcmp(user, "admin") == 0 && strcmp(pass, "1234") == 0)
        return 1;

    if (role == 2 && strcmp(user, "student") == 0 && strcmp(pass, "1234") == 0)
        return 2;

    printf("Invalid login!\n");
    exit(0);

    return 0; 
}

// ---------------- MAIN ----------------
int main() {
    int role = login();
    int ch;

    while (1) {

        if (role == 1) {
            printf("\n===== ADMIN PANEL =====\n");
            printf("1. Add Book\n2. View Books\n3. Delete Book\n4. Search Book\n");
            printf("5. Issue Book\n6. Return Book\n7. Add User\n8. View Users\n");
            printf("9. Delete User\n10. Logout\n0. Exit\nChoice: ");
        } else {
            printf("\n===== STUDENT PANEL =====\n");
            printf("1. View Books\n2. Search Book\n3. Issue Book\n");
            printf("4. Return Book\n5. Logout\n0. Exit\nChoice: ");
        }

        scanf("%d", &ch);

        switch (ch) {
            case 1: addBook(); break;
            case 2: displayBooks(); break;
            case 3: (role==1)? deleteBook(): issueBook(); break;
            case 4: (role==1)? searchBook(): returnBook(); break;
            case 5: (role==1)? issueBook(): (role = login()); break;
            case 6: if(role==1) returnBook(); break;
            case 7: if(role==1) addUser(); break;
            case 8: if(role==1) displayUsers(); break;
            case 9: if(role==1) deleteUser(); break;
            case 10: if(role==1) role = login(); break;
            case 0: exit(0);
            default: printf("Invalid choice!\n");
        }
    }
}

// ---------------- ADD BOOK ----------------
void addBook() {
    FILE *fp = fopen("books.dat", "rb");
    struct Book b, temp;
    int found = 0;

    printf("Enter Book ID: ");
    scanf("%d", &b.id);

    if (fp) {
        while (fread(&temp, sizeof(temp), 1, fp)) {
            if (temp.id == b.id) {
                found = 1;
                break;
            }
        }
        fclose(fp);
    }

    if (found) {
        printf("Duplicate Book ID!\n");
        return;
    }

    printf("Enter Book Name: ");
    scanf(" %[^\n]", b.name);
    printf("Enter Author Name: ");
    scanf(" %[^\n]", b.author);

    b.available = 1;

    fp = fopen("books.dat", "ab");
    if (!fp) {
        printf("File error!\n");
        return;
    }

    fwrite(&b, sizeof(b), 1, fp);
    fclose(fp);

    printf("Book added successfully!\n");
}

// ---------------- DISPLAY BOOKS ----------------
void displayBooks() {
    FILE *fp = fopen("books.dat", "rb");
    struct Book b;

    if (!fp) {
        printf("No books available!\n");
        return;
    }

    printf("\n--- Book List ---\n");

    while (fread(&b, sizeof(b), 1, fp)) {
        printf("ID: %d | %s | %s | %s\n",
               b.id, b.name, b.author,
               b.available ? "Available" : "Issued");
    }

    fclose(fp);
}

// ---------------- SEARCH BOOK ----------------
void searchBook() {
    FILE *fp = fopen("books.dat", "rb");
    struct Book b;
    int id, found = 0;

    if (!fp) {
        printf("No books found!\n");
        return;
    }

    printf("Enter Book ID: ");
    scanf("%d", &id);

    while (fread(&b, sizeof(b), 1, fp)) {
        if (b.id == id) {
            printf("Found: %s by %s (%s)\n",
                   b.name, b.author,
                   b.available ? "Available" : "Issued");
            found = 1;
            break;
        }
    }

    if (!found)
        printf("Book not found!\n");

    fclose(fp);
}

// ---------------- DELETE BOOK ----------------
void deleteBook() {
    FILE *fp = fopen("books.dat", "rb");
    FILE *temp = fopen("temp.dat", "wb");
    struct Book b;
    int id, found = 0;

    if (!fp || !temp) {
        printf("File error!\n");
        return;
    }

    printf("Enter Book ID to delete: ");
    scanf("%d", &id);

    while (fread(&b, sizeof(b), 1, fp)) {
        if (b.id == id) {
            found = 1;
            continue;
        }
        fwrite(&b, sizeof(b), 1, temp);
    }

    fclose(fp);
    fclose(temp);

    remove("books.dat");
    rename("temp.dat", "books.dat");

    printf(found ? "Book deleted!\n" : "Book not found!\n");
}

// ---------------- ADD USER ----------------
void addUser() {
    FILE *fp = fopen("users.dat", "rb");
    struct User u, temp;
    int found = 0;

    printf("Enter User ID: ");
    scanf("%d", &u.userId);

    if (fp) {
        while (fread(&temp, sizeof(temp), 1, fp)) {
            if (temp.userId == u.userId) {
                found = 1;
                break;
            }
        }
        fclose(fp);
    }

    if (found) {
        printf("User ID already exists!\n");
        return;
    }

    printf("Enter Name: ");
    scanf(" %[^\n]", u.name);

    u.issuedBookId = -1;
    u.dueDate = 0;

    fp = fopen("users.dat", "ab");
    if (!fp) {
        printf("File error!\n");
        return;
    }

    fwrite(&u, sizeof(u), 1, fp);
    fclose(fp);

    printf("User registered successfully!\n");
}

// ---------------- DISPLAY USERS ----------------
void displayUsers() {
    FILE *fp = fopen("users.dat", "rb");
    struct User u;

    if (!fp) {
        printf("No users found!\n");
        return;
    }

    printf("\n--- User List ---\n");

    while (fread(&u, sizeof(u), 1, fp)) {
        printf("ID: %d | %s | Issued Book: %d\n",
               u.userId, u.name, u.issuedBookId);
    }

    fclose(fp);
}

// ---------------- DELETE USER ----------------
void deleteUser() {
    FILE *fp = fopen("users.dat", "rb");
    FILE *temp = fopen("temp.dat", "wb");
    struct User u;
    int id, found = 0;

    if (!fp || !temp) {
        printf("File error!\n");
        return;
    }

    printf("Enter User ID to delete: ");
    scanf("%d", &id);

    while (fread(&u, sizeof(u), 1, fp)) {
        if (u.userId == id) {
            found = 1;
            continue;
        }
        fwrite(&u, sizeof(u), 1, temp);
    }

    fclose(fp);
    fclose(temp);

    remove("users.dat");
    rename("temp.dat", "users.dat");

    printf(found ? "User deleted!\n" : "User not found!\n");
}
// ---------------- ISSUE BOOK ----------------
void issueBook() {
    FILE *fb = fopen("books.dat", "rb+");
    FILE *fu = fopen("users.dat", "rb+");

    if (fb == NULL || fu == NULL) {
        printf("File error!\n");
        return;
    }

    struct Book b;
    struct User u;
    int bid, uid;
    long bookPos = -1, userPos = -1;

    printf("Enter Book ID: ");
    scanf("%d", &bid);

    printf("Enter User ID: ");
    scanf("%d", &uid);

    rewind(fb);
    rewind(fu);

    // -------- Find Book --------
    while (fread(&b, sizeof(b), 1, fb)) {
        if (b.id == bid) {
            if (b.available == 1) {
                bookPos = ftell(fb) - sizeof(b);
            } else {
                printf("Book is already issued!\n");
                fclose(fb);
                fclose(fu);
                return;
            }
            break;
        }
    }

    // -------- Find User --------
    while (fread(&u, sizeof(u), 1, fu)) {
        if (u.userId == uid) {
            if (u.issuedBookId == -1) {
                userPos = ftell(fu) - sizeof(u);
            } else {
                printf("User already has a book!\n");
                fclose(fb);
                fclose(fu);
                return;
            }
            break;
        }
    }

    // -------- Validation --------
    if (bookPos == -1) {
        printf("Book not found!\n");
        fclose(fb);
        fclose(fu);
        return;
    }

    if (userPos == -1) {
        printf("User not found!\n");
        fclose(fb);
        fclose(fu);
        return;
    }

    // -------- Update Book --------
    fseek(fb, bookPos, SEEK_SET);
    b.available = 0;
    fwrite(&b, sizeof(b), 1, fb);

    // -------- Update User --------
    fseek(fu, userPos, SEEK_SET);
    u.issuedBookId = bid;

    printf("Enter Due Date (in days): ");
    scanf("%d", &u.dueDate);

    fwrite(&u, sizeof(u), 1, fu);

    printf("Book issued successfully!\n");

    fclose(fb);
    fclose(fu);
}

// ---------------- RETURN BOOK ----------------
void returnBook() {
    FILE *fb = fopen("books.dat", "rb+");
    FILE *fu = fopen("users.dat", "rb+");

    if (fb == NULL || fu == NULL) {
        printf("File error!\n");
        return;
    }

    struct Book b;
    struct User u;
    int uid, bid;
    long bookPos = -1, userPos = -1;

    printf("Enter User ID: ");
    scanf("%d", &uid);

    printf("Enter Book ID: ");
    scanf("%d", &bid);

    rewind(fb);
    rewind(fu);

    // -------- Find User --------
    while (fread(&u, sizeof(u), 1, fu)) {
        if (u.userId == uid) {
            userPos = ftell(fu) - sizeof(u);
            break;
        }
    }

    if (userPos == -1) {
        printf("User not found!\n");
        fclose(fb);
        fclose(fu);
        return;
    }

    // Check if user has this book
    if (u.issuedBookId != bid) {
        printf("This book was not issued to this user!\n");
        fclose(fb);
        fclose(fu);
        return;
    }

    // -------- Find Book --------
    rewind(fb);  // IMPORTANT FIX
    while (fread(&b, sizeof(b), 1, fb)) {
        if (b.id == bid) {
            bookPos = ftell(fb) - sizeof(b);
            break;
        }
    }

    if (bookPos == -1) {
        printf("Book not found!\n");
        fclose(fb);
        fclose(fu);
        return;
    }

    // -------- Overdue Check --------
    int returnDays;
    printf("Enter days taken to return: ");
    scanf("%d", &returnDays);

    if (returnDays > u.dueDate) {
        printf("\n⚠ OVERDUE!\n");
        printf("Book returned late!\n");
    } else {
        printf("Returned on time.\n");
    }

    // -------- Update Book --------
    fseek(fb, bookPos, SEEK_SET);
    b.available = 1;
    fwrite(&b, sizeof(b), 1, fb);

    // -------- Update User --------
    fseek(fu, userPos, SEEK_SET);
    u.issuedBookId = -1;
    u.dueDate = 0;
    fwrite(&u, sizeof(u), 1, fu);

    printf("Book returned successfully!\n");

    fclose(fb);
    fclose(fu);
}
