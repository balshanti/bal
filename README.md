as2
Create below tables with appropriate constraints like primary key, foreign key, check
constraints, not null etc.
● Account(Acc_no, branch_name,balance)
● branch(branch_name,branch_city,assets)
● customer(cust_name,cust_street,cust_city)
● Depositor(cust_name,acc_no)
● Loan(loan_no,branch_name,amount)
● Borrower(cust_name,loan_no)
CREATE TABLE branch205 (
branch_name VARCHAR(50) PRIMARY KEY,
branch_city VARCHAR(50),
assets DECIMAL(15, 2)
);

CREATE TABLE Account205 (
Acc_no INT PRIMARY KEY,
branch_name VARCHAR(50) NOT NULL,
balance DECIMAL(10, 2),
FOREIGN KEY (branch_name) REFERENCES branch205(branch_name)
);
CREATE TABLE customer205 (
cust_name VARCHAR(50) PRIMARY KEY,
cust_street VARCHAR(100),
cust_city VARCHAR(50)
);
CREATE TABLE Depositor205 (
cust_name VARCHAR(50),
acc_no INT,
PRIMARY KEY (cust_name, acc_no),
FOREIGN KEY (cust_name) REFERENCES customer205(cust_name),
FOREIGN KEY (acc_no) REFERENCES Account205(Acc_no)
);
CREATE TABLE Loan205 (
loan_no INT PRIMARY KEY,
branch_name VARCHAR(50) NOT NULL,

amount DECIMAL(10, 2),
FOREIGN KEY (branch_name) REFERENCES branch205(branch_name)
);
CREATE TABLE Borrower205 (
cust_name VARCHAR(50),
loan_no INT,
PRIMARY KEY (cust_name, loan_no),
FOREIGN KEY (cust_name) REFERENCES customer205(cust_name),
FOREIGN KEY (loan_no) REFERENCES Loan205(loan_no)
);

Inserting values into the tables:
INSERT INTO branch205 (branch_name, branch_city, assets) VALUES
('Akurdi', 'Pune', 100000.00);
INSERT INTO branch205 (branch_name, branch_city, assets) VALUES
('Nigdi', 'Pune', 150000.00);
INSERT INTO branch205 (branch_name, branch_city, assets) VALUES
('Pimpri', 'PCMC', 200000.00);
INSERT INTO branch205 (branch_name, branch_city, assets) VALUES
('Chinchwad', 'PCMC', 180000.00);
INSERT INTO branch205 (branch_name, branch_city, assets) VALUES
('Wakad', 'Pune', 120000.00);

INSERT INTO customer205 (cust_name, cust_street, cust_city) VALUES
('Amit', 'Street1', 'Pune');
INSERT INTO customer205 (cust_name, cust_street, cust_city) VALUES
('Rahul', 'Street2', 'Pune');
INSERT INTO customer205 (cust_name, cust_street, cust_city) VALUES
('Anjali', 'Street3', 'PCMC');
INSERT INTO customer205 (cust_name, cust_street, cust_city) VALUES
('Priya', 'Street4', 'PCMC');

INSERT INTO customer205 (cust_name, cust_street, cust_city) VALUES
('Sandeep', 'Street5', 'Pune');

INSERT INTO Account205 (Acc_no, branch_name, balance) VALUES
(101, 'Akurdi', 5000.00);
INSERT INTO Account205 (Acc_no, branch_name, balance) VALUES
(102, 'Nigdi', 7000.00);
INSERT INTO Account205 (Acc_no, branch_name, balance) VALUES
(103, 'Pimpri', 3000.00);
INSERT INTO Account205 (Acc_no, branch_name, balance) VALUES
(104, 'Chinchwad', 9000.00);
INSERT INTO Account205 (Acc_no, branch_name, balance) VALUES
(105, 'Wakad', 6000.00);

INSERT INTO Loan205 (loan_no, branch_name, amount) VALUES
(201, 'Akurdi', 20000.00);
INSERT INTO Loan205 (loan_no, branch_name, amount) VALUES
(202, 'Nigdi', 30000.00);
INSERT INTO Loan205 (loan_no, branch_name, amount) VALUES
(203, 'Pimpri', 15000.00);
INSERT INTO Loan205 (loan_no, branch_name, amount) VALUES
(204, 'Chinchwad', 25000.00);
INSERT INTO Loan205 (loan_no, branch_name, amount) VALUES
(205, 'Wakad', 18000.00);

INSERT INTO Depositor205 (cust_name, acc_no) VALUES
('Amit', 101);
INSERT INTO Depositor205 (cust_name, acc_no) VALUES
('Rahul', 102);

INSERT INTO Depositor205 (cust_name, acc_no) VALUES
('Anjali', 103);
INSERT INTO Depositor205 (cust_name, acc_no) VALUES
('Priya', 104);
INSERT INTO Depositor205 (cust_name, acc_no) VALUES
('Sandeep', 105);

INSERT INTO Borrower205 (cust_name, loan_no) VALUES
('Amit', 201);
INSERT INTO Borrower205 (cust_name, loan_no) VALUES
('Rahul', 202);
INSERT INTO Borrower205 (cust_name, loan_no) VALUES
('Anjali', 203);
INSERT INTO Borrower205 (cust_name, loan_no) VALUES
('Priya', 204);
INSERT INTO Borrower205 (cust_name, loan_no) VALUES
('Sandeep', 205);

Solve following query:
Q1. Find the names of all branches in loan relation.
SELECT DISTINCT branch_name FROM Loan205;
Q2. Find all loan numbers for loans made at Akurdi Branch with loan
amount > 12000.
SELECT loan_no FROM Loan205 WHERE branch_name = 'Akurdi' AND amount >
12000;

Q3. Find all customers who have a loan from bank. Find their
names,loan_no and loan amount.
SELECT c.cust_name, l.loan_no, l.amount
FROM customer205 c
JOIN Borrower205 b ON c.cust_name = b.cust_name
JOIN Loan205 l ON b.loan_no = l.loan_no;

Q4. List all customers in alphabetical order who have loan from Akurdi
branch.
SELECT c.cust_name
FROM customer c
JOIN Borrower b ON c.cust_name = b.cust_name
JOIN Loan l ON b.loan_no = l.loan_no
WHERE l.branch_name = 'Akurdi'
ORDER BY c.cust_name;
Q5. Find all customers who have an account or loan or both at bank.
SELECT DISTINCT cust_name FROM (
SELECT cust_name FROM Depositor
UNION
SELECT cust_name FROM Borrower
) AS All_Customers;

Q6. Find all customers who have both account and loan at the bank.
SELECT DISTINCT c.cust_name
FROM customer205 c
JOIN Depositor205 d ON c.cust_name = d.cust_name
JOIN Borrower205 b ON c.cust_name = b.cust_name;
Q7. Find all customers who have account but no loan at the bank.
SELECT DISTINCT c.cust_name
FROM customer205 c
JOIN Depositor205 d ON c.cust_name = d.cust_name
LEFT JOIN Borrower205 b ON c.cust_name = b.cust_name
WHERE b.cust_name IS NULL;

Q8. Find average account balance at Akurdi branch.
SELECT AVG(a.balance) AS avg_balance
FROM Account205 a
WHERE a.branch_name = 'Akurdi';
Q9. Find the average account balance at each branch.
SELECT branch_name, AVG(balance) AS avg_balance
FROM Account205
GROUP BY branch_name;
Q10. Find the number of depositors at each branch.
SELECT a.branch_name, COUNT(d.cust_name) AS num_depositors
FROM Account205 a
JOIN Depositor205 d ON a.Acc_no = d.acc_no
GROUP BY a.branch_name;
Q11. Find the branches where the average account balance is greater than
12000.
SELECT branch_name
FROM Account205
GROUP BY branch_name
HAVING AVG(balance) > 12000;
Q12. Find the number of tuples in the customer relation.
SELECT COUNT(*) AS num_tuples
FROM customer205;
Q13. Calculate total loan amount given by the bank.
SELECT SUM(amount) AS total_loan_amount
FROM Loan205;
Q14. Delete all loans with loan amount between 1300 and 1500.
DELETE FROM Loan205
WHERE amount BETWEEN 1300 AND 1500;

Q15. Delete all tuples at every branch located in Nigdi.
DELETE FROM branch205
WHERE branch_city = 'Nigdi';
Q16. Create a synonym for the customer table as cust.
CREATE SYNONYM cust FOR customer205;
Q17. Create sequence roll_seq and use it in the student table for the
roll_no column.
CREATE SEQUENCE roll_seq
START WITH 1
INCREMENT BY 1
NOMAXVALUE;
ALTER TABLE student
ADD roll_no INT;
UPDATE student
SET roll_no = roll_seq.NEXTVAL;

as3

1. Create following Tables
cust_mstr(cust_no,fname,lname)
add_dets(code_no,add1,add2,state,city,pincode)
Retrieve the address of customer Fname as 'xyz' and Lname as 'pqr’.
SELECT c.fname,c.lname,a.add1,a.add2,a.state ,a.city ,a.pincode
FROM add_dets a
JOIN cust_mstr c ON a.cust_no = c.cust_no
WHERE c.fname = 'xyz' AND c.lname = 'pqr';
2.Create following Tables
cust_mstr(cust_no,fname,lname)
acc_fd_cust_dets(cust_no,acc_fd_no)
fd_dets(fd_sr_no,amt)
List the customer holding fixed deposit of amount more than 5000.
SELECT c.* FROM cust_mstr c
LEFT JOIN acc_fd_cust_dets abcd ON c.cust_no = afcd.cust_no
LEFT JOIN fd_dets fd ON afcd.acc_fd_no = fd.fd_sr_no
WHERE fd.amt > 5000
3. Create following Tables
emp_mstr(e_mpno,f_name,l_name,m_name,dept,desg,branch_no)
branch_mstr(name,b_no)
List the employee details along with branch names to which they belong.
SELECT em.* , bm.name
FROM emp_mstr em
LEFT JOIN branch_mstr bm ON em.branch_no = bm.b_no;
4.Create following Tables
emp_mstr(emp_no,f_name,l_name,m_name,dept)
cntc_dets(code_no,cntc_type,cntc_data)
List the employee details along with contact details using left outer join & right
Outer join.
i.LEFT JOIN ::
SELECT emp.* ,cd.cntc_type , cd.cntc_data

FROM emp_mstr emp
LEFT JOIN cntc_dets cd ON emp.e_mpno = cd.cust_no;
ii.RIGHT JOIN ::
SELECT emp.* ,cd.cntc_type , cd.cntc_data
FROM emp_mstr emp
RIGHT JOIN cntc_dets cd ON emp.e_mpno = cd.cust_no;
5. Create following Tables
cust_mstr(cust_no,fname,lname)
add_dets(cust_no,pincode)
List the customer who do not have bank branches in their vicinity.
SELECT cm.* FROM cust_mstr cm
LEFT JOIN add_dets ad ON cm.cust_no = ad.cust_no
WHERE ad.cust_no is NULL;
6. a) Create View on borrower table by selecting any two columns and perform
insert update delete operations.
b) Create view on borrower and depositor table by selecting any one column
from each table perform insert update delete operations.
c) Create update table view on borrower table by selecting any two columns
and perform insert update delete operations.

a) SELECT * from borrower;
CREATE VIEW V AS
SELECT cust_name, loan_no
FROM borrower;
INSERT INTO v
VALUES ('Ajay' ,102);
UPDATE v
SET cust_name = 'Rahul'
WHERE cust_name = 'Ram';
DELETE FROM v WHERE cust_name = ‘Ajay’;

ass4
1. Consider table Stud(Roll, Att,Status) Write a PL/SQL block for following requirements
and handle the exceptions. Roll no. of students will be entered by the user. Attendance of
roll no. entered by the user will be checked in the Stud table. If attendance is less than 75%
then display the message “Term not granted” and set the status in the stud table as “D”.
Otherwise display message “Term granted” and set the status in stud table as “ND”
Ans :
create table stud_272
(
roll int primary key,
att int,
status varchar(7)
);
insert into stud_272 values(1,23,'-');
insert into stud_272 values(2,84,'-');
insert into stud_272 values(3,57,'-');
insert into stud_272 values(4,90,'-');
insert into stud_272 values(5,98,'-');
insert into stud_272 values(6,66,'-');
insert into stud_272 values(7,76,'-');
Select * from stud_272;

PL/SQL Block :
Declare
mroll number(5);
matt number(5);
Begin
mroll:=&mroll;
select att into matt from stud_272 where roll = mroll;
if matt<75 then
dbms_output.put_line('Term not granted');
update stud_272 set status='D' where roll=mroll;
else
dbms_output.put_line('Term granted');
update stud_272 set status='ND' where roll=mroll;
End if;
End;
/
Output :
Enter value for mroll: 1
old 5: mroll:=&mroll;
new 5: mroll:=1;
Term not granted
Enter value for mroll: 2
old 5: mroll:=&mroll;
new 5: mroll:=2;
Term granted
Enter value for mroll: 3
Term not granted
Enter value for mroll: 4
Term granted
Enter value for mroll: 5
Term granted
Enter value for mroll: 6
Term not granted

Enter value for mroll: 7
Term granted
Updated Table :

_______________________________________________________________________________
2. Write a PL/SQL block for following requirements using user defined exception handling.
The account_master table records the current balance for an account, which is updated
whenever any deposits or withdrawals take place. If the withdrawal attempted is more than
the current balance held in the account. The user defined exception is raised, displaying an
appropriate message. Write a PL/SQL block for the above requirement using user defined
exception handling.
Ans :
create table account_272
(
acc_no int primary key,
name varchar(10),
balance int
);
insert into account_272 values(45231,'Imtiyaz',5000);
insert into account_272 values(98233,'Pragati',4300);
insert into account_272 values(91231,'Tanvi',12000);
insert into account_272 values(81721,'Rutika',5600);
insert into account_272 values(98212,'Mahima',21000);
Account_272 Table :

PL/SQL block :
DECLARE
mbal NUMBER(10);
macc NUMBER(10);
mamt NUMBER(10);
choice NUMBER(5);
low_balance_exception EXCEPTION;
inappropriate_exception EXCEPTION;
BEGIN
macc := &macc;
mamt := &mamt;
choice := &choice;
SELECT balance INTO mbal FROM account_272 WHERE acc_no = macc;
IF choice = 1 THEN
IF mbal >= mamt THEN
UPDATE account_272 SET balance = (mbal - mamt) WHERE acc_no = macc;
DBMS_OUTPUT.PUT_LINE('Balance updated');
ELSE
RAISE low_balance_exception;
END IF;
ELSE
IF mamt > 0 THEN
UPDATE account_272 SET balance = (mbal + mamt) WHERE acc_no = macc;
DBMS_OUTPUT.PUT_LINE('Balance updated');
ELSE
RAISE inappropriate_exception;
END IF;
END IF;

EXCEPTION
WHEN low_balance_exception THEN
DBMS_OUTPUT.PUT_LINE('You do not have sufficient balance to withdraw it.');
WHEN inappropriate_exception THEN
DBMS_OUTPUT.PUT_LINE('Deposit amount should be greater than 0');
END;
/
Output :
Enter value for macc: 45231
Enter value for mamt: 4000
Enter value for choice: 1
Balance updated
select * from account_272;

Enter value for macc: 91231
Enter value for mamt: 15000
Enter value for choice: 1
You do not have sufficient balance to withdraw it.
Enter value for macc: 98212
Enter value for mamt: -23
Enter value for choice: 2
Deposit amount should be greater than 0

Enter value for macc: 98233
old 11: macc := &macc;
new 11: macc := 98233;
Enter value for mamt: 2500
old 12: mamt := &mamt;
new 12: mamt := 2500;
Enter value for choice: 2
old 13: choice := &choice;
new 13: choice := 2;

Balance updated
Updated table :

Q.3 1. Borrower(Roll_no, Name, Date_of_Issue, Name_of_Book, Status)
2. Fine(Roll_no, Date, Amt)
Accept Roll_no & Name of Book from user.
Check the number of days (from date of issue), if days are between 15 to 30 then fine
amount will be Rs 5per day.
If no. of days>30, fine will be Rs 50 per day & for days less than 30, Rs. 5 per day. After
submitting the book, status will change from I to R.
If condition of fine is true, then details will be stored into fine table.
Also handles the exception by named exception handler or user define exception handler.
Ans :

Create table Borrower_272
(
Roll_no INT PRIMARY KEY,
S_name VARCHAR(10),
Issue_date DATE,
B_name VARCHAR(10),
Status VARCHAR(10)
);
Insert into borrower_272 values(221,'Arshiya',to_date('25-12-2023','dd-mm-yyyy'),'Fire','I');
Insert into borrower_272 values(280,'Pragati',to_date('12-01-2024','dd-mm-yyyy'),'Runner','R');
Insert into borrower_272 values(282,'Tanvi',to_date('01-02-2024','dd-mm-yyyy'),'Magic','I');
Insert into borrower_272 values(281,'Rutika',to_date('16-02-2024','dd-mm-yyyy'),'Today','I');
Insert into borrower_272 values(276,'Mahima',to_date('03-02-2024','dd-mm-yyyy'),'Alchemy','I');
Insert into borrower_272 values(268,'Sayali',to_date('12-02-2024','dd-mm-yyyy'),'Vibes','R');
Borrower_272 Table :

