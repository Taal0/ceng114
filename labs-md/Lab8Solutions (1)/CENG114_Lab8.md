CENG114 — Lab 8: Exceptions & Text File I/O

Ankara Yıldırım Beyazıt University
Department of Computer Engineering

CENG114 – Computer Programming II
Lab Guide #8: Exception Handling and Text File I/O

Instructor: Yusuf Evren AYKAÇ

Week: 12 (Spring 2025–2026)

Assistants: Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

Lab: 8

1. Learning Objectives

By the end of this lab, you will be able to:

•  Distinguish between checked and unchecked exceptions, and decide when each is appropriate.
•  Use try–catch–finally blocks correctly, including the order of multiple catch clauses.
•  Declare exceptions with the throws keyword and propagate them across method calls.
•  Define and throw your own custom exception classes that extend Exception or RuntimeException.
•  Read from and write to text files using FileReader/BufferedReader and FileWriter/PrintWriter.
•  Use the try-with-resources statement to manage I/O resources safely without leaks.

2. Setup

1. Create a project folder named lab8 and place your .java files inside it.
2. Create a sub-folder lab8/data for the input files used by Q2 and Q3 (sample contents are provided

below — copy them verbatim).

3. Compile and run from the lab8 folder so that relative paths like data/students.txt resolve correctly.
4. Each question must have its own driver class with a main method (e.g. Q1Main.java, Q2Main.java,

Q3Main.java).

AYBU · Department of Computer Engineering · Page 1 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

Question 1

Problem

Write a program Q1Main that repeatedly reads a line of input from the user and accumulates a running

sum of valid integers. The user types done to finish. Any non-integer input that is not done must be

reported with a friendly message and skipped — the program must not crash. After the user types done,

save the total sum to a file named sum.txt.

Requirements

•  Use Scanner to read input from System.in.
•  Catch NumberFormatException for invalid integer inputs and print: Invalid input: '<value>' is not an

integer. Skipped.

•  Use a finally block to print Program terminated. as the very last line, no matter what.
•  Open the output file using try-with-resources (PrintWriter wrapping a FileWriter) so that the file is

always closed.

•  Single source file: Q1Main.java.

Example run
 CONSOLE
Enter integers (type 'done' to finish):
> 10
> 25
> abc
Invalid input: 'abc' is not an integer. Skipped.
> 7
> -3
> 12.5
Invalid input: '12.5' is not an integer. Skipped.
> done

Total sum: 39
Sum saved to sum.txt
Program terminated.

Contents of sum.txt after the run
 SUM.TXT
Total sum: 39

Hints

•  Read each line with scanner.nextLine().trim() and check for the literal string "done" before

parsing.

•  Use Integer.parseInt(line) inside the try block — it throws NumberFormatException for any

non-integer value, including decimals.

•  The finally block runs even if you return from the try block, so it is the right place for the closing

message.

AYBU · Department of Computer Engineering · Page 2 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

Before you pass Q2 and Q3 → Text files in practice: .txt vs .log

They are the same thing as far as Java is concerned. FileWriter, BufferedReader, and every other I/O

class read and write either one identically — the extension is just a convention that tells humans (and

log-viewing tools) what the file is for. The two extensions exist because the files play very different roles

in a program.

Aspect

Purpose

Audience

Lifecycle

.txt

.log

Primary data the program reads or
produces.

Chronological record of events that
happened during execution.

End user, or the next program in a
pipeline.

Developer, sysadmin, auditor — someone
debugging or reviewing.

Read once, sometimes edited; usually
overwritten on each run.

Append-only; continuously grows; often
rotated and archived.

Typical format

Whatever the data needs (CSV, JSON,
prose, ...).

Structured lines: timestamp + severity +
message.

Where it lives

Project folder, /data/, output directory.

Tooling

Text editors, spreadsheets, CSV parsers.

Dedicated logs/ folder; on Linux servers,
/var/log/.

tail -f, grep, log aggregators (Splunk, ELK,
Loki).

In this lab

•  students.txt, valid_students.txt, accounts.txt, transactions.txt, final_balances.txt,
sum.txt are data files — they hold records the program reads or produces. A user opens them to
see results.

•  errors.log and audit.log are event records — they trace what happened while the program
ran, one line per event. A grader (or, in the bank scenario, an auditor) opens them to see the
program's reasoning, not its output.

Practical tip — append mode

The FileWriter constructor has a two-argument form that controls whether the existing content is

