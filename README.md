# Student Management System in C

A menu-driven console application built in C that performs
complete CRUD operations on student records using structures
and binary file handling for permanent data storage.

## Features
- ✅ Add new student records with duplicate ID validation
- ✅ Display all students in a formatted table
- ✅ Search by Student ID or partial name (case-insensitive)
- ✅ Update any field of an existing record in-place
- ✅ Delete records safely using temp file replacement
- ✅ Live record count shown on every menu load
- ✅ Data persists between sessions via binary file (students.dat)

## Tech Used
- Language  : C (C99 standard)
- Storage   : Binary File Handling (.dat)
- Concepts  : Structures, fread/fwrite, fseek, CRUD operations

## How to Run

### Compile
gcc student_management.c -o sms

### Execute
./sms

## Project Structure
student_management.c   →  Main source file
students.dat           →  Auto-generated data file (created on first Add)

## Menu Options
1 → Add Student
2 → Display All Students
3 → Search Student (by ID or Name)
4 → Update Student
5 → Delete Student
0 → Exit

## Data Fields per Student
- Student ID     (unique integer)
- Full Name      (up to 50 characters)
- Department     (up to 30 characters)
- Age            (integer)
- GPA            (float, clamped 0.0 – 4.0)

## Key Concepts Demonstrated
- typedef struct for data modeling
- Binary file I/O (fread, fwrite, fseek)
- Safe delete using temp file and rename()
- In-place record update using fseek + fwrite
- Partial string matching with strstr()

## Author
Built during my virtual C Language Internship at Code Alpha.