create table Fine_272
(
Roll_no INT,
Issue_date DATE,
Amount INT,
Foreign key (Roll_no) REFERENCES Borrower_272(Roll_no)
);
PL/SQL Block :
DECLARE
mroll borrower_272.Roll_no%type;
mbook borrower_272.B_name%type;
days number(3);
amt number(5);
date_iss borrower_272.Issue_date%type;
stat borrower_272.Status%type;
BEGIN
mroll:=&mroll;
mbook:='&mbook';
select Issue_date,Status,B_name into date_iss,stat,mbook from Borrower_272 where
roll_no=mroll AND B_name=mbook ;
If stat='R' then
dbms_output.put_line('Book has been returned');
else
days := trunc(sysdate) - date_iss;
dbms_output.put_line('Total days of book issued = '||days);
IF days>15 AND days<=30 then
amt:=(days-15) * 5 ;
insert into fine values(mroll, date_iss, amt);

dbms_output.put_line(mroll||' roll number has been added to Fine table');
elsif days>30 then
amt:= (15*5)+(days-30)*50 ;
insert into fine values(mroll, date_iss, amt);
dbms_output.put_line(mroll||' roll number has been added to Fine table');
else
dbms_output.put_line('No fine for this student');
End if;
End if;
EXCEPTION
when no_data_found then
dbms_output.put_line('The provided roll number or book name is not available. Provide
valid one!');
End;
/

as5
1. Write a PL/SQL stored Procedure for following requirements and call the procedure in
appropriate PL/SQL block. 1. Borrower(Rollin, Name, DateofIssue, NameofBook, Status) 2.
Fine(Roll_no,Date,Amt) Accept roll_no &name of book from user. Check the number of
days (from date of issue), if days are between 15 to 30 then fine amount will be Rs 5per
day. If no. of days>30, per day fine will be Rs 50 per day & for days less than 30, Rs. 5 per
day. After submitting the book, status will change from I to R. If condition of fine is true,
then details will be stored into fine table.

create table Borrower_272
(
Roll_no INT PRIMARY KEY,
Name VARCHAR(10),
Issue_Date DATE,
B_name VARCHAR(10),
Status VARCHAR(8)
);

Insert into borrower_272 values(221,'Arshiya',to_date('25-12-2023','dd-mm-yyyy'),'Fire','I');
Insert into borrower_272 values(280,'Pragati',to_date('12-01-2024','dd-mm-yyyy'),'Runner','R');
Insert into borrower_272 values(282,'Tanvi',to_date('01-02-2024','dd-mm-yyyy'),'Magic','I');
Insert into borrower_272 values(281,'Rutika',to_date('16-02-2024','dd-mm-yyyy'),'Today','I');
Insert into borrower_272 values(276,'Mahima',to_date('03-02-2024','dd-mm-yyyy'),'Alchemy','I');
Insert into borrower_272 values(268,'Sayali',to_date('12-02-2024','dd-mm-yyyy'),'Vibes','R');

PROCEDURE :
CREATE OR REPLACE PROCEDURE Cal_fine(date_iss IN DATE, Stat IN VARCHAR2, B_name
IN VARCHAR2, mroll IN NUMBER)
IS
amt number(10);
days number(5);
BEGIN
If stat='R' then
dbms_output.put_line('Book has been returned');
else
days := trunc(sysdate) - date_iss;
dbms_output.put_line('Total days of book issued = '||days);
IF days>15 AND days<=30 then
amt:= (days-15) * 5 ;
insert into fine_272 values(mroll, date_iss, amt);
dbms_output.put_line(mroll||' has been added to fine table');
elsif days>30 then
amt:= (15*5)+(days-30)*50 ;
insert into fine_272 values(mroll, date_iss, amt);
dbms_output.put_line(mroll||' has been added to fine table');
else
dbms_output.put_line('No fine for this student');
End if;
End if;
End;
/
--------------------------------------------------------
CALL TO PROCEDURE
---------------------------------------------------------
DECLARE
mroll borrower_272.Roll_no%type;
mbook borrower_272.B_name%type;
date_iss borrower_272.Issue_date%type;
stat borrower_272.Status%type;
BEGIN
mroll:=&mroll;
mbook:='&mbook';
select Issue_date,Status,B_name into date_iss,stat,mbook from Borrower_272 where
roll_no=mroll AND B_name=mbook ;
Cal_fine(date_iss,stat,mbook,mroll);
END;

/
OUTPUT :
Enter value for mroll: 221
Total days of book issued = 61
221 has been added to fine table
Enter value for mroll: 280
Book has been returned
Enter value for mroll: 282
Total days of book issued = 23
282 has been added to fine table
Enter value for mroll: 281
Total days of book issued = 8
No fine for this student
Enter value for mroll: 276
Total days of book issued = 21
276 has been added to fine table
Enter value for mroll: 268
Book has been returned

2. Write a stored function in PL/SQL for given requirement and use the same in PL/SQL
block. Account no. and branch name will be accepted from user. The same will be searched
in table acct_details. If status of the account is active then display appropriate message
and also store the account details in active_acc_details table, otherwise display message
on screen “account is inactive”.

CREATE TABLE Account_272
(

Acc_no INT PRIMARY KEY,
Name VARCHAR(10),
Branch VARCHAR(10),
Status VARCHAR(8)
);
Insert into Account_272 Values(12342,'Imtiyaz','Akurdi','A');
Insert into Account_272 Values(18722,'Pragati','Nigdi','I');
Insert into Account_272 Values(29028,'Tanvi','Degloor','I');
Insert into Account_272 Values(91821,'Rutika','Akurdi','A');
Insert into Account_272 Values(89282,'Mahima','Aurad','I');
Insert into Account_272 Values(29812,'Sayali','Akurdi','A');

CREATE FUNCTION :
CREATE OR REPLACE FUNCTION Check_status(Acc IN NUMBER)
RETURN NUMBER
IS
Bran VARCHAR(10);
Stat VARCHAR(8);
BEGIN
select Branch, Status into Bran,Stat from Account_272 where Acc_no=Acc;
If Stat='A' then
insert into Active_acc_272 values(Acc,Bran);
return 1;
elsif Stat='I' then
return 0;
End if;
EXCEPTION
When no_data_found then
dbms_output.put_line('Provided details are invalid. Try again!');
END;
/

CALLING FUNCTION :
DECLARE
macc Account_272.Acc_no%type;
result number(2);
BEGIN
macc:=&macc;
result:= Check_status(macc);
if result=1 then
dbms_output.put_line('This account is active and added to Active table');
else
dbms_output.put_line('This account is inactive');
End if;
END;
/
Enter value for macc: 12342
old 5: macc:=&macc;
new 5: macc:=12342;
This account is active and added to Active table
Enter value for macc: 18722
old 5: macc:=&macc;
new 5: macc:=18722;
This account is inactive
Enter value for macc: 29028
old 5: macc:=&macc;
new 5: macc:=29028;
This account is inactive
Enter value for macc: 91821
old 5: macc:=&macc;
new 5: macc:=91821;
This account is active and added to Active table
Enter value for macc: 89282
old 5: macc:=&macc;
new 5: macc:=89282;
This account is inactive
Enter value for macc: 29812
old 5: macc:=&macc;
new 5: macc:=29812;
This account is active and added to Active table

Enter value for macc: 17826
old 5: macc:=&macc;
new 5: macc:=17826;
Provided details are invalid. Try again!

3. Write a Stored Procedure namely proc_Grade for the categorization of student. If marks
scored by students in examination is <=1500 and marks>=990 then student will be placed
in distinction category if marks scored are between 989 and900 category is first class, if
marks 899 and 825 category is Higher Second Class Write a PL/SQL block for using
procedure created with above requirement.
Stud_Marks(name, total_marks)
Result(Roll,Name, Class)

Create table Stud_marks_272
(
Roll_no INT PRIMARY KEY,
Name VARCHAR(10),
Total_marks INT
);
insert into stud_marks_272 values (1,'Ruhi',920);
insert into stud_marks_272 values (2,'Muskan',860);
insert into stud_marks_272 values (3,'Diksha',1200);
insert into stud_marks_272 values (4,'Nikhat',975);
insert into stud_marks_272 values (5,'Vaishnavi',830);
insert into stud_marks_272 values (6,'Zain',943);

PROCEDURE :
CREATE OR REPLACE PROCEDURE Check_result (mroll IN NUMBER)
IS
mmarks Stud_marks_272.total_marks%type;
mname Stud_marks_272.name%type;
C VARCHAR(2);
BEGIN

SELECT total_marks,Name INTO mmarks,mname FROM Stud_marks_272 where
roll_no=mroll;
IF mmarks>=990 AND mmarks<=1500 THEN
C:='D';
ELSIF mmarks>=900 and mmarks<=989 THEN
C:='F';
ELSIF mmarks>=825 AND mmarks<=899 THEN
C:='S';
End if;
insert into result_272 values(mroll,mname,C);
dbms_output.put_line('Class has been updated');
EXCEPTION
WHEN no_data_found THEN
dbms_output.put_line('Invalid details. Try again!');
END;

CALLING PROCEDURE :

DECLARE
mroll stud_marks_272.roll_no%type;
BEGIN
mroll:=&mroll;
Check_result(mroll);
END;

as6
1. The bank manager has decided to activate all those accounts which were previously marked
as inactive for performing no transaction in the last 365 days. Write a PL/SQL block (using
implicit cursor) to update the status of the account, display an approximate message based on
the no. of rows affected by the update.
CREATE TABLE Bank_272
2 (
3 Acc_no INT PRIMARY KEY,
4 Name VARCHAR(10),
5 Status VARCHAR(8)
6 );
INSERT INTO Bank_272 values(27323,'Imtiyaz','A');
INSERT INTO Bank_272 values(82739,'Pragati','I');
INSERT INTO Bank_272 values(29873,'Tanvi','A');
INSERT INTO Bank_272 values(20392,'Rutika','I');
INSERT INTO Bank_272 values(92292,'Mahima','I');

DECLARE
Rows number(2);
BEGIN
update bank_272 set status='A' where status='I';
IF sql%found then
dbms_output.put_line('Status updated');
END IF;
Rows:=sql%rowcount;
dbms_output.put_line(Rows||' rows are affected in the table');
end;
/
OUTPUT :

Status updated
3 rows are affected in the table.

2. Organization has decided to increase the salary of employees by 10% of existing salary, who
are having salaries less than the average salary of the organization. Whenever such salary
updates take place, a record for the same is maintained in the increment_salary table.
EMP (E_no , Salary)
increment_salary(E_no , Salary)

SQL> Create table Emp_272
2 (
3 Emp_no INT PRIMARY KEY,
4 Salary INT
5 );
Table created.
SQL> Insert into Emp_272 values (1,23000);
1 row created.
SQL> Insert into Emp_272 values (2,10000);
1 row created.
SQL> Insert into Emp_272 values (3,82000);
1 row created.
SQL> Insert into Emp_272 values (4,30000);

1 row created.
SQL> Insert into Emp_272 values(5,9000);
1 row created.
SQL> Insert into Emp_272 values(6,25000);
1 row created.
SQL> Insert into Emp_272 values(7,8000);
1 row created.
select * from Emp_272;

SQL> create table inc_sal_272
2 (
3 Emp_no INT,
4 Sal INT,
5 FOREIGN KEY (Emp_no) REFERENCES Emp_272(Emp_no)
6 );

DECLARE
Cursor Crsr IS select emp_no, salary FROM Emp_272 WHERE salary<(SELECT avg(salary) FROM
Emp_272);
memp Emp_272.emp_no%type;
msalary Emp_272.salary%type;
msal Emp_272.salary%type;
BEGIN
open crsr;
if crsr%isopen then

loop
fetch crsr into memp,msalary;
exit when crsr%notfound;
msal:=msalary+(msalary*0.1);
update Emp_272 set salary=msal where emp_no=memp;
if crsr%found then
dbms_output.put_line('Salary updated for '||memp);
insert into inc_sal_272 values (memp,msal);
End if;
End loop;
End if
;
End;
/
OUTPUT :
Salary updated for 1
Salary updated for 2
Salary updated for 5
Salary updated for 6
Salary updated for 7

3. Write PL/SQL block using explicit cursor for following requirements: College has decided to
mark all those students detained (D) who are having attendance less than 75%. Whenever such
update takes place, a record for the same is maintained in the D_Stud table.
create table stud21(roll number(4), att number(4), status varchar(1));
create table d_stud(roll number(4), att number(4));
create table stud_272
(
Roll INT PRIMARY KEY,
Att INT,
Status VARCHAR(8)
);
create table d_272
(
Roll int,
Att int,
FOREIGN KEY (Roll) REFERENCES STUD_272(Roll)
);

Insert into stud_272 values(1,56,'-');
Insert into stud_272 values(2,89,'-');
Insert into stud_272 values(3,90,'-');
Insert into stud_272 values(4,67,'-');
Insert into stud_272 values(5,74,'-');
Insert into stud_272 values(6,98,'-');

select * from stud_272;

DECLARE
CURSOR crsr is select roll,att from stud_272 where att<75;
mroll stud_272.roll%type;
matt stud_272.att%type;
BEGIN
open crsr;
if crsr%isopen then
loop
fetch crsr into mroll,matt;
exit when crsr%notfound;
update stud_272 set status='D'where roll=mroll;
if crsr%found then
dbms_output.put_line('Roll '||mroll||' has been detained');
insert into d_272 values(mroll,matt);
End if;
End loop;
End if;
End;
/
Output :
Roll 1 has been detained
Roll 4 has been detained
Roll 5 has been detained
PL/SQL procedure successfully completed.

6. Write PL/SQL block using explicit cursor: Cursor FOR Loop for following requirements:
College has decided to mark all those students detained (D) who are having attendance less
than 75%. Whenever such update takes place, a record for the same is maintained in the
D_Stud table.
create table stud21(roll number(4), att number(4), status varchar(1));
create table d_stud(roll number(4), att number(4))
Delete from stud_272;
Delete from d_272;
create table stud_272
(
Roll INT PRIMARY KEY,
Att INT,
Status VARCHAR(8)
);
create table d_272
(
Roll int,
Att int,
FOREIGN KEY (Roll) REFERENCES STUD_272(Roll)
);

Insert into stud_272 values(1,56,'-');

Insert into stud_272 values(2,89,'-');
Insert into stud_272 values(3,90,'-');
Insert into stud_272 values(4,67,'-');
Insert into stud_272 values(5,74,'-');
Insert into stud_272 values(6,98,'-');

select * from stud_272;

DECLARE
CURSOR crsr is select roll,att from stud_272 where att<75;
BEGIN
FOR demo IN crsr
LOOP
update stud_272 set status='D' where roll=demo.roll;
dbms_output.put_line('Roll '||demo.roll||' has been detained');
insert into d_272 values (demo.roll, demo.att);
END LOOP;
END;
/

ASS 2] Create below tables with appropriate constraints like primary key, foreign key, check constrains, not null etc.
Account(Acc_no, branch_name,balance)
branch(branch_name,branch_city,assets)
customer(cust_name,cust_street,cust_city)
Depositor(cust_name,acc_no)
Loan(loan_no,branch_name,amount)
Borrower(cust_name,loan_no)
1) Find the names of all branches in loan relation.
SQL> select distinct branchname from loan_B100;
2) Find all loan numbers for loans made at Chikli Branch with loan amount > 20000.
SQL> select loan_no from loan_B100 where branchname='Chikli' and amount>20000;
3) Find all customers who have a loan from bank. Find their names,loan_no and loan amount.
SQL> select cust_name, borrow_B100.loan_no ,amount from borrow_B100 ,loan_B100 where borrow_B100.loan_no=loan_B100.loan_no;
4) List all customers in alphabetical order who have loan from Pimpri branch.
SQL> select cust_name from borrow_B100 where loan_no = (select loan_no from loan_B100 where branchname='Pimpri') order by cust_name;
5) Find all customers who have an account or loan or both at bank.
SQL> select cust_name from depositor_B100 UNION SELECT cust_name from borrow_B100;
6) Find all customers who have both account and loan at bank.
SQL> select cust_name from depositor_B100 INTERSECT SELECT cust_name from borrow_B100;
7) Find all customer who have account but no loan at the bank.
SQL>  select cust_name from depositor_B100 MINUS SELECT cust_name from borrow_B100;
8) Find average account balance at Chinchwad branch.
SQL> SELECT avg(balance) from account_B_100 where branchname='Chinchwad';
9) Find the average account balance at each branch
SQL> SELECT avg(balance),branchname from account_B_100 group by branchname;
10) Find no. of depositors at each branch.
SQL> SELECT count(acc_no),branchname from account_B_100 group by branchname;
11) Find the branches where average account balance > 40000.
SQL>  SELECT avg(balance),branchname from account_B_100 group by branchname having avg(balance)>40000;
12) Find number of tuples in customer relation.
SQL> select count(*) from customer_B100;
13) Calculate total loan amount given by bank.
SQL> select sum(amount) from loan_B100;
14) Delete all loans with loan amount between 20000 and 60000.
SQL> delete from loan_B100 where amount between 2000 and 60000;
15) Delete all tuples at every branch located in Khed.
SQL> DELETE FROM branch_B100 WHERE branchname='Khed';
16) Create synonym for customer table as cust.
SQL> create synonym cust_B100 for customer_B100;
17) Create sequence roll_seq and use in student table for roll_no column.
SQL> CREATE SEQUENCE id_seq INCREMENT BY 1 START WITH 1;
ASS 3]
Title - SQL queries using concepts like all types of Join, Sub-Query and View
1. Create following Tables
cust_mstr(cust_no,firstname,lname)
add_dets(code_no,add1,add2,state,city,pincode)
Retrieve the address of customer Firstname as 'xyz' and Lname as
'pqr'. 
SQL> SELECT *FROM add_dets118
2 INNER JOIN cust_mstr118 ON cust_mstr118.cust_no=add_dets118.cust_no
3 WHERE firstname='Omkar' AND lname='Shinde';
2.Create following Tables
cust_mstr(custno,firstname,lname)
acc_fd_cust_dets(codeno,acc_fd_no)
fd_dets(fd_sr_no,amt)
List the customer holding fixed deposit of amount more than
5000 
SQL> SELECT cust_mstr118.cust_no,firstname FROM cust_mstr118
2 WHERE cust_no IN (SELECT acc_fd_cust_dets118.cust_no
FROM acc_fd_cust_dets118
3 INNER JOIN fd_dets118 ON acc_fd_cust_dets118.acc_fd_no=fd_dets118.acc_fd_no
4 WHERE amt>5000);
3. Create following Tables
emp_mstr(e_mpno,f_name,l_name,m_name,dept,desg,branch_no
) branch_mstr(name,b_no)
List the employee details along with branch names to which they belong 
SQL> SELECT f_name,m_name,l_name,dept,desg FROM emp_mstr118
2 INNER JOIN branch_mstr118 ON
3 emp_mstr118.branch_no=branch_mstr118.branch_no;
SQL> SELECT f_name,m_name,l_name,dept,desg,branch_mstr118.name
FROM emp_mstr118
2 INNER JOIN branch_mstr118 ON
3 emp_mstr118.branch_no=branch_mstr118.branch_no;
4. Create following Tables
emp_mstr(emp_no,f_name,l_name,m_name,dept)
cntc_dets(code_no,cntc_type,cntc_data)
List the employee details along with contact details using left outer
join & right join
SQL> SELECT emp_no,f_name,m_name,l_name,dept,cntc_type,cntc_data
2 FROM cntc_dets118
3 LEFT OUTER JOIN emp_mstr118 ON cntc_dets118.cust_no=(SELECT cust_no
FROM
4 cust_mstr118 WHERE firstname=emp_mstr118.f_name
AND 5 lname=emp_mstr118.l_name);
SQL> SELECT emp_no,f_name,m_name,l_name,dept,cntc_type,cntc_data
2 FROM emp_mstr118
3 RIGHT OUTER JOIN cntc_dets118 ON
cntc_dets118.cust_no=(SELECT
cust_no FROM
4 cust_mstr118 WHERE firstname=emp_mstr118.f_name AND
5 lname=emp_mstr118.l_name);
5. Create following Tables
cust_mstr(cust_no,firstname,lname)
add_dets(code_no,pincode)
List the customer who do not have bank branches in their
vicinity. 
SQL> SELECT firstname,lname,pincode FROM add_dets118 JOIN cust_mstr118 ON
cust_mstr118.cust_no=add_dets118.cust_no
3 WHERE cust_mstr118.cust_no NOT IN ( SELECT cust_no FROM
acc_fd_cust_dets118
4 WHERE cust_mstr118.cust_no=acc_fd_cust_dets118.cust_no
);
6. a) Create View on borrower table by selecting any two columns and
perform insert update delete
operations
SQL> create view V1 as
2 select cust_name,loan_no
3 from Borrower118 ;
View created.
SQL> update V1 set cust_name='Sameer' where loan_no=6002; 1 row updated.
b) Create view on borrower and depositor table by selecting any one
column from each table
perform insert update delete operations
SQL> create view V2 as
2 select distinct
Borrower118.cust_name,loan_no 3 from
Borrower118 join
4 Depositor118 on
5
Borrower118.cust_name=Depositor118.cust_name;
View created.
SQL> update V2 set cust_name='Sameer' where
loan_no=6002; 1 row updated.
SQL> delete V2 where loan_no=6002;
c) create updateable view on borrower table by selecting any two columns
and perform insert
update delete operations.
SQL> create view V3 as
2 select cust_name,loan_no
3 from Borrower118 ;
View created.
ASS 4] 
1. Consider table Stud(Roll, Att,Status)
Write a PL/SQL block for following requirement and handle the exceptions.
Roll no. of student will be entered by user. Attendance of roll no. entered by user will be checked in Stud table. If attendance is less than 75% then display the message “Term not granted” and set the status in stud table as “D”. Otherwise display message “Term granted” and set the status in stud table as “ND”
Solution:
Declare
mroll number(10);
matt number(10);
Begin
mroll:= &mroll;
select att into matt from stud11 where roll = mroll;
if matt<75 then
dbms_output.put_line(mroll||'is detained');
update stud11 set status='D'where roll=mroll;
else
dbms_output.put_line(mroll||'is Not detained');
update stud11 set status='ND'where roll=mroll;
end if;
Exception
when no_data_found then
dbms_output.put_line(mroll||'Not found');
End;
SQL> select * from stud11;
ROLL ATT STATUS
---------- ---------- ----------
1 70 D
2 75
3 77 ND
4 71
5 79
Enter value for mroll: 6
old 5: mroll:= &mroll;new 5: mroll:= 6;
6Not found
PL/SQL procedure successfully completed.
2. Write a PL/SQL block for following requirement using user defined exception handling.
The account_master table records the current balance for an account, which is updated whenever, any deposits or withdrawals takes place. If the withdrawal attempted is more than the current balance held in the account. The user defined exception is raised, displaying an appropriate message. Write a PL/SQL block for above requirement using user defined exception handling.
DECLARE
 2      macc_no varchar(10);
 3      mamnt NUMBER;
 4      choice number;
 5      m_amount NUMBER;
 6      rem_bal NUMBER;
 7      ex_invalid_input EXCEPTION;
 8  BEGIN
 9      choice := &choice;