overwritten or appended to:

 JAVA
new FileWriter("data/audit.log")          // overwrites the file every run
new FileWriter("data/audit.log", true)    // APPENDS - correct for a real log

In this lab the overwriting form is used because each invocation processes the inputs from scratch and

the expected outputs must be deterministic for grading. In a long-running real system you would set the

second argument to true so that yesterday's audit trail is preserved when the program restarts.

AYBU · Department of Computer Engineering · Page 3 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

Practical tip — timestamps

A line in a production log usually starts with a timestamp. Java's java.time API makes this a one-liner:

 JAVA
import java.time.LocalDateTime;

audit.printf("%s | %s | %-8s | %s%n",
             LocalDateTime.now(), id, type, status);
// 2026-05-03T14:22:11.345 | ACC001 | DEPOSIT  | OK

The lab guide's expected output does not include timestamps so that the example runs stay reproducible

— but adding them is a five-line change, and seeing how a real audit log looks is part of the point.

Question 2

Problem

You are given a CSV-like text file data/students.txt where each line has the format FullName,grade.

Some entries are malformed: the grade may not be a number, or it may be outside the valid range [0,

100]. Write a program Q2Main that processes the file and produces two output files:

•  data/valid_students.txt — only the lines that pass all validations.
•  data/errors.log — one line per rejected record, including the original line number and the

reason.

Required classes

•  InvalidGradeException — a checked custom exception (extends Exception). Constructor that takes

a message and forwards it to super.

•  Q2Main — driver with main plus a helper method static int validateGrade(int grade)

throws InvalidGradeException that returns the grade if valid or throws otherwise.

Sample input — data/students.txt
 STUDENTS.TXT
Ada Lovelace,95
Alan Turing,88
Grace Hopper,150
John Doe,seventy
Linus Torvalds,72
Margaret Hamilton,-5
Donald Knuth,100

Required behaviour

•  Read the file with try-with-resources around a BufferedReader (do NOT close the reader manually).
•  For each line: split on the first comma; trim both parts.
•  If the second part is not a valid integer, catch NumberFormatException and log a parse error.
•  If it is a valid integer, call validateGrade — catch InvalidGradeException and log the message.

AYBU · Department of Computer Engineering · Page 4 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

•  Valid records go to data/valid_students.txt (preserving the original line, comma included).
•  Errors go to data/errors.log with the format: Line <n>: <reason> [<student name>].
•  At the end, print a one-line summary: Done. <validCount> valid, <errorCount> errors.

Example run
 CONSOLE
Processing data/students.txt ...
Line 1  Ada Lovelace, 95         -> OK
Line 2  Alan Turing, 88          -> OK
Line 3  Grace Hopper, 150        -> ERROR: Grade 150 is out of valid range (0-100).
Line 4  John Doe                 -> ERROR: Cannot parse grade 'seventy' as integer.
Line 5  Linus Torvalds, 72       -> OK
Line 6  Margaret Hamilton, -5    -> ERROR: Grade -5 is out of valid range (0-100).
Line 7  Donald Knuth, 100        -> OK

Done. 4 valid, 3 errors.
Output written to: data/valid_students.txt
Errors written to: data/errors.log

Contents of data/valid_students.txt
 VALID_STUDENTS.TXT
Ada Lovelace,95
Alan Turing,88
Linus Torvalds,72
Donald Knuth,100

Contents of data/errors.log
 ERRORS.LOG
Line 3: Grade 150 is out of valid range (0-100). [Grace Hopper]
Line 4: Cannot parse grade 'seventy' as integer. [John Doe]
Line 6: Grade -5 is out of valid range (0-100). [Margaret Hamilton]

Why a checked exception?
Invalid grades are an expected part of real-world data — the caller has a sensible recovery (skip the
row, log it). That is exactly what checked exceptions are designed for, and the throws clause
documents the contract for whoever calls validateGrade.

AYBU · Department of Computer Engineering · Page 5 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

Question 3

Problem

You are writing the back-end of a tiny bank. Two text files describe the work:

•  data/accounts.txt — initial account balances, one per line: ACCOUNT_ID,balance.
•  data/transactions.txt — operations to apply, one per line: ACCOUNT_ID,TYPE,amount where

TYPE is DEPOSIT or WITHDRAW.

Process every transaction in order, updating the in-memory map of balances. Each transaction either

