/*
 * =====================================================
 *       STUDENT MANAGEMENT SYSTEM IN C
 *   Features: Add | Delete | Update | Search | Display
 *   Storage : File Handling (students.dat)
 *   Author  : Code Alpha Internship Project
 * =====================================================
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// --- Constants ----------------------------------------
#define FILE_NAME "students.dat"
#define MAX_NAME  50
#define MAX_DEPT  30

// --- Student Structure --------------------------------
typedef struct {
    int    id;
    char   name[MAX_NAME];
    char   department[MAX_DEPT];
    int    age;
    float  gpa;
} Student;

// --- Function Prototypes ------------------------------
void    addStudent();
void    displayAll();
void    searchStudent();
void    updateStudent();
void    deleteStudent();
int     idExists(int id);
void    printHeader();
void    printStudent(Student s);
void    printDivider();
int     getStudentCount();

// -----------------------------------------------------
//  UTILITY FUNCTIONS
// -----------------------------------------------------

void printDivider() {
    printf("  +------+----------------------+--------------------+-----+-------+\n");
}

void printHeader() {
    printf("\n");
    printDivider();
    printf("  | %-4s | %-20s | %-18s | %-3s | %-5s |\n",
           "ID", "Name", "Department", "Age", "GPA");
    printDivider();
}

void printStudent(Student s) {
    printf("  | %-4d | %-20s | %-18s | %-3d | %-5.2f |\n",
           s.id, s.name, s.department, s.age, s.gpa);
}

// Count total records in file
int getStudentCount() {
    FILE *fp = fopen(FILE_NAME, "rb");
    if (!fp) return 0;
    fseek(fp, 0, SEEK_END);
    int count = ftell(fp) / sizeof(Student);
    fclose(fp);
    return count;
}

// Check if an ID already exists
int idExists(int id) {
    FILE *fp = fopen(FILE_NAME, "rb");
    if (!fp) return 0;
    Student s;
    while (fread(&s, sizeof(Student), 1, fp)) {
        if (s.id == id) { fclose(fp); return 1; }
    }
    fclose(fp);
    return 0;
}

// -----------------------------------------------------
//  1. ADD STUDENT
// -----------------------------------------------------
void addStudent() {
    FILE *fp = fopen(FILE_NAME, "ab");
    if (!fp) {
        printf("\n  [ERROR] Could not open file!\n");
        return;
    }

    Student s;
    printf("\n  +-----------------------------+");
    printf("\n  ¦        ADD NEW STUDENT      ¦");
    printf("\n  +-----------------------------+\n");

    // ID with duplicate check
    do {
        printf("  Enter Student ID   : ");
        scanf("%d", &s.id);
        if (idExists(s.id))
            printf("  [!] ID %d already exists. Try another.\n", s.id);
    } while (idExists(s.id));

    printf("  Enter Name         : ");
    scanf(" %[^\n]", s.name);

    printf("  Enter Department   : ");
    scanf(" %[^\n]", s.department);

    printf("  Enter Age          : ");
    scanf("%d", &s.age);

    printf("  Enter GPA (0-4.0)  : ");
    scanf("%f", &s.gpa);

    // Clamp GPA
    if (s.gpa < 0) s.gpa = 0;
    if (s.gpa > 4.0) s.gpa = 4.0;

    fwrite(&s, sizeof(Student), 1, fp);
    fclose(fp);

    printf("\n  [?] Student added successfully!\n");
}

// -----------------------------------------------------
//  2. DISPLAY ALL STUDENTS
// -----------------------------------------------------
void displayAll() {
    FILE *fp = fopen(FILE_NAME, "rb");
    if (!fp) {
        printf("\n  [!] No records found. File does not exist yet.\n");
        return;
    }

    Student s;
    int count = 0;

    printf("\n  +-----------------------------+");
    printf("\n  ¦      ALL STUDENT RECORDS    ¦");
    printf("\n  +-----------------------------+");

    printHeader();

    while (fread(&s, sizeof(Student), 1, fp)) {
        printStudent(s);
        count++;
    }

    printDivider();
    printf("\n  Total Records: %d\n", count);
    fclose(fp);

    if (count == 0)
        printf("  [!] No students in the database.\n");
}

// -----------------------------------------------------
//  3. SEARCH STUDENT
// -----------------------------------------------------
void searchStudent() {
    printf("\n  +-----------------------------+");
    printf("\n  ¦       SEARCH STUDENT        ¦");
    printf("\n  +-----------------------------+\n");
    printf("  Search by:\n  1. Student ID\n  2. Name\n");
    printf("  Choice: ");

    int opt;
    scanf("%d", &opt);

    FILE *fp = fopen(FILE_NAME, "rb");
    if (!fp) {
        printf("\n  [!] No records found.\n");
        return;
    }

    Student s;
    int found = 0;

    if (opt == 1) {
        int id;
        printf("  Enter Student ID: ");
        scanf("%d", &id);

        printHeader();
        while (fread(&s, sizeof(Student), 1, fp)) {
            if (s.id == id) {
                printStudent(s);
                found = 1;
                break;
            }
        }
        printDivider();

    } else if (opt == 2) {
        char name[MAX_NAME];
        printf("  Enter Name (or part of it): ");
        scanf(" %[^\n]", name);

        printHeader();
        while (fread(&s, sizeof(Student), 1, fp)) {
            // Case-insensitive partial match
            char sLower[MAX_NAME], qLower[MAX_NAME];
            strcpy(sLower, s.name);
            strcpy(qLower, name);
            for (int i = 0; sLower[i]; i++) if (sLower[i]>='A'&&sLower[i]<='Z') sLower[i]+=32;
            for (int i = 0; qLower[i]; i++) if (qLower[i]>='A'&&qLower[i]<='Z') qLower[i]+=32;

            if (strstr(sLower, qLower)) {
                printStudent(s);
                found++;
            }
        }
        printDivider();

    } else {
        printf("  [!] Invalid option.\n");
        fclose(fp);
        return;
    }

    fclose(fp);

    if (!found)
        printf("\n  [!] No matching student found.\n");
    else
        printf("\n  [?] %d record(s) found.\n", found);
}

// -----------------------------------------------------
//  4. UPDATE STUDENT
// -----------------------------------------------------
void updateStudent() {
    printf("\n  +-----------------------------+");
    printf("\n  ¦       UPDATE STUDENT        ¦");
    printf("\n  +-----------------------------+\n");

    int id;
    printf("  Enter Student ID to update: ");
    scanf("%d", &id);

    if (!idExists(id)) {
        printf("\n  [!] Student with ID %d not found.\n", id);
        return;
    }

    FILE *fp = fopen(FILE_NAME, "rb+");
    if (!fp) {
        printf("\n  [ERROR] Could not open file!\n");
        return;
    }

    Student s;
    int updated = 0;

    while (fread(&s, sizeof(Student), 1, fp)) {
        if (s.id == id) {
            printf("\n  Current Record:\n");
            printHeader();
            printStudent(s);
            printDivider();

            printf("\n  Enter new details (press Enter to keep current):\n");

            char buf[MAX_NAME];

            printf("  New Name [%s]: ", s.name);
            scanf(" %[^\n]", buf);
            if (strlen(buf) > 0) strcpy(s.name, buf);

            printf("  New Department [%s]: ", s.department);
            scanf(" %[^\n]", buf);
            if (strlen(buf) > 0) strcpy(s.department, buf);

            printf("  New Age [%d]: ", s.age);
            int newAge;
            scanf("%d", &newAge);
            if (newAge > 0) s.age = newAge;

            printf("  New GPA [%.2f]: ", s.gpa);
            float newGpa;
            scanf("%f", &newGpa);
            if (newGpa >= 0 && newGpa <= 4.0) s.gpa = newGpa;

            // Move pointer back one record and overwrite
            fseek(fp, -(long)sizeof(Student), SEEK_CUR);
            fwrite(&s, sizeof(Student), 1, fp);
            updated = 1;
            break;
        }
    }

    fclose(fp);
    if (updated)
        printf("\n  [?] Student record updated successfully!\n");
}

// -----------------------------------------------------
//  5. DELETE STUDENT
// -----------------------------------------------------
void deleteStudent() {
    printf("\n  +-----------------------------+");
    printf("\n  ¦       DELETE STUDENT        ¦");
    printf("\n  +-----------------------------+\n");

    int id;
    printf("  Enter Student ID to delete: ");
    scanf("%d", &id);

    if (!idExists(id)) {
        printf("\n  [!] Student with ID %d not found.\n", id);
        return;
    }

    // Confirm deletion
    char confirm;
    printf("  Are you sure you want to delete ID %d? (y/n): ", id);
    scanf(" %c", &confirm);
    if (confirm != 'y' && confirm != 'Y') {
        printf("  [!] Deletion cancelled.\n");
        return;
    }

    FILE *fp    = fopen(FILE_NAME, "rb");
    FILE *temp  = fopen("temp.dat", "wb");

    if (!fp || !temp) {
        printf("\n  [ERROR] File operation failed!\n");
        return;
    }

    Student s;
    int deleted = 0;

    while (fread(&s, sizeof(Student), 1, fp)) {
        if (s.id == id) {
            deleted = 1;      // skip this record
        } else {
            fwrite(&s, sizeof(Student), 1, temp);
        }
    }

    fclose(fp);
    fclose(temp);

    remove(FILE_NAME);
    rename("temp.dat", FILE_NAME);

    if (deleted)
        printf("\n  [?] Student with ID %d deleted successfully!\n", id);
    else
        printf("\n  [!] Student not found.\n");
}

// -----------------------------------------------------
//  MAIN MENU
// -----------------------------------------------------
int main() {
    int choice;

    printf("\n+--------------------------------------+");
    printf("\n¦     STUDENT MANAGEMENT SYSTEM  (C)   ¦");
    printf("\n¦         Code Alpha Internship         ¦");
    printf("\n+--------------------------------------+");

    do {
        printf("\n\n  +------------------------------+");
        printf("\n  ¦           MAIN MENU          ¦");
        printf("\n  +------------------------------¦");
        printf("\n  ¦  1. Add    Student            ¦");
        printf("\n  ¦  2. Display All Students      ¦");
        printf("\n  ¦  3. Search Student            ¦");
        printf("\n  ¦  4. Update Student            ¦");
        printf("\n  ¦  5. Delete Student            ¦");
        printf("\n  ¦  0. Exit                      ¦");
        printf("\n  +------------------------------+");
        printf("\n  Total Records: %d", getStudentCount());
        printf("\n  Your Choice  : ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent();    break;
            case 2: displayAll();    break;
            case 3: searchStudent(); break;
            case 4: updateStudent(); break;
            case 5: deleteStudent(); break;
            case 0: printf("\n  Goodbye! Data saved to '%s'.\n\n", FILE_NAME); break;
            default: printf("\n  [!] Invalid choice. Try again.\n");
        }

    } while (choice != 0);

    return 0;
}