10      macc_no := '&macc_no';
11      mamnt := &mamnt;
12      IF choice = 1 THEN
13          SELECT amount INTO m_amount FROM Acc_B109 WHERE acc_no = macc_no;
14          IF mamnt > m_amount THEN
15              RAISE ex_invalid_input;
16          ELSE
17              rem_bal := m_amount - mamnt;
18              UPDATE Acc_B109 SET amount = rem_bal WHERE acc_no = macc_no;
19              DBMS_OUTPUT.PUT_LINE('Withdrawal successful.');
20          END IF;
21      ELSIF choice = 2 THEN
22          SELECT amount INTO m_amount FROM Acc_B109 WHERE acc_no = macc_no;
23          rem_bal := m_amount + mamnt;
24          UPDATE Acc_B109 SET amount = rem_bal WHERE acc_no = macc_no;
25          DBMS_OUTPUT.PUT_LINE('Deposit successful.');
27      END IF;
29  EXCEPTION 
30      WHEN ex_invalid_input THEN
31          DBMS_OUTPUT.PUT_LINE('Insufficient funds for withdrawal.');
32      WHEN OTHERS THEN
33         DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
34  END; 
35  / 
Enter value for choice: 1
old   9:     choice := &choice;
new   9:     choice := 1;
Enter value for macc_no: B123
old  10:     macc_no := '&macc_no';
new  10:     macc_no := 'B123';
Enter value for mamnt: 200
old  11:     mamnt := &mamnt;
new  11:     mamnt := 200;
Withdrawal successful.
PL/SQL procedure successfully completed.
SQL> /
Enter value for choice: 2
old   9:     choice := &choice;
new   9:     choice := 2;
Enter value for macc_no: C123
old  10:     macc_no := '&macc_no';
new  10:     macc_no := 'C123';
Enter value for mamnt: 200
old  11:     mamnt := &mamnt;
new  11:     mamnt := 200;
Deposit successful.
PL/SQL procedure successfully completed.
SQL> /
Enter value for choice: 1
old   9:     choice := &choice;
new   9:     choice := 1;
Enter value for macc_no: A123
old  10:     macc_no := '&macc_no';
new  10:     macc_no := 'A123';
Enter value for mamnt: 35000
old  11:     mamnt := &mamnt;
new  11:     mamnt := 35000;
Insufficient funds for withdrawal.
PL/SQL procedure successfully completed.
3. Write an SQL code block these raise a user defined exception where business rule is voilated. BR for client_master table specifies when the value of bal_due field is less than 0 handle the exception.
SQL> set serveroutput on;
SQL> declare
 2       c_id number;
 3       bal number;
 4       business_rule_violation exception;
 5  begin
 6      c_id:=&c_id;
 7      bal:=&bal;
 8       if bal<0
 9        then raise business_rule_violation;
10       else
11        insert into client_mastar_B109 (balance,id) values(bal,c_id);
12    end if;
13  Exception 
14              When business_rule_violation
15          then dbms_output.put_line(' Business Rule is Violated');
17  END; 
18  / 
Enter value for c_id: 1
old   6:     c_id:=&c_id;
new   6:     c_id:=1;
Enter value for bal: 1200
old   7:     bal:=&bal;
new   7:     bal:=1200;
PL/SQL procedure successfully completed.
SQL> /
Enter value for c_id: 2
old   6:     c_id:=&c_id;
new   6:     c_id:=2;
Enter value for bal: -100
old   7:     bal:=&bal;
new   7:     bal:=-100;
Business Rule is Violated
PL/SQL procedure successfully completed.
SQL> /
Enter value for c_id: 3
old   6:     c_id:=&c_id;
new   6:     c_id:=3;
Enter value for bal: 300
old   7:     bal:=&bal;
new   7:     bal:=300;
PL/SQL procedure successfully completed.
4.
1] Borrower(Roll_no, Name, DateofIssue, NameofBook, Status)
2. Fine(Roll_no,Date,Amt)
-Accept roll_no & name of book from user.
-Check the number of days (from date of issue), if days are between 15 to 30 then fine amount will be Rs 5per day.
-If no. of days>30, per day fine will be Rs 50 per day & for days less than 30, Rs. 5 per day.
-After submitting the book, status will change from I to R.
-If condition of fine is true, then details will be stored into fine table.
Also handles the exception by named exception handler or user define exception  handler.
SQL> declare
 2     no_of_days int;
 3     m_rollno varchar(10);
 4     book_name char(10);
 5     B_DOI date;
 6     AMT number;
 7     date1 date;
 8     new_days int;
 9  begin
10      m_rollno :=&m_rollno;
11      book_name:='&book_name';
12      select DOI into B_DOI from Borrower_B109 where Roll_no=m_rollno AND book_name=B_name;
13      no_of_days:=sysdate-B_DOI;
14     If no_of_days<15 then
15         AMT:=0;
16         date1:=sysdate;
17        insert into Fine_B109 values (m_rollno,date1,AMT) ;
18         UPDATE Borrower_B109 set status='R' where Roll_no=m_rollno;
19      ELSIF no_of_days>15 AND no_of_days<=30 then
20          new_days:=no_of_days-15;
21          AMT:=new_days*5;
22          date1:=sysdate;
23          insert into Fine_B109 values (m_rollno,date1,AMT) ;
24          UPDATE Borrower_B109 set status='R' where Roll_no=m_rollno;
25      ELSIF no_of_days>30 then
26          new_days:=no_of_days-30;
27          AMT:=new_days*50;
28          date1:=sysdate;
29          insert into Fine_B109 values (m_rollno,date1,AMT) ;
30          UPDATE Borrower_B109 set status='R' where Roll_no=m_rollno;
31      END IF;
32  end; 
33  / 
Enter value for m_rollno: 1
old  10:     m_rollno :=&m_rollno;
new  10:     m_rollno :=1;
Enter value for book_name: PL/SQL
old  11:     book_name:='&book_name';
new  11:     book_name:='PL/SQL';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 2
old  10:     m_rollno :=&m_rollno;
new  10:     m_rollno :=2;
Enter value for book_name: JAVA
old  11:     book_name:='&book_name';
new  11:     book_name:='JAVA';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 3
old  10:     m_rollno :=&m_rollno;
new  10:     m_rollno :=3;
Enter value for book_name: OP
old  11:     book_name:='&book_name';
new  11:     book_name:='OP';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 4
old  10:     m_rollno :=&m_rollno;
new  10:     m_rollno :=4;
Enter value for book_name: se
old  11:     book_name:='&book_name';
new  11:     book_name:='se';
PL/SQL procedure successfully completed.
ASS 5] 1. Write a PL/SQL stored Procedure for following requirements and call the procedure in appropriate PL/SQL block.
1. Borrower(Rollin, Name, DateofIssue, NameofBook, Status)
2. Fine(Roll_no,Date,Amt)
-Accept roll_no &name of book from user.
-Check the number of days (from date of issue), if days are between 15 to 30 then fine amount will be Rs 5per day.
-If no. of days>30, per day fine will be Rs 50 per day & for days less than 30, Rs. 5 per day.
-After submitting the book, status will change from I to R.
-If condition of fine is true, then details will be stored into fine table.
SQL> set serveroutput on;
SQL> CREATE OR REPLACE PROCEDURE library_B109(m_rollno IN VARCHAR2, book_name IN VARCHAR2) IS
  2     no_of_days INT;
  3     B_DOI DATE;
  4     AMT NUMBER;
  5     date1 DATE;
  6     new_days INT;
  7  BEGIN
  8      -- Retrieve the Date of Issue (DOI) for the given roll number and book name
  9      SELECT DOI INTO B_DOI FROM Borrower_B109 WHERE Roll_no = m_rollno AND book_name = book_name;
 10
 11      -- Calculate the number of days the book has been borrowed
 12      no_of_days := sysdate-B_DOI;
 13
 14      IF no_of_days < 15 THEN
 15          AMT := 0;
 16      ELSIF no_of_days >= 15 AND no_of_days <= 30 THEN
 17          new_days := no_of_days - 15;
 18          AMT := new_days * 5;
 19      ELSE
 20          new_days := no_of_days - 30;
 21          AMT := new_days * 50;
 22      END IF;
 23
 24      -- Record the fine details into the Fine_B109 table
 25      date1 := sysdate;
 26      INSERT INTO Fine_B109 VALUES (m_rollno, date1, AMT);
 27
 28      -- Update the status of the borrower
 29      UPDATE Borrower_B109 SET status = 'R' WHERE Roll_no = m_rollno;
 30  END;
 31  /
Procedure created.
SQL>
SQL> DECLARE
  2      m_rollno varchar(10);
  3     book_name char(10);
  4  BEGIN
  5       m_rollno :=&m_rollno;
  6      book_name:='&book_name';
  7
  8      library_B109(m_rollno, book_name);
  9  END;
 10  /
Enter value for m_rollno: 1
old   5:      m_rollno :=&m_rollno;
new   5:      m_rollno :=1;
Enter value for book_name: PL/SQL
old   6:     book_name:='&book_name';
new   6:     book_name:='PL/SQL';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 2
old   5:      m_rollno :=&m_rollno;
new   5:      m_rollno :=2;
Enter value for book_name: JAVA
old   6:     book_name:='&book_name';
new   6:     book_name:='JAVA';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 3
old   5:      m_rollno :=&m_rollno;
new   5:      m_rollno :=3;
Enter value for book_name: OP
old   6:     book_name:='&book_name';
new   6:     book_name:='OP';
PL/SQL procedure successfully completed.
SQL> /
Enter value for m_rollno: 4
old   5:      m_rollno :=&m_rollno;
new   5:      m_rollno :=4;
Enter value for book_name: se
old   6:     book_name:='&book_name';
new   6:     book_name:='se';
PL/SQL procedure successfully completed.
SQL> select * from Fine_B109;
ROLL_NO    DOI_RETUR       FINE_AMT
-----------     ---------           ----------
1                20-FEB-24         10
2               20-FEB-24         45
3               20-FEB-24        350
4               20-FEB-24         60
SQL> select * from Borrower_B109;
ROLL_NO    NAME       DOI       B_NAME     STATUS
---------- ----------      ---------       ----------      ----------
1          Anushka    04-FEB-24    PL/SQL           R
2          siya            28-JAN-24     JAVA               R
3          Ayush        15-JAN-24     OP                  R
4          Piya            25-JAN-24     se                   R
2. Write a stored function in PL/SQL for given requirement and use the same in PL/SQL block.
Account no. and branch name will be accepted from user. The same will be searched in table
acct_details. If status of account is active then display appropriate message and also store the account
details in active_acc_details table, otherwise display message on screen “account is inactive”.
SQL> create or replace function acc_status ( account_no number, b_name char, mstatus char)
  2  return number is
  3   a number;
  4
  5  begin
  6  if mstatus = 'Active' then
  7   return 1;
  8  else
  9   return 0;
 10  end if;
 11
 12  end;
 13  /