succeeds or fails for one specific reason — and every outcome must be written to data/audit.log.

After processing all transactions, write the final balances to data/final_balances.txt.

Required classes

•  AccountNotFoundException — checked, raised when the account ID does not exist.
•  InsufficientFundsException — checked, raised when a withdrawal would make the balance

negative.

•  InvalidTransactionException — checked, raised for non-positive amounts or unknown transaction

types.

•  Q3Main — driver class. Must contain static void processTransaction(Map<String,Double>

accounts, String id, String type, double amount) throws
AccountNotFoundException, InsufficientFundsException,
InvalidTransactionException.

Validation order inside processTransaction

1. If amount <= 0 or type is neither "DEPOSIT" nor "WITHDRAW" → throw

InvalidTransactionException.

2. Else if the account ID is not in the map → throw AccountNotFoundException.
3. Else if it is a WITHDRAW and amount > current balance → throw InsufficientFundsException.
4. Otherwise, update the balance and return normally.

Sample input — data/accounts.txt
 ACCOUNTS.TXT
ACC001,1500.00
ACC002,3200.50
ACC003,80.00

Sample input — data/transactions.txt
 TRANSACTIONS.TXT
ACC001,DEPOSIT,500.00
ACC002,WITHDRAW,1000.00
ACC003,WITHDRAW,500.00
ACC999,DEPOSIT,200.00
ACC001,WITHDRAW,-50.00
ACC002,DEPOSIT,750.00
ACC001,TRANSFER,100.00

AYBU · Department of Computer Engineering · Page 6 of 7

CENG114 — Lab 8: Exceptions & Text File I/O

Required behaviour

•  Use try-with-resources to load accounts.txt into a HashMap<String, Double>.
•  Use a single try-with-resources statement that opens BOTH the BufferedReader for transactions.txt

AND the PrintWriter for audit.log together.

•  Inside the loop, wrap each call to processTransaction in its own try–catch with multiple catch

blocks. List the most specific exceptions before the generic Exception catch.

•  Every successful transaction is printed as [OK]; every failed transaction is printed as [FAIL] with the

exception class name and message. The same line is appended to audit.log.

•  After the loop ends, write the final state of the HashMap to final_balances.txt, sorted by account

ID.

•  The program must NEVER terminate due to an uncaught exception — even malformed input lines

must be caught and reported.

Example run
 CONSOLE
Loaded 3 accounts from data/accounts.txt.
Processing transactions ...

[OK]    ACC001 DEPOSIT  500.00  -> balance 2000.00
[OK]    ACC002 WITHDRAW 1000.00 -> balance 2200.50
[FAIL]  ACC003 WITHDRAW 500.00  -> InsufficientFundsException: Cannot withdraw 500.00 from ACC003 (balance 80.00).
[FAIL]  ACC999 DEPOSIT  200.00  -> AccountNotFoundException: Account 'ACC999' does not exist.
[FAIL]  ACC001 WITHDRAW -50.00  -> InvalidTransactionException: Amount must be positive, got -50.00.
[OK]    ACC002 DEPOSIT  750.00  -> balance 2950.50
[FAIL]  ACC001 TRANSFER 100.00  -> InvalidTransactionException: Unknown transaction type 'TRANSFER'.

Summary: 3 successful, 4 failed (out of 7).
Final balances written to: data/final_balances.txt
Audit log written to:      data/audit.log

Contents of data/final_balances.txt
 FINAL_BALANCES.TXT
ACC001,2000.00
ACC002,2950.50
ACC003,80.00

Contents of data/audit.log
 AUDIT.LOG
ACC001 | DEPOSIT  | 500.00   | OK   | new balance 2000.00
ACC002 | WITHDRAW | 1000.00  | OK   | new balance 2200.50
ACC003 | WITHDRAW | 500.00   | FAIL | InsufficientFundsException: Cannot withdraw 500.00 from ACC003 (balance 80.00).
ACC999 | DEPOSIT  | 200.00   | FAIL | AccountNotFoundException: Account 'ACC999' does not exist.
ACC001 | WITHDRAW | -50.00   | FAIL | InvalidTransactionException: Amount must be positive, got -50.00.
ACC002 | DEPOSIT  | 750.00   | OK   | new balance 2950.50
ACC001 | TRANSFER | 100.00   | FAIL | InvalidTransactionException: Unknown transaction type 'TRANSFER'.

AYBU · Department of Computer Engineering · Page 7 of 7