Function created.
SQL> declare
  2  account_no number ;
  3  branch_name char(10);
  4  mstatus char(10);
  5  ch number;
  6
  7  begin
  8  account_no := &account_no;
  9  select b_name , status into branch_name, mstatus from acc_B109 where account_no = acc_no;
 10  ch := acc_status( account_no, branch_name, mstatus);
 11  dbms_output.put_line ( 'ch = ' ||ch);
 12
 13  if ch = 1 then
 14  dbms_output.put_line ( 'account is active');
 15  insert into ActiveAcc_B109 values (account_no, branch_name,mstatus);
 16  else
 17  dbms_output.put_line('account is inactive');
 18  end if;
 19
 20  end;
 21
 22  /
Enter value for account_no: 1234
old   8: account_no := &account_no;
new   8: account_no := 1234;
ch = 1
account is active
PL/SQL procedure successfully completed.
SQL> /
Enter value for account_no: 109
old   8: account_no := &account_no;
new   8: account_no := 109;
ch = 0
account is inactive
PL/SQL procedure successfully completed.
SQL> select * from ActiveAcc_B109;
    ACC_NO B_NAME     STATUS
---------- ---------- ----------
   1234 Nigdi      Active
SQL> declare
  2  account_no number ;
  3  branch_name char(10);
  4  mstatus char(10);
  5  ch number;
  6
  7  begin
  8  account_no := &account_no;
  9  select b_name , status into branch_name, mstatus from acc_B109 where account_no = acc_no;
 10  ch := acc_status( account_no, branch_name, mstatus);
 11  dbms_output.put_line ( 'ch = ' ||ch);
 12
 13  if ch = 1 then
 14  dbms_output.put_line ( 'account is active');
 15  insert into ActiveAcc_B109 values (account_no, branch_name,mstatus);
 16  else
 17  dbms_output.put_line('account is inactive');
 18  end if;
 19
 20  end;
 21  /
Enter value for account_no: 1234
old   8: account_no := &account_no;
new   8: account_no := 1234;
ch = 1
account is active
PL/SQL procedure successfully completed.
SQL> /
Enter value for account_no: 109
old   8: account_no := &account_no;
new   8: account_no := 109;
ch = 0
account is inactive
PL/SQL procedure successfully completed.
SQL> /
    ACC_NO B_NAME     STATUS
---------- ---------- ----------
      1234 Nigdi      Active
SQL> /
Enter value for account_no: 6789
old   8: account_no := &account_no;
new   8: account_no := 6789;
ch = 0
account is inactive
PL/SQL procedure successfully completed.
SQL> /
Enter value for account_no: 1769
old   8: account_no := &account_no;
new   8: account_no := 1769;
ch = 1
account is active
PL/SQL procedure successfully completed.
3. Write a Stored Procedure namely proc_Grade for the categorization of student. If marks scored
by students in examination is <=1500 and marks>=990 then student will be placed in
distinction category if marks scored are between 989 and900 category is first class, if marks
899 and 825 category is Higher Second Class
Write a PL/SQL block for using procedure created with above requirement.
Stud_Marks(name, total_marks)
Result(Roll,Name, Class)
SQL> create or replace procedure Stud_proc_grade (roll in number, name in char, marks in number)
  2  is
  3  begin
  4  if marks <= 1500 and marks >= 990 then
  5  insert into result_B109 values ( roll, name , 'distinction');
  6  elsif marks <= 989 and marks >= 900 then
  7  insert into result_B109 values ( roll, name , 'firstclass');
  8  elsif marks  <= 899 and marks >= 825 then
  9  insert into result_B109 values ( roll, name, 'high 2nd class');
 10  else
 11  insert into result_B109 values ( roll , name , 'fail');
 12  end if ;
 13  end;
 14  /
Procedure created.
SQL> declare
  2  mname char(10);
  3  mroll number;
  4  marks number;
  5  begin
  6  mname:='&mname';
  7  mroll:=&mroll;
  8  select total_marks into marks from Stud_marks_B109 where name=mname;
  9  Stud_proc_grade(mroll,mname,marks);
 10  end;
 11  /
Enter value for mname: Janhvi
old   6: mname:='&mname';
new   6: mname:='Janhvi';
Enter value for mroll: 1
old   7: mroll:=&mroll;
new   7: mroll:=1;
PL/SQL procedure successfully completed.
SQL> /
Enter value for mname: siya
old   6: mname:='&mname';
new   6: mname:='siya';
Enter value for mroll: 2
old   7: mroll:=&mroll;
new   7: mroll:=2;
PL/SQL procedure successfully completed.
SQL> /
Enter value for mname: diya
old   6: mname:='&mname';
new   6: mname:='diya';
Enter value for mroll: 3
old   7: mroll:=&mroll;
new   7: mroll:=3;
PL/SQL procedure successfully completed.
SQL> /
Enter value for mname: Anurag
old   6: mname:='&mname';
new   6: mname:='Anurag';
Enter value for mroll: 4
old   7: mroll:=&mroll;
new   7: mroll:=4;
PL/SQL procedure successfully completed.
SQL> /
Enter value for mname: Anushka
old   6: mname:='&mname';
new   6: mname:='Anushka';
Enter value for mroll: 5
old   7: mroll:=&mroll;
new   7: mroll:=5;
PL/SQL procedure successfully completed.
ASS 6] 
Implicit Cursor
1. The bank manager has decided to activate all those accounts which were previously marked as
inactive for performing no transaction in last 365 days. Write a PL/SQ block (using implicit cursor) to
update the status of account, display an approximate message based on the no. of rows affected by the
update.
(Use of %FOUND, %NOTFOUND, %ROWCOUNT)
SQL> declare
  2  flag number;
  3
  4  account number;
  5  begin
  6  account:=&account;
  7  update ac_B109 set status='A' where account=acc_no and status='NA';
  8  IF sql%notfound THEN
  9        dbms_output.put_line('no records selected');
 10     ELSIF sql%found THEN
 11         flag:= (sql%rowcount) ;
 12  dbms_output.put_line('The no of records updates ' || flag);
 13  end if;
 14  end;
 15  /
Enter value for account: 1234
old   6: account:=&account;
new   6: account:=1234;
The no of records updates 1
PL/SQL procedure successfully completed.
SQL> /
Enter value for account: 678
old   6: account:=&account;
new   6: account:=678;
no records selected
PL/SQL procedure successfully completed.
SQL> /
Enter value for account: 4567
old   6: account:=&account;
new   6: account:=4567;
The no of records updates 1
PL/SQL procedure successfully completed.
SQL> /
Enter value for account: 3498
old   6: account:=&account;
new   6: account:=3498;
no records selected
PL/SQL procedure successfully completed.
EXPLICIT CURSOR:
2. Organization has decided to increase the salary of employees by 10% of existing salary, who are
having salary less than average salary of organization, Whenever such salary updates takes place, a
record for the same is maintained in the increment_salary table.
EMP (E_no , Salary)
increment_salary(E_no , Salary)
SQL> declare
  2       empid EMP_B109.E_no%type;
  3      empsal EMP_B109.salary%type;
  4       CURSOR increase_sal is select E_no,salary from EMP_B109 where salary<(select avg(salary) from EMP_B109);
  5       Begin
  6       open increase_sal;
  7       WHILE increase_sal%isopen
  8      LOOP
  9     fetch increase_sal into empid,empsal;
 10     exit when increase_sal%notfound;
 11      if increase_sal%found then
 12      update EMP_B109 set salary=(salary+(salary*0.1)) where E_no=empid;
 13        select salary into empsal FROM EMP_B109 where E_no=empid;
 14    insert into  inc_salary_B109 values(empid,empsal);
 15      end if;
 16     END LOOP;
 17      close increase_sal;
 18      END;
 19    /
PL/SQL procedure successfully completed.
]
3. Write PL/SQL block using explicit cursor for following requirements:
College has decided to mark all those students detained (D) who are having attendance less than 75%.
Whenever such update takes place, a record for the same is maintained in the D_Stud table.
create table stud21(roll number(4), att number(4), status varchar(1));
create table d_stud(roll number(4), att number(4));
SQL> create table stud_B109(roll number, Attendance number, status char(10));
Table created.
SQL> create table D_stud_B109(roll number, Attendance number);
Table created.
SQL> insert into stud_B109 values(1,89,null);
1 row created.
SQL> insert into stud_B109 values(2,67,null);
1 row created.
SQL> insert into stud_B109 values(3,71,null);
1 row created.
SQL> insert into stud_B109 values(4,75,null);
1 row created.
SQL> insert into stud_B109 values(5,95,null);
1 row created.
SQL> select * from stud_B109;
   ROLL  ATTENDANCE STATUS
----------   -----------------      ----------
         1         89
         2         67 
         3         71 
         4         75
         5         95
SQL> declare
  2  Roll_no stud_B109.roll%type;
  3  att  stud_B109.Attendance%type;
  4  cursor Detained is select roll,Attendance from stud_B109 where Attendance<75;
  5  Begin
  6  open Detained;
  7  while Detained%isopen
  8  Loop
  9  fetch Detained into Roll_no,att;
 10  exit when Detained%notfound;
 11  if Detained %found then
 12  update stud_B109 set status='D' where att<75 and Roll_no=roll;
 13  insert into D_stud_B109 values(Roll_no,att);
 14  end if;
 15  END LOOP;
 16  close Detained;
 17  End;
 18  /
PL/SQL procedure successfully completed.
-- Check status update in stud_B109 table
SELECT * FROM stud_B109;

-- Check detained students in D_stud_B109 table
SELECT * FROM D_stud_B109;

ASS 7] 
1. Write a update, delete trigger on clientmstr table. The System should keep track of the
records that ARE BEING updated or deleted. The old value of updated or deleted
records should be added in audit_trade table. (separate implementation using both row
and statement triggers)
Q1:
SQL> create table client_B109(c_id number,c_name char(10));
Table created.
SQL> create table audit_trade_B109(old_id number,old_name char(10),action char(10));
Table created.
SQL> insert into client_B109 values(1,'Janhvi');
1 row created.
SQL> insert into client_B109 values(2,'siya');
1 row created.
SQL> insert into client_B109 values(3,'Anurag');
1 row created.
SQL> insert into client_B109 values(4,'roshani');
1 row created.
SQL> select * from client_B109;
      C_ID C_NAME
---------- ----------
         1      Janhvi
         2     siya
         3    Anurag
         4    roshani
SQL> select * from audit_trade_B109;
no rows selected
SQL> create or replace trigger track before update or delete on client_B109
  2  for each row
  3  declare
  4      op char(10);
  5  begin
  6      if updating then
  7       op:='updated';
  8      end if;
  9      if deleting then
 10      op:='deleting';
 11      end if;
 12      insert into audit_trade_B109 values(:old.c_id,:old.c_name,op);
 13  end;
 14  /
Trigger created.
SQL> update client_B109 set c_name='Harshad' where c_id=4;
1 row updated.
SQL> select * from audit_trade_B109;
   OLD_ID  OLD_NAME   ACTION
---------- ---------- ----------
         4      roshani    updated
2. Write a before trigger for Insert, update event considering following requirement:
Emp(e_no, e_name, salary)
I) Trigger action should be initiated when salary is tried to be inserted is less than Rs.
50,000/-
II) Trigger action should be initiated when salary is tried to be updated for value less
than Rs. 50,000/-
Action should be rejection of update or Insert operation by displaying appropriate error
message. Also the new values expected to be inserted will be stored in new table
Tracking(e_no, salary).
SQL> create table Emp_B109(e_no number,emp_name char(10),salary number);
Table created.
SQL> create table tracking_B109(emp_no number,e_sal number);
Table created.
SQL> create or replace trigger Employee before insert or update on Emp_B109
  2  for each row
  3  begin
  4        if :new.salary<50000 then
  5            raise_application_error(-20003,'Salary must be at least Rs. 50000');
  6        else
  7         insert into tracking_B109 values(:new.e_no,:new.salary);
  8        end if;
  9  end;
 10  /
Trigger created.
SQL>  insert into Emp_B109 values(1,'Janhvi',70000);
1 row created.
SQL> insert into Emp_B109 values(2,'siya',40000);
insert into Emp_B109 values(2,'siya',40000)
            *
ERROR at line 1:
ORA-20003: Salary must be at least Rs. 50000
ORA-06512: at "SYSTEM.EMPLOYEE", line 3
ORA-04088: error during execution of trigger 'SYSTEM.EMPLOYEE'
SQL> insert into Emp_B109 values(3,'Anurag',60000);
1 row created.
SQL> insert into Emp_B109 values(4,'Anushka',2000);
insert into Emp_B109 values(4,'Anushka',2000)
            *
ERROR at line 1:
ORA-20003: Salary must be at least Rs. 50000
ORA-06512: at "SYSTEM.EMPLOYEE", line 3
ORA-04088: error during execution of trigger 'SYSTEM.EMPLOYEE'
SQL> select * from Emp_B109;
      E_NO EMP_NAME       SALARY
---------- ---------- ----------
         1 Janhvi          70000
         3 Anurag          60000
SQL> select * from tracking_B109;
    EMP_NO      E_SAL
---------- ----------
         1      70000
         3      60000
3. Write a Database trigger for following requirements:
Employee salary of last three month is stored in the emp_sal table.
emp_sal(emp_no, sal1,sal2,sal3)
before inserting salary into emp_sal table, if salary of employee in any of the last three month
is greater than Rs. 50,000/- then entry of average salary along with emp_no needs to be
inserted into new table emp_new(emp_no, avg_sal).
SQL> create table Emp_sal_B109 (e_no number,sal1 number,sal2 number,sal3 number);
Table created.
SQL> create table Emp_new_B109 (e_no number,avg number);
Table created.
SQL> CREATE OR REPLACE TRIGGER salary BEFORE INSERT ON
  2  Emp_sal_B109
  3  FOR EACH ROW
  4  DECLARE
  5   Avg NUMBER;
  6   BEGIN
  7   IF :NEW.sal1>50000 OR :NEW.sal2> 50000 OR :NEW.sal3>50000 THEN
  8   Avg := (:NEW.sal1 + :NEW.sal2 + :NEW.sal3) / 3;
  9   INSERT INTO Emp_new_B109 VALUES (:NEW.e_no, Avg);
 10   END IF;
 11   END;
 12  /
Trigger created.
SQL>  insert into Emp_sal_B109 values(1,48000,52000,46000);
1 row created.
SQL>  insert into Emp_sal_B109 values(2,36000,40000,46000);
1 row created.
SQL>  insert into Emp_sal_B109 values(3,42000,40000,46000);
1 row created.
SQL> insert into Emp_sal_B109 values(4,52000,58000,46000);
1 row created.
SQL> select * from Emp_sal_B109;
      E_NO       SAL1       SAL2       SAL3
---------- ---------- ---------- ----------
         1      48000      52000      46000
         2      36000      40000      46000
         3      42000      40000      46000
         4      52000      58000      46000
SQL> select * from Emp_new_B109;
      E_NO        AVG
---------- ----------
         1 48666.6667
         4      52000
1] Consider following Bank database schema and solve given queries:
Account(Acc_no, branch_name,balance)
branch(branch_name,branch_city, assets)
customer(cust_name,cust_street,cust_city)
Depositor(cust_name,acc_no)
Loan(loan_no,branch_name,amount)
Borrower(cust_name,loan_no)
Q.1 Create above tables with appropriate constraints like primary key, foreign key, not null
etc. with suitable data
SQL> create table branch(branch_name char(20) primary key,branch_city char(20) , assets
number);
Table created.
SQL> insert into branch values('Nigdi','Pune',10000000)
SQL> insert into branch values('Akurdi','Pune',50000000);
SQL> insert into branch values('Khed','Pune',70000000);
SQL> insert into branch values('Chinchwad','Pune',80000000);
SQL> insert into branch values('Wafgaon','Khed',30000000);
SQL> select * from branch;
BRANCH_NAME BRANCH_CITY ASSETS
-------------------- -------------------- ----------
Nigdi Pune 10000000
Akurdi Pune 50000000Khed Pune 70000000
Chinchwad Pune 80000000
Wafgaon Khed 30000000
SQL> create table account(acc_no number primary key , branch_name char(20) references
branch(branch_name) on delete set null ,balance number);
Table created.
SQL> insert into account values(1234567891,'Nigdi',10000);
SQL> insert into account values(1234567892,'Nigdi',20000);
SQL> insert into account values(1234567893,'Khed',30000);
SQL> insert into account values(1234567894,'Chinchwad',40000);
SQL> select * from account;
ACC_NO BRANCH_NAME BALANCE
---------- -------------------- ----------
1234567891 Nigdi 10000
1234567892 Nigdi 20000
1234567893 Khed 30000
1234567894 Chinchwad 40000 SQL> create table customer(cust_name char(20)
primary key,cust_street varchar(30),cust_city char(20) );
Table created.
SQL> insert into customer values(‘Harshad’,'khed','Pune');
SQL> insert into customer values('Aviraj','Nigdi11','Pune');
SQL> insert into customer values('Nayan','Nigdi12','Pune');
SQL> insert into customer values('Ashirwad','Nigdi13','Pandharpur');
SQL> insert into customer values('Aditesh','audh','Pune');SQL> insert into customer values('Vedant','Khed','Pune');
SQL> insert into customer values('Aditya','Khed','Pune');
SQL> insert into customer values('Siddhesh','Kolhapur','Kolhapur');
SQL> insert into customer values('Rushi','Kolhapur','Kolhapur');
SQL> insert into customer values('Sarang','Satara','Satara');
SQL> select * from customer;
CUST_NAME CUST_STREET CUST_CITY
-------------------- ------------------------------ --------------------
Harshad Khed Pune
Aviraj Nigdi11 Pune
Nayan Nigdi12 Pune
Ashirwad Nigdi13 Pandharpur
Aditesh audh Pune
Vedant Khed Pune
Aditya Khed Pune
Siddhesh Kolhapur Kolhapur
Rushi Kolhapur Kolhapur
Sarang Satara Satara
10 rows selected.
SQL> create table depositor(cust_name char(20) references customer(cust_name) on delete
cascade , acc_no number references account(acc_no) on delete set null);
Table created.
SQL> select * from depositor;
CUST_NAME ACC_NO
-------------------- ----------Harshad 1234567891
Aviraj 1234567892
Nayan 1234567893
Ashirwad 1234567894
SQL> create table loan(loan_no number primary key,branch_name char(20) references
branch(branch_name) on delete set null , amount number);
Table created.
SQL> select * from loan;
LOAN_NO BRANCH_NAME AMOUNT
---------- -------------------- ----------
9874563211 Nigdi 200000
9874563212 Nigdi 300000
9874563213 Nigdi 400000
9874563214 Khed 350000
9874563215 Chinchwad 250000
SQL> create table borrower(customer_name char(20) references customer(cust_name)on
delete cascade,loan_no number references loan(loan_no) on delete set null);
Table created.
SQL> select * from borrower;
CUSTOMER_NAME LOAN_NO
-------------------- ----------
Aditesh 9874563211
Vedant 9874563212Aditya 9874563213
Rushi 9874563214
Q.2. Create synonym for customer table as cust.
SQL> create synonym cust for customer;
Synonym created.
Q.3 Add customer phone number in Customer table.
SQL> alter table customer add phone_no number(10);
Table altered.
Q.4 Delete phone number attribute from Customer table.
SQL> alter table customer drop column phone_no;
Table altered.
SQL> desc customer;
Name Null? Type
----------------------------------------- -------- ----------------------------
CUST_NAME NOT NULL CHAR(20)
CUST_STREET VARCHAR2(30)
CUST_CITY CHAR(20)
Q.5. Find the names of all branches in loan relation.
SQL> select distinct branch_name from loan;
BRANCH_NAME
--------------------
Nigdi
Khed
Chinchwad
Q.6. Find all customers who have a loan from bank. Find their names,loan_no and loan amount.
SQL> select borrower.customer_name , borrower.loan_no , loan.amount from borrower,loan
where borrower.loan_no = loan.loan_no;
CUSTOMER_NAME LOAN_NO AMOUNT
-------------------- ---------- ----------
Aditesh 9874563211 200000
Vedant 9874563212 300000
Aditya 9874563213 400000
Rushi 9874563214 350000
Q.7. List all customers in alphabetical order who have loan from Nigdi branch.
SQL> select customer_name from borrower where loan_no in (select loan_no from loan
where branch_name = 'Nigdi') order by customer_name;
CUSTOMER_NAME
--------------------
Aditesh
Aditya
Vedant
Q.8. Find all customers who have an account or loan or both at bank.
SQL> SELECT cust_name FROM customer where cust_name IN(SELECT customer_name
FROM borrower UNION SELECT cust_name FROM depositor);
CUST_NAME
--------------------
Rushi
Q.9. Find average account balance at Nigdi branch.
SQL> SELECT AVG(balance) AS avg_balance FROM account WHERE branch_name = 'Nigdi';
AVG_BALANCE
-----------
15000
Q.10. Find no. of depositors at each branch.
SELECT branch_name, COUNT(*) AS num_depositors FROM depositor GROUP BY
branch_name;
Q.11. Delete all tuples at every branch located in Nigdi.
DELETE FROM branch WHERE branch_city = 'Nigdi';
DELETE FROM account WHERE branch_name = 'Nigdi';
2]
a) Consider following database schema and solve given queries
cust_mstr(cust_no,fname,lname)
add_dets(code_no,add1,add2,state,city,pincode)
1. Create above Tables with suitable data
2. Retrieve the address of customer Fname as 'xyz' and Lname as 'pqr'
3. Create View on add_dets table by selecting any two columns and perform insert update delete operations
SQL> CREATE TABLE cust_mstr (cust_no INT PRIMARY KEY, fname VARCHAR(50),4 lname VARCHAR(50) );
Table created.
SQL> select * from cust_mstr;
CUST_NO FNAME LNAME
---------- --------------------- -----------------------------
1 Harshad Karale
2 Rushikesh Magadum
3 Sarang Kadam
SQL> CREATE TABLE add_dets (code_no INT references cust_mstr(cust_no),add1
VARCHAR(100),add2 VARCHAR(100),state VARCHAR(50),city VARCHAR(50),pincode
VARCHAR(10));
Table created.
SQL> INSERT INTO add_dets values(1,'Jaulke BK','Pargaon','Maharashtra','Khed',410512);
1 row created.
SQL> INSERT INTO add_dets values(2,'xyz','Kolhapur','Maharashtra','Kolhapur',410512);
1 row created
SQL> select * from add_dets where code_no IN ( select cust_no from cust_mstr where
fname='Harshad' AND lname='Karale');1
Jaulke BK
Pargaon
Maharashtra
Khed
410512
b) Create following Tables
emp_mstr(e_mpno,f_name,l_name,m_name,dept,desg,branch_no)
branch_mstr(name,b_no)
List the employee details along with branch names to which they belong
SQL> CREATE TABLE branch_mstr(name char(10),b_no number primary key);
Table created.
SQL> select * from branch_mstr;
NAME B_NO
---------- ----------
Computer 1
MECH 2
IT 3
Civil 4
SQL> CREATE TABLE emp_mstr(emp_no number NOT NULL,fname char(10),lname
char(10),mname char(10),dept char(10),desg char(10),branch_no number references
branch_mstr(b_no) on delete set null);
Table created.
SQL> select * from emp_mstr;
EMP_NO FNAME LNAME MNAME DEPT DESG BRANCH_NO
---------- ---------- ---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp Student 1SQL> select * from emp_mstr where branch_no IN(select b_no from branch_mstr where
name = 'Computer');
EMP_NO FNAME LNAME MNAME DEPT DESG BRANCH_NO
---------- ---------- ---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp Student 1
3] Consider following Bank database schema and solve given queries:
Account(Acc_no, branch_name,balance)
branch(branch_name,branch_city, assets)
customer(cust_name,cust_street,cust_city)
Depositor(cust_name,acc_no)
Loan(loan_no,branch_name,amount)
Borrower(cust_name,loan_no)
Q.1 Create above tables with appropriate constraints like primary key, foreign key
constrains, not
null etc. with suitable data
Q.2. Modify “assets” attribute of branch table to “Property”
SQL> ALTER TABLE branch RENAME COLUMN assets TO property;
Table altered.
Q.3. Find all loan numbers for loans made at Akurdi Branch with loan amount > 12000.
SQL> SELECT loan_no FROM loan WHERE branch_name = 'Nigdi' AND amount > 12000;
LOAN_NO
----------
9874563211
9874563212
9874563213
Q.4. Find all customers who have both account and loan at bank.
SQL> SELECT cust_name FROM customer where cust_name IN(SELECT customer_name
FROM borrower INTERSECT SELECT cust_name FROM depositor);
CUST_NAME
--------------------
Rushi
Q.5. Find all customer who have account but no loan at the bank.
SQL> SELECT cust_name FROM customer where cust_name IN(SELECT customer_name
FROM borrower MINUS SELECT cust_name FROM depositor);
CUST_NAME
--------------------------------------------------
Aviraj
Rushi
Q.6. Find the average account balance at each branch
SQL> SELECT branch_name,AVG(balance) AS avg_balance FROM account Group by
branch_name;
BRANCH_NAME AVG_BALANCE
-------------------- -----------
Nigdi 15000
Khed 30000
Chinchwad 40000
Q.7. Find the branches where average account balance > 12000.
SQL> SELECT AVG(balance),branch_name FROM account GROUP BY branch_name HAVING
AVG(balance)>20000;
AVG(BALANCE) BRANCH_NAME------------ --------------------
30000 Khed
40000 Chinchwad
Q.8. Find number of tuples in customer relation.
SQL> SELECT COUNT(*) AS tuple_no FROM customer;
TUPLE_NO
----------
12
Q.9. Calculate total loan amount given by bank.
SQL> SELECT SUM(amount) AS total_loan FROM loan;
TOTAL_LOAN
----------
2143800
Q.10. Delete all loans with loan amount between 1300 and 1500.
SQL> DELETE FROM loan WHERE amount BETWEEN 25000 AND 30000;
1 row deleted.
Q.11. Create sequence roll_seq and use in student table for roll_no column.
SQL> CREATE SEQUENCE roll_no START with 1 INCREMENT BY 1 MINVALUE 1 MAXVALUE 100
CYCLE;
Sequence created.
SQL> CREATE TABLE student(rollno number , name varchar(10));
Table created.
SQL> select * from student;ROLLNO NAME
---------- ----------
1 Harshad
2 Nayan
3 Aviraj
4]
a) Create following Tables with suitable data and solve following query
cust_mstr(custno,fname,lname)
acc_fd_cust_dets(codeno,acc_fd_no)
fd_dets(fd_sr_no,amt)
List the customer holding fixed deposit of amount more than 5000
SQL> CREATE TABLE fd_dets(fd_sr_no number primary key ,amt number);
Table created.
SQL> select * from cust_mstr;
CUST_NO FNAME LNAME
---------- ---------- ----------
1 Harshad Karale
2 Aviraj Kale
3 Nayan Keote
SQL> select * from fd_dets;
FD_SR_NO AMT
---------- ----------
1 2000
2 6000
3 8000
4 10000SQL> CREATE TABLE acc_fd_cust_dets(codeno number references cust_mstr(cust_no) on
delete cascade,acc_fd_no number references fd_dets(fd_sr_no) on delete cascade);
Table created.
SQL> select * from acc_fd_cust_dets;
CODENO ACC_FD_NO
---------- ----------
1 1
2 2
3 3
SQL> select * from cust_mstr where cust_no IN(SELECT codeno from acc_fd_cust_dets
where acc_fd_no IN(SELECT fd_sr_no FROM fd_dets where amt>5000));
CUST_NO FNAME LNAME
---------- ---------- ----------
2 Aviraj Kale
3 Nayan Keote
b) Create view on cust_mstr and acc_fd_cust_dets tables by selecting any one column
from each table perform insert update delete operations
1. Create a view selecting any one column from each table:
CREATE VIEW cust_acc_view AS SELECT c.cust_name, a.acc_no FROM cust_mstr c JOIN
acc_fd_cust_dets a ON c.cust_no = a.cust_no;
2. Perform insert operation on the view:
INSERT INTO cust_acc_view (cust_name, acc_no) VALUES ('John Doe', 123456);
3. Perform update operation on the view:
UPDATE cust_acc_view SET acc_no = 654321 WHERE cust_name = 'John Doe';
4. Perform delete operation on the view:
DELETE FROM cust_acc_view WHERE cust_name = 'John Doe';
c) Create following Tables with suitable data and solve following query
emp_mstr(emp_no,f_name,l_name,m_name,dept)cntc_dets(code_no,cntc_type,cntc_data)
List the employee details along with contact details using left outer join & right join
SQL> CREATE TABLE branch_mstr(name char(10),b_no number primary key);
Table created.
SQL> select * from branch_mstr;
NAME B_NO
---------- ----------
Computer 1
MECH 2
IT 3
Civil 4
SQL> CREATE TABLE emp_mstr(emp_no number NOT NULL,fname char(10),lname
char(10),mname char(10),dept char(10),desg char(10),branch_no number references
branch_mstr(b_no) on delete set null);
Table created.
SQL> select * from emp_mstr;
EMP_NO FNAME LNAME MNAME DEPT DESG BRANCH_NO
---------- ---------- ---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp Student 1
SQL> select * from emp_mstr where branch_no IN(select b_no from branch_mstr where
name = 'Computer');
EMP_NO FNAME LNAME MNAME DEPT DESG BRANCH_NO
---------- ---------- ---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp Student 1
4] Create following Tables emp_mstr(emp_no,f_name,l_name,m_name,dept)
cntc_dets(code_no,cntc_type,cntc_data) List the employee details along with contact details using left outer join & right join
SQL> CREATE TABLE emp_mstr(emp_no number primary key,fname char(10),lname
char(10),mname char(10),dept char(10));
Table created.
SQL> select * from emp_mstr;
EMP_NO FNAME LNAME MNAME DEPT
---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp
2 Aviraj Kale Popat IT
3 Nayan Keote Gajanan AIML
4 Ashirwad Katakamwar Rajeshwar Entc
5 Rushi Magadum Ranjit Comp
SQL> CREATE TABLE cntc_dets(codeno number references
emp_mstr(emp_no),cntc_type varchar(20),cntc_data varchar(20));
Table created.
SQL> select * from cntc_dets;
CODENO CNTC_TYPE CNTC_DATA
---------- -------------------- --------------------
1 Email harshad@gmail.com
2 Mob 9322918702
3 Email nayan@gmail.com
3 mob 1234567890
LEFT OUTER JOIN:
SQL> select * from emp_mstr LEFT OUTER JOIN cntc_dets ON
emp_mstr.emp_no = cntc_dets.codeno;
EMP_NO FNAME LNAME MNAME DEPT CODENO
---------- ---------- ---------- ---------- ---------- ----------
1 Harshad Karale Sanjay Comp 12 Aviraj Kale Popat IT 2
3 Nayan Keote Gajanan AIML 3
4 Ashirwad Katakamwar Rajeshwar Entc
5 Rushi Magadum Ranjit Comp
CNTC_TYPE CNTC_DATA
-------------------- --------------------
Email harshad@gmail.com
Mob 9322918702
Email nayan@gmail.com
RIGHT OUTER JOIN
SQL> select * from emp_mstr RIGHT OUTER JOIN cntc_dets ON
emp_mstr.emp_no = cntc_dets.codeno;
EMP_NO FNAME LNAME MNAME DEPT CODENO
---------- ---------- ---------- ---------- ---------- ----------
CNTC_TYPE CNTC_DATA
-------------------- --------------------
1 Harshad Karale Sanjay Comp 1
Email harshad@gmail.com
2 Aviraj Kale Popat IT 2
Mob 9322918702
3 Nayan Keote Gajanan AIML 3
Email nayan@gmail.com
3 Nayan Keote Gajanan AIML 3
mob 1234567890
5. Create following Tables cust_mstr(cust_no,fname,lname) add_dets(code_no,pincode) List
the customer who do not have bank branches in their vicinity.
SQL> CREATE TABLE cust_mstr(cust_no varchar(10) primary key ,fname char(10),lname
char(10));Table created.
SQL> select * from cust_mstr;
CUST_NO FNAME LNAME
---------- ---------- ----------
C1 Harshad Karale
C2 Aviraj Kale
C3 Nayan Keote
SQL> select * from add_dets;
SQL> CREATE TABLE add_dets(codeno varchar(10),pincode number);
Table created.
CODENO PINCODE
---------- ----------
B1 410510
C1 410510
C2 410511
C3 410512
SQL> select * from cust_mstr where cust_no IN(select codeno from add_dets where
codeno like 'C%' AND pincode NOT IN(select pincode from add_dets where codeno like
'B%'));
CUST_NO FNAME LNAME
---------- ---------- ----------
C2 Aviraj Kale
C3 Nayan Keote
5]
a) Consider following database schema and solve given queries
cust_mstr(cust_no,fname,lname)
add_dets(code_no,add1,add2,state,city,pincode)
1. Create above Tables with suitable data
2. Retrieve the address of customer Fname as 'xyz' and Lname as 'pqr'
3. Create View on add_dets table by selecting any two columns and perform insert update delete operations
-- Create View
CREATE VIEW add_dets_view AS
SELECT add1, add2
FROM add_dets;
-- Perform insert operation on the view
INSERT INTO add_dets_view (add1, add2) VALUES ('Test Address 1', 'Test Address 2');
-- Perform update operation on the view
UPDATE add_dets_view SET add1 = 'Updated Address 1' WHERE add1 = 'Test Address 1';
-- Perform delete operation on the view
DELETE FROM add_dets_view WHERE add1 = 'Updated Address 1';
b)Create following Tables
cust_mstr(cust_no,fname,lname)
add_dets(code_no,pincode)
(Most of the queries are similar as follow)
List the customer who do not have bank branches in their vicinity.
SELECT cust_no, fname, lname
FROM cust_mstr
LEFT JOIN add_dets ON cust_mstr.cust_no = add_dets.code_no
LEFT JOIN bank_branches ON add_dets.pincode = bank_branches.pincode
WHERE bank_branches.pincode IS NULL;
6]
Q 1.Consider table Stud(Roll, Att,Status)
Write a PL/SQL block for following requirement and handle the exceptions.
Roll no. of student will be entered by user. Attendance of roll no. entered by user will be
checked in Stud table. If attendance is less than 75% then display the message “Term not granted” and set the
status in stud table as “D”. Otherwise display message “Term granted” and set the status in stud table as “ND”.
SQL> CREATE TABLE stud(roll_no NUMBER PRIMARY KEY , att NUMBER ,status char(5));
Table created.
SQL> select * from stud;
ROLL_NO ATT STATU---------- ---------- -----
123 95 NULL
129 99 NULL
133 94 NULL
137 97 NULL
300 60 NULL
301 70 NULL
Declare
mroll number(10);
matt number(10);
Begin
mroll:=&mroll;
select att into matt from stud where roll_no = mroll;
if matt<75 then
dbms_output.put_line(mroll || 'is detained');
update stud set status='D' where roll_no = mroll;
else
dbms_output.put_line(mroll || 'is not detained');
update stud set status='ND' where roll_no = mroll;
end if;
Exception
when no_data_found then
dbms_output.put_line(mroll || 'Not found');
End;
/
SQL> select * from stud;
ROLL_NO ATT STATU
---------- ---------- -----123 95 ND
129 99 ND
133 94 ND
137 97 ND
300 60 D
301 70 D
6 rows selected.
Q 2.The bank manager has decided to activate all those accounts which were previously marked as inactive for performing no transaction in last 365 days. 
Write a PL/SQ block (using implicit cursor) to update the status of account, display an approximate message based on the no. of rows affected by the update. 
(Use of %FOUND, %NOTFOUND, %ROWCOUNT)
SQL> CREATE TABLE accounts (
2 account_id INT PRIMARY KEY,
3 customer_name VARCHAR2(100),
4 status VARCHAR2(10) CHECK (status IN ('Active', 'Inactive')),
5 last_transaction_date DATE
6 );
Table created.
SQL> INSERT INTO accounts (account_id, customer_name, status, last_transaction_date)
VALUES (1, 'John Doe', 'Inactive', SYSDATE - 366);
SQL> INSERT INTO accounts (account_id, customer_name, status, last_transaction_date)
VALUES (2, 'Jane Smith', 'Inactive', SYSDATE - 200);SQL> INSERT INTO accounts (account_id, customer_name, status, last_transaction_date)
VALUES (3, 'Alice Johnson', 'Active', SYSDATE - 100);
SQL> INSERT INTO accounts (account_id, customer_name, status, last_transaction_date)
VALUES (4, 'Bob Brown', 'Inactive', SYSDATE - 400);
SQL> INSERT INTO accounts (account_id, customer_name, status, last_transaction_date)
VALUES (5, 'Charlie Davis', 'Active', SYSDATE - 50);
ACCOUNT_ID CUSTOMER_NAME STATUS LAST_TRAN
---------- --------------- ---------- ---------
1 John Doe Inactive 20-APR-23
2 Jane Smith Inactive 03-OCT-23
3 Alice Johnson Active 11-JAN-24
4 Bob Brown Inactive 17-MAR-23
5 Charlie Davis Active 01-MAR-24
DECLARE
v_num_rows NUMBER;
BEGIN
UPDATE accounts
SET status = 'Active'
WHERE status = 'Inactive'
AND last_transaction_date <= SYSDATE - 365;
v_num_rows := SQL%ROWCOUNT;
IF v_num_rows = 0 THEN
DBMS_OUTPUT.PUT_LINE('No accounts were reactivated. No accounts have been
inactive for more than 365 days.');
ELSE
DBMS_OUTPUT.PUT_LINE(v_num_rows || ' account(s) have been reactivated.');
END IF;
EXCEPTIONWHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
END;
ACCOUNT_ID CUSTOMER_NAME STATUS LAST_TRAN
---------- --------------- ---------- ---------
1 John Doe Active 20-APR-23
2 Jane Smith Inactive 03-OCT-23
3 Alice Johnson Active 11-JAN-24
4 Bob Brown Active 17-MAR-23
5 Charlie Davis Active 01-MAR-24
7]
Q 1. Write an SQL code block these raise a user defined exception where business rule is
voilated.BR for client_master table specifies when the value of bal_due field is less than 0 handle the exception.
Declare
input number(10);
client_id number;
bal_due Exception;
Begin
client_id :=& client_id;
input :=&input;
IF input < 0 THEN
raise bal_due;ELSE
INSERT INTO client_master VALUES(client_id,input);
dbms_output.put_line('Inserted successfully');
END IF;
Exception
when bal_due then
dbms_output.put_line(input || 'Your balance is less then 0');
End;
Enter value for client_id: 3
old 6: client_id :=& client_id;
new 6: client_id :=3;
Enter value for input: -10
old 7: input :=&input;
new 7: input :=-10;
-10
Your balance is less then 0
Enter value for client_id: 2
old 6: client_id :=& client_id;
new 6: client_id :=2;
Enter value for input: 100
old 7: input :=&input;
new 7: input :=100;
Inserted successfully
Q 2. Organization has decided to increase the salary of employees by 10% of existing salary, whoare having salary less than average salary of organization, 
Whenever such salary updates takes place, a record for the same is maintained in the increment_salary table.
EMP (E_no , Salary)
increment_salary(E_no , Salary)
SELECT * FROM salary;
EMP_NO SALARY
---------- ----------
129 11000
123 22000
137 30000
DECLARE
CURSOR salhigh IS SELECT emp_no,salary FROM salary WHERE salary < (SELECT
AVG(salary) FROM salary);
memp_no salary.emp_no%type;
msalary salary.salary%type;
BEGIN
OPEN salhigh;
IF salhigh%isopen THEN
LOOP
fetch salhigh into memp_no,msalary;
exit when salhigh%notfound;
if salhigh%found then
update salary set salary = (0.1*msalary+ msalary) WHERE emp_no = memp_no;
insert into increment_salary values(memp_no,0.1*msalary+ msalary);
end if;
end loop;
end if;Close salhigh;
END;
/
PL/SQL procedure successfully completed.
SELECT * FROM salary;
EMP_NO SALARY
---------- ----------
129 12100
123 24200
137 33000
SELECT * FROM increment_salary;
EMP_NO INCREMENT_SALARY
---------- ----------------
121 12100
122 24200
129 33000
8]
Q 1.Borrower(Roll_no, Name, DateofIssue, NameofBook, Status)
Fine(Roll_no,Date,Amt)
1. Accept roll_no& name of book from user.
2. Check the number of days (from date of issue), if days are between 15 to 30 then fine
amount
will be Rs 5per day.
3. If no. of days>30, per day fine will be Rs 50 per day & for days less than 30, Rs. 5 per day.
After submitting the book, status will change from I to R
4. If condition of fine is true, then details will be stored into fine table.
5. Also handles the exception by named exception handler or user define exception
handler.
CREATE PROCEDURE process_borrower_fine (
mroll_no IN NUMBER
)
IS
days INTEGER;
doi DATE;
dor DATE := SYSDATE;
mamt NUMBER;
BEGIN
SELECT dateofissue INTO doi FROM borrower WHERE roll_no = mroll_no;
days := dor - doi;
dbms_output.put_line(days);
UPDATE borrower SET status = 'r' WHERE roll_no = mroll_no;
IF (days > 30) THEN
mamt := (days - 30) * 50 + 75;
INSERT INTO fine VALUES (mroll_no, dor, mamt);
ELSIF (days > 15) THEN
mamt := (days - 15) * 5;
INSERT INTO fine VALUES (mroll_no, dor, mamt);
END IF;
END;
SQL> BEGIN
2 process_borrower_fine(&mroll_no);
3 END;
4 /
SQL> select * from fine;
ROLL_NO DATEOFRET AMT
---------- --------- ----------3 16-FEB-24 40
4 16-FEB-24 150
2 16-FEB-24 5
3 16-FEB-24 40
4 rows selected.
9]
Q 1. Write PL/SQL block using explicit cursor for following requirements:
College has decided to mark all those students detained (D) who are having attendance less than 75%.
Whenever such update takes place, a record for the same is maintained in the D_Stud table.
create table stud21(roll number(4), att number(4), status varchar(1));
create table d_stud(roll number(4), att number(4));
SELECT * FROM stud;
ROLL_NO ATT S
---------- ---------- -
129 90
137 74
123 50
DECLARE
CURSOR check_status IS SELECT roll_no,att FROM stud WHERE att < 75 ;
mroll_no stud.roll_no%type;
matt stud.att%type;
BEGIN
OPEN check_status;
IF check_status%isopen THENLOOP
FETCH check_status INTO mroll_no,matt;
exit WHEN check_status%notfound;
IF check_status%found THEN
UPDATE stud SET status = 'D' WHERE roll_no = mroll_no;
INSERT INTO d_stud VALUES(mroll_no,matt);
END IF;
END LOOP;
END IF;
CLOSE check_status;
END;
/
PL/SQL procedure successfully completed.
SELECT * FROM stud;
ROLL_NO ATT S
---------- ---------- -
129 90
137 74 D
123 50 D
SELECT * FROM d_stud;
ROLL_NO ATT
---------- ----------
137 74
123 50
10]
Q 1.Consider table Stud(Roll, Att,Status)
Write a PL/SQL block for following requirement and handle the exceptions.
Roll no. of student will be entered by user. Attendance of roll no. entered by user will be
checked in Stud table. If attendance is less than 75% then display the message “Term not granted” and set the
status in stud table as “D”. Otherwise display message “Term granted” and set the status in stud table as “ND”.
(Taken From ChatGPT)
DECLARE
v_roll_number Stud.Roll%TYPE;
v_attendance Stud.Att%TYPE;
v_status Stud.Status%TYPE;
BEGIN
-- Accepting roll number from user
v_roll_number := &roll_number;
-- Retrieving attendance and status for the entered roll number
SELECT Att, Status
INTO v_attendance, v_status
FROM Stud
WHERE Roll = v_roll_number;
-- Checking attendance percentage and updating status accordingly
IF v_attendance < 75 THEN
DBMS_OUTPUT.PUT_LINE('Term not granted');
UPDATE Stud
SET Status = 'D'WHERE Roll = v_roll_number;
ELSE
DBMS_OUTPUT.PUT_LINE('Term granted');
UPDATE Stud
SET Status = 'ND'
WHERE Roll = v_roll_number;
END IF;
EXCEPTION
WHEN NO_DATA_FOUND THEN
DBMS_OUTPUT.PUT_LINE('Roll number not found');
WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
END;
/
Q 2.Write a update, delete trigger on clientmstr table. The System should keep track of the
records that ARE BEING updated or deleted. The old value of updated or deleted records should be added
in audit_trade table. (separate implementation using both row and statement triggers)

CREATE OR REPLACE TRIGGER trade_record
AFTER INSERT OR DELETE
ON client
FOR EACH ROW
DECLARE
op VARCHAR(10);
BEGIN
IF updating THEN
op:='update';
END IF;IF deleting THEN
op:='Delete';
END IF;
INSERT INTO trade VALUES(:old.c_id,:old.p_amt,op);
END;
/
Trigger created.
SELECT * FROM client;
C_ID P_AMT
---------- ----------
121 1500
122 1800
129 1500
124 1900
125 3000
SELECT * FROM trade;
no rows selected
CREATE OR REPLACE TRIGGER trade_record
AFTER UPDATE OR DELETE
ON client
FOR EACH ROW
DECLARE
op VARCHAR(10);
BEGIN
IF updating THEN
op:='update';
END IF;IF deleting THEN
op:='Delete';
END IF;
INSERT INTO trade VALUES(:old.c_id,:old.p_amt,op);
END;
/
Trigger created.
SELECT * FROM client;
C_ID P_AMT
---------- ----------
121 1500
122 1800
129 1500
124 1900
125 3000
SELECT * FROM trade;
no rows selected
UPDATE client SET p_amt = 1000 WHERE p_amt=1500;
4 rows updated.
SELECT * FROM trade;
C_ID P_AMT STATUS
---------- ---------- ----------
121 1500 update
129 1500 update
CREATE OR REPLACE TRIGGER trade_record
AFTER UPDATE OR DELETEON client
DECLARE
op VARCHAR(10);
BEGIN
IF updating THEN
op:='update';
END IF;
IF deleting THEN
op:='Delete';
END IF;
INSERT INTO trade VALUES(:old.c_id,:old.p_amt,op);
END;
CREATE TABLE trade_status(status VARCHAR(10));
Table created.
CREATE OR REPLACE TRIGGER trade_record
AFTER UPDATE OR DELETE
ON client
DECLARE
op VARCHAR(10);
BEGIN
IF updating THEN
op:='update';
END IF;
IF deleting THEN
op:='Delete';
END IF;
INSERT INTO trade_status VALUES(op);END;
/
Trigger created.
SELECT * FROM client;
C_ID P_AMT
---------- ----------
121 1000
122 1000
129 1000
124 1000
125 3000
UPDATE client SET p_amt = 1500 WHERE p_amt=1000;
4 rows updated.
SELECT * FROM trade_status;
STATUS
----------
Update
11]
Q 1. Write a stored function in PL/SQL for given requirement and use the same in PL/SQL
block. Account no. and branch name will be accepted from user. The same will be searched in table
acct_details. If status of account is active then display appropriate message and also store the
account details in active_acc_details table, otherwise display message on screen “account is inactive”.
SQL> create table acc_details(acc_no NUMBER primary key , b_name char(10) , status
char(2));
Table created.
SQL> SELECT * FROM acc_details;
ACC_NO B_NAME ST
---------- ---------- --
1 Nigdi A
2 Nigdi I
3 Nigdi A
4 Khed I
5 Khed I
CREATE OR REPLACE FUNCTION account_fun (
macc_no IN NUMBER,
b_name IN CHAR -- Removed size specification
) RETURN CHAR -- Removed size specification
IS
mst CHAR(2); -- It's okay to specify sizes for internal variables
BEGIN
SELECT status INTO mst FROM acc_details
WHERE acc_no = macc_no AND b_name = b_name; -- Use the parameter b_name
RETURN mst;
EXCEPTION
WHEN NO_DATA_FOUND THEN
RETURN NULL; -- Return NULL if no data is found
WHEN OTHERS THEN
RETURN NULL; -- General error handling
END;Function created.
DECLARE
macc_no NUMBER(10);
mst CHAR(2);
b_name CHAR(10); -- Example value for b_name
BEGIN
macc_no := &macc_no;
b_name := '&b_name';
mst := account_fun(macc_no, b_name); -- Corrected function name and added b_name
IF mst = 'A' THEN
DBMS_OUTPUT.PUT_LINE('Account is active'); -- Corrected quotes and added missing
parenthesis
INSERT INTO active_acc_details VALUES(macc_no,b_name,mst);
ELSE
DBMS_OUTPUT.PUT_LINE('Account is Inactive'); -- Corrected quotes and added missing
parenthesis
END IF;
END;
SQL> select * from active_acc_details;
ACC_NO B_NAME ST
---------- -------------------- --
1 1 A
1 1 A
3 Nigdi A
Enter value for macc_no: 2
old 6: macc_no := &macc_no;
new 6: macc_no := 2;
Enter value for b_name: Nigdi
old 7: b_name := '&b_name';new 7: b_name := 'Nigdi';
Account is Inactive
12]
Q 1. Write an SQL code block these raise a user defined exception where business rule is
voilated. BR for client_master table specifies when the value of bal_due field is less than 0 handle the exception.
Declare
input number(10);
client_id number;
bal_due Exception;
Begin
client_id :=& client_id;
input :=&input;
IF input < 0 THEN
raise bal_due;
ELSE
INSERT INTO client_master VALUES(client_id,input);
dbms_output.put_line('Inserted successfully');
END IF;
Exception
when bal_due then
dbms_output.put_line(input || 'Your balance is less then 0');
End;
Enter value for client_id: 3old 6: client_id :=& client_id;
new 6: client_id :=3;
Enter value for input: -10
old 7: input :=&input;
new 7: input :=-10;
-10
Your balance is less then 0
Enter value for client_id: 2
old 6: client_id :=& client_id;
new 6: client_id :=2;
Enter value for input: 100
old 7: input :=&input;
new 7: input :=100;
Inserted successfully
Q 2.Write a before trigger for Insert, update event considering following requirement:
Emp(e_no, e_name, salary)
I) Trigger action should be initiated when salary is tried to be inserted is less than Rs. 50,000/-
II) Trigger action should be initiated when salary is tried to be updated for value less than Rs. 50,000/-
Action should be rejection of update or Insert operation by displaying appropriate error message.
Also the new values expected to be inserted will be stored in new table.
Tracking(e_no, salary).
SELECT * FROM employee;no rows selected
CREATE OR REPLACE TRIGGER emp_sal_record
2 BEFORE UPDATE OR INSERT
3 ON employee
4 FOR EACH ROW
5 DECLARE
6 sal NUMBER:=:new.salary;
7 BEGIN
8 IF sal < 50000 THEN
9 IF updating THEN
10 raise_application_error(-20003,'This update opration violet comapany rule / record
not inserted');
11 END IF;
12 IF inserting THEN
13 raise_application_error(-20003,'This insert opration violet comapany rule / record
not inserted');
14 END IF;
15 ELSE
16 DBMS_OUTPUT.PUT_LINE('Record created successfully');
17 INSERT INTO emp_sal VALUES(:new.e_no,:new.salary);
18 END IF;
19 END;
20 /
Trigger created.
INSERT INTO employee VALUES(129,'Aviraj',100000);
Record created successfully
1 row created.
INSERT INTO employee VALUES(124,'Ankit',1000);INSERT INTO employee VALUES(124,'Ankit',1000)
UPDATE employee SET salary = 1000 WHERE e_no =129;
13]
Q 1. . Write a PL/SQL stored Procedure for following requirements and call the procedure in appropriate PL/SQL block.
Borrower(Rollin, Name, DateofIssue, NameofBook, Status)
Fine(Roll_no,Date,Amt)
Accept roll_no& name of book from user.
1. Check the number of days (from date of issue), if days are between 15 to 30 then fine
amount
will be Rs 5per day.
2. If no. of days>30, per day fine will be Rs 50 per day & for days less than 30, Rs. 5 per day.
3. After submitting the book, status will change from I to R.
4. If condition of fine is true, then details will be stored into fine table.


-- Create Borrower and Fine tables
CREATE TABLE Borrower (
    Rollin NUMBER PRIMARY KEY,
    Name VARCHAR2(100),
    DateofIssue DATE,
    NameofBook VARCHAR2(100),
    Status VARCHAR2(1) CHECK (Status IN ('I', 'R'))
);

CREATE TABLE Fine (
    Roll_no NUMBER,
    Date DATE,
    Amt NUMBER
);

-- Inserting more values into the Borrower table
INSERT INTO Borrower (Rollin, Name, DateofIssue, NameofBook, Status)
VALUES
(104, 'Rakesh', TO_DATE('2024-04-05', 'YYYY-MM-DD'), 'उपनिषद', 'I'),
(105, 'Suresh', TO_DATE('2024-04-12', 'YYYY-MM-DD'), 'वेद', 'I');

-- Inserting values into the Fine table
INSERT INTO Fine (Roll_no, Date, Amt) VALUES (104, TO_DATE('2024-04-23', 'YYYY-MM-DD'), 200);
INSERT INTO Fine (Roll_no, Date, Amt) VALUES (105, TO_DATE('2024-04-20', 'YYYY-MM-DD'), 300);

-- PL/SQL Stored Procedure
CREATE OR REPLACE PROCEDURE Process_Fine (
    p_roll_no IN NUMBER,
    p_name_of_book IN VARCHAR2
)
IS
    v_days INTEGER;
    v_fine_amt NUMBER := 0;
    v_status VARCHAR2(1);
    v_current_date DATE := SYSDATE;
BEGIN
    -- Retrieve Date of Issue and Status for the given roll number
    SELECT DateofIssue, Status INTO v_days, v_status
    FROM Borrower
    WHERE Rollin = p_roll_no AND NameofBook = p_name_of_book;
    
    -- Calculate the number of days since the date of issue
    v_days := TRUNC(v_current_date) - TRUNC(v_days);
    
    -- Change status to 'R' (Returned)
    UPDATE Borrower
    SET Status = 'R'
    WHERE Rollin = p_roll_no AND NameofBook = p_name_of_book;
    
    -- Calculate fine amount based on the number of days
    IF v_days <= 15 THEN
        v_fine_amt := 0; -- No fine within 15 days
    ELSIF v_days > 15 AND v_days <= 30 THEN
        v_fine_amt := (v_days - 15) * 5; -- Rs 5 per day for days between 16 to 30
    ELSE
        v_fine_amt := (v_days - 30) * 50 + 75; -- Rs 50 per day for days greater than 30
    END IF;
    
    -- Insert fine details into the Fine table
    INSERT INTO Fine (Roll_no, Date, Amt)
    VALUES (p_roll_no, v_current_date, v_fine_amt);
    
    COMMIT; -- Commit the transaction
    dbms_output.put_line('Fine processed successfully.');
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        dbms_output.put_line('No record found for the provided roll number and book.');
    WHEN OTHERS THEN
        dbms_output.put_line('An error occurred: ' || SQLERRM);
END;
/

-- PL/SQL Block to call the procedure
DECLARE
    v_roll_no NUMBER;
    v_name_of_book VARCHAR2(100);
BEGIN
    v_roll_no := &roll_no;
    v_name_of_book := '&name_of_book';
    Process_Fine(v_roll_no, v_name_of_book);
END;
/



Problem statement 14.

Q 1. Write a Stored Procedure namely proc_Grade for the categorization of student. If marks
scored by students in examination is <=1500 and marks>=990 then student will be placed in
distinction category if marks scored are between 989 and 900 category is first class, if marks 899 and 825 category is Higher Second Class.
Write a PL/SQL block for using procedure created with above requirement.
Stud_Marks(name, total_marks)
Result(Roll,Name, Class)
-- Creating tables
CREATE TABLE Stud_Marks_1180 (
    roll_no NUMBER PRIMARY KEY,
    name VARCHAR2(100),
    total_marks NUMBER
);

CREATE TABLE Result_1180 (
    Roll NUMBER,
    Name VARCHAR2(100),
    Class VARCHAR2(100)
);

-- Inserting sample data into Stud_Marks_1180 table
INSERT INTO Stud_Marks_1180 (roll_no, name, total_marks)
VALUES
(1, 'राम', 1450),
(2, 'श्याम', 1000),
(3, 'मोहन', 925),
(4, 'सोहन', 800);

-- Displaying the Stud_Marks_1180 table
SELECT * FROM Stud_Marks_1180;

-- Creating the stored procedure proc_Grade
CREATE OR REPLACE PROCEDURE proc_Grade(
    p_name IN VARCHAR2,
    p_marks IN NUMBER,
    p_roll IN NUMBER
)
IS
    v_class VARCHAR2(100);
BEGIN
    IF p_marks <= 1500 AND p_marks >= 990 THEN
        v_class := 'Dist';
    ELSIF p_marks < 990 AND p_marks >= 900 THEN
        v_class := 'First';
    ELSIF p_marks < 900 AND p_marks >= 825 THEN
        v_class := 'Higher';
    ELSE
        v_class := 'Other';
    END IF;
    INSERT INTO Result_1180 VALUES (p_roll, p_name, v_class);
END;

-- Executing the stored procedure
DECLARE
    v_name VARCHAR2(100);
    v_total_marks NUMBER;
    v_roll NUMBER;
BEGIN
    v_roll := &v_roll;
    v_name := '&v_name';
    SELECT total_marks INTO v_total_marks FROM Stud_Marks_1180 WHERE roll_no = v_roll;
    proc_Grade(v_name, v_total_marks, v_roll);
    COMMIT;
END;

-- Displaying the result of the procedure
SELECT * FROM Result_1180;

Problem statement 15.
Create Database PCCOE
Create following Collections
Teachers(Tname,dno,dname,experience,salary,date_of_joining )
Students(Sname,roll_no,class)
Q1. Find the information about all teachers
Q2. Find the information about all teachers of computer department
Q3. Find the information about all teachers of computer,IT,ande&TC department
Q4. Find the information about all teachers of computer,IT,and E&TC department having
salary
greater than or equl to 10000/-
Q5. Find the student information having roll_no = 2 or Sname=xyz
Q6. Update the experience of teacher-praveen to 10years, if the entry is not available in
database
consider the entry as new entry.
Q7. Update the deparment of all the teachers working in IT deprtment to COMP
Q8. Find the teachers name and their experience from teachers collection
Q9. Using Save() method insert one entry in department collection
Q10. Using Save() method change the dept of teacher praveen to IT
Q11. Delete all the doccuments from teachers collection having IT dept.
Q12. Display with pretty() method, the first 3 doccuments in teachers collection in
ascending order.

CREATE DATABASE PCCOE;
CREATE TABLE Teachers_1180 (
    Tname VARCHAR(100),
    dno INT,
    dname VARCHAR(100),
    experience INT,
    salary DECIMAL(10, 2),
    date_of_joining DATE
);

CREATE TABLE Students_1180 (
    Sname VARCHAR(100),
    roll_no INT,
    class VARCHAR(50)
);

-- Inserting sample data into Teachers_1180 table
INSERT INTO Teachers_1180 (Tname, dno, dname, experience, salary, date_of_joining)
VALUES ('Amit', 101, 'Computer', 5, 25000.00, '2020-01-01'),
       ('Sudha', 102, 'IT', 8, 28000.00, '2018-05-15'),
       ('Ravi', 103, 'E&TC', 6, 30000.00, '2019-09-10');

-- Inserting sample data into Students_1180 table
INSERT INTO Students_1180 (Sname, roll_no, class)
VALUES ('Rajesh', 1, 'XII'),
       ('Pooja', 2, 'X'),
       ('Manish', 3, 'XI');


-- Q1. Find the information about all teachers
SELECT * FROM Teachers_1180;

-- Q2. Find the information about all teachers of computer department
SELECT * FROM Teachers_1180 WHERE dname = 'Computer';

-- Q3. Find the information about all teachers of computer, IT, and E&TC department
SELECT * FROM Teachers_1180 WHERE dname IN ('Computer', 'IT', 'E&TC');

-- Q4. Find the information about all teachers of computer, IT, and E&TC department having salary greater than or equal to 10000/-
SELECT * FROM Teachers_1180 WHERE dname IN ('Computer', 'IT', 'E&TC') AND salary >= 10000;

-- Q5. Find the student information having roll_no = 2 or Sname=xyz
SELECT * FROM Students_1180 WHERE roll_no = 2 OR Sname = 'xyz';

-- Q6. Update the experience of teacher-praveen to 10 years, if the entry is not available in database consider the entry as new entry.
UPDATE Teachers_1180 SET experience = 10 WHERE Tname = 'Praveen';

-- Q7. Update the department of all the teachers working in IT department to COMP
UPDATE Teachers_1180 SET dname = 'COMP' WHERE dname = 'IT';

-- Q8. Find the teachers name and their experience from teachers collection
SELECT Tname, experience FROM Teachers_1180;

-- Q9. Using Save() method insert one entry in department collection
-- Assuming you meant inserting into Teachers_1180 table
INSERT INTO Teachers_1180 (Tname, dno, dname, experience, salary, date_of_joining)
VALUES ('NewTeacher', 104, 'Mechanical', 3, 15000.00, '2022-03-01');

-- Q10. Using Save() method change the department of teacher praveen to IT
UPDATE Teachers_1180 SET dname = 'IT' WHERE Tname = 'Praveen';

-- Q11. Delete all the documents from teachers collection having IT dept.
DELETE FROM Teachers_1180 WHERE dname = 'IT';

-- Q12. Display with pretty() method, the first 3 documents in teachers collection in ascending order
SELECT * FROM Teachers_1180 ORDER BY Tname ASC LIMIT 3;


16] 
Consider the relational database
Supplier(Sid,Sname,address)
Parts(Pid, Pname, Color)
Catalog(sid,pid,cost)


-- Step 1: Create Tables
CREATE TABLE Supplier_1180 (
    Sid INT PRIMARY KEY,
    Sname VARCHAR(100),
    address VARCHAR(100)
);

CREATE TABLE Parts_1180 (
    Pid INT PRIMARY KEY,
    Pname VARCHAR(100),
    Color VARCHAR(50)
);

CREATE TABLE Catalog_1180 (
    Sid INT,
    Pid INT,
    cost DECIMAL(10,2),
    FOREIGN KEY (Sid) REFERENCES Supplier_1180(Sid),
    FOREIGN KEY (Pid) REFERENCES Parts_1180(Pid)
);

-- Step 2: Insert Sample Values
-- Inserting sample data into Supplier_1180 table
INSERT INTO Supplier_1180 (Sid, Sname, address) VALUES (1, 'Supplier1', 'Address1');
INSERT INTO Supplier_1180 (Sid, Sname, address) VALUES (2, 'Supplier2', 'Address2');

-- Inserting sample data into Parts_1180 table
INSERT INTO Parts_1180 (Pid, Pname, Color) VALUES (101, 'Part1', 'green');
INSERT INTO Parts_1180 (Pid, Pname, Color) VALUES (102, 'Part2', 'red');

-- Inserting sample data into Catalog_1180 table
INSERT INTO Catalog_1180 (Sid, Pid, cost) VALUES (1, 101, 20);
INSERT INTO Catalog_1180 (Sid, Pid, cost) VALUES (2, 102, 30);

-- Display contents of tables
SELECT * FROM Supplier_1180;
SELECT * FROM Parts_1180;
SELECT * FROM Catalog_1180;

-- Q1. Find name of all parts whose color is green.
SELECT Pname
FROM Parts_1180
WHERE Color = 'green';

-- Q2. Find names of suppliers who supply some red parts.
SELECT DISTINCT Sname
FROM Supplier_1180
WHERE Sid IN (
    SELECT Sid
    FROM Catalog_1180
    WHERE Pid IN (
        SELECT Pid
        FROM Parts_1180
        WHERE Color = 'red'
    )
);

-- Q3. Find names of all parts whose cost is more than Rs25.
SELECT Pname
FROM Parts_1180
WHERE Pid IN (
    SELECT Pid
    FROM Catalog_1180
    WHERE cost > 25
);

Consider the relational database
Person(pname,street city)
Company(cname,city)

Manages(pname,mname)
Q. Find the street and city of all employees who work for “Idea”, live in Pune and earn more than
3000.
Consider the relational database
Student(Rollno,name,address)
Subject(sub_code,sub_name)
Marks(Rollno,sub_code, marks)
Q. Find out average marks of each student along with the name of student.
Q. Find how many students have failed in the subject “DBMS”

-- Step 1: Create Tables
CREATE TABLE Person_1180 (
    pname VARCHAR(100) PRIMARY KEY,
    street VARCHAR(100),
    city VARCHAR(100)
);

CREATE TABLE Company_1180 (
    cname VARCHAR(100) PRIMARY KEY,
    city VARCHAR(100)
);

CREATE TABLE Manages_1180 (
    pname VARCHAR(100),
    mname VARCHAR(100),
    FOREIGN KEY (pname) REFERENCES Person_1180(pname),
    FOREIGN KEY (mname) REFERENCES Company_1180(cname)
);

-- Step 2: Insert Sample Values
-- Inserting sample data into Person_1180 table
INSERT INTO Person_1180 (pname, street, city) VALUES ('John', 'Street1', 'Mumbai');
INSERT INTO Person_1180 (pname, street, city) VALUES ('Alice', 'Street2', 'Pune');
INSERT INTO Person_1180 (pname, street, city) VALUES ('Bob', 'Street3', 'Pune');

-- Inserting sample data into Company_1180 table
INSERT INTO Company_1180 (cname, city) VALUES ('Idea', 'Pune');
INSERT INTO Company_1180 (cname, city) VALUES ('ABC Corp', 'Mumbai');

-- Inserting sample data into Manages_1180 table
INSERT INTO Manages_1180 (pname, mname) VALUES ('John', 'Idea');
INSERT INTO Manages_1180 (pname, mname) VALUES ('Alice', 'Idea');
INSERT INTO Manages_1180 (pname, mname) VALUES ('Bob', 'ABC Corp');

-- Display contents of tables
SELECT * FROM Person_1180;
SELECT * FROM Company_1180;
SELECT * FROM Manages_1180;

-- Q. Find the street and city of all employees who work for “Idea”, live in Pune and earn more than 3000.
SELECT p.street, p.city
FROM Person_1180 p
JOIN Manages_1180 m ON p.pname = m.pname
JOIN Company_1180 c ON m.mname = c.cname
WHERE c.cname = 'Idea' AND p.city = 'Pune';

-- Step 1: Create Tables
CREATE TABLE Student_1180 (
    Rollno INT PRIMARY KEY,
    name VARCHAR(100),
    address VARCHAR(100)
);

CREATE TABLE Subject_1180 (
    sub_code INT PRIMARY KEY,
    sub_name VARCHAR(100)
);

CREATE TABLE Marks_1180 (
    Rollno INT,
    sub_code INT,
    marks DECIMAL(5,2),
    FOREIGN KEY (Rollno) REFERENCES Student_1180(Rollno),
    FOREIGN KEY (sub_code) REFERENCES Subject_1180(sub_code)
);

-- Step 2: Insert Sample Values
-- Inserting sample data into Student_1180 table
INSERT INTO Student_1180 (Rollno, name, address) VALUES (1, 'Alice', 'Address1');
INSERT INTO Student_1180 (Rollno, name, address) VALUES (2, 'Bob', 'Address2');

-- Inserting sample data into Subject_1180 table



17]
Write Pl/SQL code block that will accept account number from user , check if the users
balance is less than the minimum balance , only deduct Rs.100/- from the balance .


CREATE TABLE Account (Acc_no NUMBER PRIMARY KEY, branch_name VARCHAR2(50),
balance NUMBER);
INSERT INTO Account (Acc_no, branch_name, balance)VALUES (1, 'Branch1', 1500);
INSERT INTO Account (Acc_no, branch_name, balance)VALUES (2, 'Branch2', 2000);
INSERT INTO Account (Acc_no, branch_name, balance)VALUES (3, 'Branch3', 100);
DECLARE
v_acc_no NUMBER;
v_balance NUMBER;
v_min_balance NUMBER := 1000; -- Minimum balance requiredBEGIN
-- Accepting account number from user
v_acc_no := &acc_no;
-- Retrieving balance for the entered account number
SELECT balance INTO v_balance
FROM Account
WHERE Acc_no = v_acc_no;
-- Checking if balance is less than minimum balance
IF v_balance < v_min_balance THEN
UPDATE Account
SET balance = balance - 100
WHERE Acc_no = v_acc_no;
DBMS_OUTPUT.PUT_LINE('Deducted Rs.100 from the balance.');
ELSE
DBMS_OUTPUT.PUT_LINE('Balance is above minimum balance.');
END IF;
COMMIT; -- Committing the transaction
EXCEPTION
WHEN NO_DATA_FOUND THEN
DBMS_OUTPUT.PUT_LINE('Account number not found.');
WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
ROLLBACK; -- Rolling back the transaction in case of error
END;
/
18]  Write Pl/SQL code block for inverting number 1234 to 4321.
DECLARE
v_number NUMBER := 1234; -- Number to invert
v_inverted_number NUMBER := 0;
v_remainder NUMBER;
BEGIN
WHILE v_number > 0 LOOP
v_remainder := MOD(v_number, 10);
v_inverted_number := v_inverted_number * 10 + v_remainder;
v_number := (v_number - v_remainder) / 10;
END LOOP;
DBMS_OUTPUT.PUT_LINE('Inverted Number: ' || v_inverted_number);
END;
/

SET SERVEROUTPUT ON; -- Enable output

DECLARE
    v_number NUMBER := 1234; -- Number to invert
    v_inverted_number NUMBER := 0;
    v_remainder NUMBER;
    v_original_number NUMBER := v_number;
BEGIN
    -- Invert the number
    WHILE v_number > 0 LOOP
        v_remainder := MOD(v_number, 10);
        v_inverted_number := v_inverted_number * 10 + v_remainder;
        v_number := (v_number - v_remainder) / 10;
    END LOOP;
    
    -- Reverse the original number
    DECLARE
        v_reversed_number NUMBER := 0;
        v_temp_number NUMBER := v_original_number;
    BEGIN
        WHILE v_temp_number > 0 LOOP
            v_remainder := MOD(v_temp_number, 10);
            v_reversed_number := v_reversed_number * 10 + v_remainder;
            v_temp_number := (v_temp_number - v_remainder) / 10;
        END LOOP;
        
        -- Check if the inverted number matches the reverse of the original number
        IF v_inverted_number = v_reversed_number THEN
            DBMS_OUTPUT.PUT_LINE('The number ' || v_original_number || ' is successfully inverted.');
        ELSE
            DBMS_OUTPUT.PUT_LINE('Error: The number ' || v_original_number || ' is not inverted correctly.');
        END IF;
    END;
END;
/

Problem statements 19
The bank manager has decided to mark all those accounts as inactive (I) on which there
are no transactions performed in last 365 days. Whenever any such update takes place a record
for the same is maintained in the INACT_MASTER_TABLE comprising of the account number, theopening date and type of account. Write PL/SQL code block to do the same(cursor for
loop) (We doesn’t have for loop cursor)

-- Step 1: Create Tables
CREATE TABLE Account_1180 (
    account_number NUMBER PRIMARY KEY,
    opening_date DATE,
    last_transaction_date DATE,
    account_type VARCHAR2(50)
);

CREATE TABLE INACT_MASTER_TABLE_1180 (
    account_number NUMBER,
    opening_date DATE,
    account_type VARCHAR2(50)
);

-- Step 2: Insert Sample Values
-- Inserting sample data into Account_1180 table
INSERT INTO Account_1180 (account_number, opening_date, last_transaction_date, account_type)
VALUES
(1001, TO_DATE('2023-04-22', 'YYYY-MM-DD'), TO_DATE('2023-04-21', 'YYYY-MM-DD'), 'Savings'),
(1002, TO_DATE('2023-03-15', 'YYYY-MM-DD'), TO_DATE('2023-03-14', 'YYYY-MM-DD'), 'Current'),
(1003, TO_DATE('2022-12-10', 'YYYY-MM-DD'), TO_DATE('2023-01-09', 'YYYY-MM-DD'), 'Savings'),
(1004, TO_DATE('2023-02-05', 'YYYY-MM-DD'), TO_DATE('2023-03-15', 'YYYY-MM-DD'), 'Savings');

-- Display contents of Account_1180 table before PL/SQL execution
SELECT * FROM Account_1180;

-- Display contents of INACT_MASTER_TABLE_1180 before PL/SQL execution
SELECT * FROM INACT_MASTER_TABLE_1180;

-- Step 3: PL/SQL Code Block
DECLARE
    v_account_number Account_1180.account_number%TYPE;
    v_opening_date Account_1180.opening_date%TYPE;
    v_account_type Account_1180.account_type%TYPE;
BEGIN
    FOR rec IN (SELECT account_number, opening_date, account_type FROM Account_1180 WHERE last_transaction_date < SYSDATE - 365) LOOP
        v_account_number := rec.account_number;
        v_opening_date := rec.opening_date;
        v_account_type := rec.account_type;
        
        -- Mark account as inactive
        UPDATE Account_1180
        SET last_transaction_date = NULL
        WHERE account_number = v_account_number;
        
        -- Insert into INACT_MASTER_TABLE_1180
        INSERT INTO INACT_MASTER_TABLE_1180 (account_number, opening_date, account_type)
        VALUES (v_account_number, v_opening_date, v_account_type);
        
        COMMIT; -- Commit after each record to avoid transaction rollback
    END LOOP;
END;
/

-- Display contents of Account_1180 table after PL/SQL execution
SELECT * FROM Account_1180;

-- Display contents of INACT_MASTER_TABLE_1180 after PL/SQL execution
SELECT * FROM INACT_MASTER_TABLE_1180;



20] Write PL/SQL code block that will merge the data available in the newly created table NEW_BRANCHES with the data available in the table BRANCH_MASTER. If the data in the
first table already exists in the second table then data should be skipped.(parameterized
cursor)
(We doesn’t have parameterize cursor)

-- Step 1: Create Tables
CREATE TABLE BRANCH_MASTER_after_1180 (
    branch_id NUMBER PRIMARY KEY,
    branch_name VARCHAR2(100),
    branch_location VARCHAR2(100)
);

CREATE TABLE NEW_BRANCHES_after_1180 (
    branch_id NUMBER PRIMARY KEY,
    branch_name VARCHAR2(100),
    branch_location VARCHAR2(100)
);

-- Step 2: Insert Sample Values
-- Inserting sample data into BRANCH_MASTER_after_1180 table
INSERT INTO BRANCH_MASTER_after_1180 (branch_id, branch_name, branch_location)
VALUES
(1, 'Main Branch', 'New York'),
(2, 'Downtown Branch', 'Los Angeles');

-- Inserting sample data into NEW_BRANCHES_after_1180 table
INSERT INTO NEW_BRANCHES_after_1180 (branch_id, branch_name, branch_location)
VALUES
(3, 'Uptown Branch', 'Chicago'),
(4, 'Eastside Branch', 'Miami');

-- Step 3: Display Tables
-- Display contents of BRANCH_MASTER_after_1180 table
SELECT * FROM BRANCH_MASTER_after_1180;

-- Display contents of NEW_BRANCHES_after_1180 table
SELECT * FROM NEW_BRANCHES_after_1180;

-- Step 4: PL/SQL Code Block
DECLARE
    v_branch_id NEW_BRANCHES_after_1180.branch_id%TYPE;
    v_branch_name NEW_BRANCHES_after_1180.branch_name%TYPE;
    v_branch_location NEW_BRANCHES_after_1180.branch_location%TYPE;
BEGIN
    FOR rec IN (SELECT branch_id, branch_name, branch_location FROM NEW_BRANCHES_after_1180) LOOP
        -- Check if data already exists in BRANCH_MASTER_after_1180
        SELECT COUNT(*) INTO v_branch_id FROM BRANCH_MASTER_after_1180 WHERE branch_id = rec.branch_id;
        
        -- If data doesn't exist, insert into BRANCH_MASTER_after_1180
        IF v_branch_id = 0 THEN
            INSERT INTO BRANCH_MASTER_after_1180 (branch_id, branch_name, branch_location)
            VALUES (rec.branch_id, rec.branch_name, rec.branch_location);
        END IF;
    END LOOP;
    COMMIT; -- Commit changes
END;
/

-- Display contents of BRANCH_MASTER_after_1180 table after merge operation
SELECT * FROM BRANCH_MASTER_after_1180;


21]
Write PL/SQL code block such that depending upon user supplied account number, the
customer to whom account belongs , the introducer of that account are inserted into
ACCOUNT_MASTER_INFO table .If the user enters an account number that is not in the
ACCOUNT_MASTER table, then the PL/SQL block must display appropriate error
message(Exception Handling)

CREATE TABLE ACCOUNT_MASTER_INFO (
    acc_no NUMBER PRIMARY KEY,
    cust_name VARCHAR2(100),
    introducer VARCHAR2(100)
);

INSERT INTO ACCOUNT_MASTER VALUES (123456789, 'John Doe', 'Jane Smith');
INSERT INTO ACCOUNT_MASTER VALUES (987654321, 'Alice Johnson', 'Bob Brown');

DECLARE
    v_acc_no NUMBER := &account_number; -- User-supplied account number
    v_cust_name VARCHAR2(100);
    v_introducer VARCHAR2(100);
BEGIN
    SELECT cust_name, introducer
    INTO v_cust_name, v_introducer
    FROM ACCOUNT_MASTER
    WHERE acc_no = v_acc_no;

    INSERT INTO ACCOUNT_MASTER_INFO (acc_no, cust_name, introducer)
    VALUES (v_acc_no, v_cust_name, v_introducer);
    
    DBMS_OUTPUT.PUT_LINE('Record inserted successfully.');
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Account number not found in ACCOUNT_MASTER table.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
END;
/

-- Test with an existing account number
EXECUTE;
-- Test with a non-existing account number
EXECUTE;


Problem statements 22
A stored function is created to perform the ACCOUNT_NO check operation .F_checkAccNO() is
the name of function which accept a variable ACCOUNT_NO and returns the value to host
environment The value changes from 0(if ACCOUNT_NO does not exist) to 1(if ACCOUNT_NO
exist) depending on the records retrieved.
CREATE TABLE ACCOUNT_MASTER_1180 (
    acc_no NUMBER PRIMARY KEY
);
INSERT INTO ACCOUNT_MASTER_1180 VALUES (123456789);
INSERT INTO ACCOUNT_MASTER_1180 VALUES (987654321);
CREATE OR REPLACE FUNCTION F_checkAccNO (p_account_no IN NUMBER)
RETURN NUMBER
IS
    v_count NUMBER;
BEGIN
    -- Check if the account number exists in the table
    SELECT COUNT(*)
    INTO v_count
    FROM ACCOUNT_MASTER_1180
    WHERE acc_no = p_account_no;

    -- Return 1 if account number exists, otherwise return 0
    RETURN CASE WHEN v_count > 0 THEN 1 ELSE 0 END;
END;
-- Call the function with an existing account number
SELECT F_checkAccNO(123456789) AS result FROM dual;

-- Call the function with a non-existing account number
SELECT F_checkAccNO(999999999) AS result FROM dual;


23] 
create a row level trigger for the CUSTOMERS table that would fire for INSERT or UPDATE
or DELETE operations performed on the CUSTOMERS table. This trigger will display the salary
difference between the old values and new values
-- Create CUSTOMERS_1180 table
CREATE TABLE CUSTOMERS_1180 (
    customer_id NUMBER PRIMARY KEY,
    name VARCHAR2(50),
    salary NUMBER
);

-- Insert some sample data into CUSTOMERS_1180 table
INSERT INTO CUSTOMERS_1180 VALUES (1, 'Rohan', 25000);
INSERT INTO CUSTOMERS_1180 VALUES (2, 'Priya', 30000);
INSERT INTO CUSTOMERS_1180 VALUES (3, 'Amit', 35000);
INSERT INTO CUSTOMERS_1180 VALUES (4, 'Neha', 28000);
INSERT INTO CUSTOMERS_1180 VALUES (5, 'Deepak', 32000);

-- Create a row level trigger for CUSTOMERS_1180 table
CREATE OR REPLACE TRIGGER customers_1180_salary_trigger
BEFORE INSERT OR UPDATE OR DELETE ON CUSTOMERS_1180
FOR EACH ROW
DECLARE
    v_old_salary NUMBER;
    v_new_salary NUMBER := :NEW.salary;
BEGIN
    IF INSERTING OR UPDATING THEN
        v_old_salary := :OLD.salary;
        DBMS_OUTPUT.PUT_LINE('Salary difference: ' || (v_new_salary - v_old_salary));
    ELSIF DELETING THEN
        DBMS_OUTPUT.PUT_LINE('Salary deleted: ' || :OLD.salary);
    END IF;
END;
/
SELECT * FROM CUSTOMERS_1180;


-- Test the trigger by updating the salary of a customer
UPDATE CUSTOMERS_1180 SET salary = 28000 WHERE customer_id = 1;


-- Test the trigger by updating the salary of a customer
UPDATE CUSTOMERS SET salary = 28000 WHERE customer_id = 1;

24] 
Write PL/SQL block to update the Customer table and increase the salary of each customer
by 500 and use the SQL%ROWCOUNTattribute to determine the number of rows affected.
-- Create Customer_1180 table
CREATE TABLE Customer_1180 (
    customer_id NUMBER PRIMARY KEY,
    name VARCHAR2(50),
    salary NUMBER
);

-- Insert some sample data into Customer_1180 table
INSERT INTO Customer_1180 VALUES (1, 'Rohan', 25000);
INSERT INTO Customer_1180 VALUES (2, 'Priya', 30000);
INSERT INTO Customer_1180 VALUES (3, 'Amit', 35000);
INSERT INTO Customer_1180 VALUES (4, 'Neha', 28000);
INSERT INTO Customer_1180 VALUES (5, 'Deepak', 32000);

-- PL/SQL block to update the Customer_1180 table and increase the salary by 500
DECLARE
    v_num_rows NUMBER;
BEGIN
    -- Update the Customer_1180 table to increase the salary by 500
    UPDATE Customer_1180 SET salary = salary + 500;
    
    -- Get the number of rows affected by the update
    v_num_rows := SQL%ROWCOUNT;
    
    -- Display the number of rows affected
    DBMS_OUTPUT.PUT_LINE('Number of rows updated: ' || v_num_rows);
END;
/
