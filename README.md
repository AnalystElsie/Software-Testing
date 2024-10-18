# Software-Testing
Designed and implemented a database system tailored to the education sector. The system will manage student enrollments, courses, instructors, and academic records. Created tables based on the provided scenario and Populated the table with at least 500 records using T-SQL.
## Problem Statement
As a database developer consultant for an educational institution, your task is to design and implement a database system tailored to the education sector. The system will manage student enrollments, courses, instructors, and academic records.

### Task Details: 
As the database consultant for the education sector, your tasks include:
- Database Design and Normalization:
- Design and normalize the database into 3NF, providing detailed explanations and justifications.
- Use T-SQL statements in Microsoft SQL Server Management Studio, supported by screenshots.
- Create tables and views using T-SQL, highlighting primary keys, foreign keys, and data types.
- Apply constraints for data integrity.

## Skills
- T-SQL
## Entity Relationship Diagram
![ERD](https://github.com/user-attachments/assets/32e5c4d0-574c-4072-99a7-bf3a3737fbfd)

## Code Snippets
```sql
CREATE TABLE Students
(
	Student_ID INT IDENTITY(1,1) PRIMARY KEY,
	First_Name VARCHAR (20),
	Last_Name VARCHAR (20),
	Full_Name VARCHAR (50),
	Email VARCHAR(100) UNIQUE,
	Gender VARCHAR(10),
	Dates_of_Birth DATE
);
CREATE TABLE Instructors
(
	Instructor_ID INT IDENTITY(1,1) PRIMARY KEY,
	First_Name VARCHAR (20),
	Last_Name VARCHAR(20),
	Email VARCHAR(50) UNIQUE,
	Gender VARCHAR(10),
	Years_Of_Experience INT
);
CREATE TABLE Courses
(
	Course_ID INT IDENTITY(1,1) PRIMARY KEY,
	Course_Name VARCHAR(50),
	Course_Code VARCHAR(20),
	Course_Availabilty VARCHAR(3),
	Course_Level INT,
	Instructor_ID INT FOREIGN KEY REFERENCES Instructors(Instructor_ID)
);

CREATE TABLE Grades (
Grade_ID INT IDENTITY (1,1)PRIMARY KEY, 
Grade VARCHAR (1)
);

CREATE TABLE Enrollment_Status
(
 EnrollmentStatus_ID INT IDENTITY (1,1) PRIMARY KEY,
   Enrollment_Status VARCHAR(20) CHECK (Enrollment_Status IN ('Enrolled', 'Not Enrolled', 'Pending')),
CREATE TABLE Enrollment_Table
(
	Enrollment_ID int identity (1,1) PRIMARY KEY,
	Student_ID int FOREIGN KEY REFERENCES Students(Student_ID),
	Course_ID int FOREIGN KEY REFERENCES Courses (Course_ID),
	Enrollmentstatus__ID Int FOREIGN KEY REFERENCES Enrollment_Status (EnrollmentStatus_ID),
	Enrollment_Date Date,
	Grade_ID int FOREIGN KEY REFERENCES Grades(Grade_ID),
	Instructor_ID int FOREIGN KEY REFERENCES Instructors(Instructor_ID),
	GPA Decimal(2,1)
);

declare @Current_Record int =1
declare @No_of_Records int =500
declare @Student_ID int
declare @Course_ID INT
declare @EnrollmentStatus_ID int
declare @Enrollment_Date date
declare @Start_Date date ='2024-01-01'
declare @End_Date date ='2024-03-31'
declare @Num_Days int =Datediff(day,@start_Date,@End_Date)
declare @Grade_ID int
declare @Instructor_ID INT
declare @GPA decimal(2,1)
while @Current_Record<=@No_of_Records
Begin
select @Student_ID = cast(rand()*(select Max(Student_ID) from Students) as int)+1
select @Course_ID = cast(rand() *(select max(Course_ID) from Courses) as int)+1
select @EnrollmentStatus_ID = Cast(rand()*(select Max(EnrollmentStatus_ID) from Enrollment_Status) as int)+1
select @Enrollment_Date =dateadd(day,cast(rand()*datediff(day,@Start_Date,@End_Date)as int)+1, @Start_Date)
Select @Grade_ID = cast(rand() *(select max(Grade_ID) from Grades) as int)+1
Select @Instructor_ID =cast(rand() *(select max(Instructor_ID) from Instructors) as int)+1
select @GPA = case 
				when @Grade_ID = 1 then ROUND((RAND() * 1.5) + 3.5, 1)
				when @Grade_ID = 2 then ROUND((RAND() * 0.3) + 3.1, 1)
				when @Grade_ID = 3 then ROUND((RAND() * 0.5) + 2.5, 1)
				when @Grade_ID = 4 then ROUND((RAND() * 0.3) + 2.1, 1)
				when @Grade_ID = 5 then ROUND((RAND() * 0.5) + 1.5, 1)
				when @Grade_ID = 6 then ROUND((RAND() * 1.4), 1)
				End
Insert into Enrollment_Table(Student_ID,Course_ID,Enrollmentstatus__ID,Enrollment_Date,Grade_ID,Instructor_ID,GPA)
Values( @Student_ID,@Course_ID,@EnrollmentStatus_ID,@Enrollment_Date,@Grade_ID,@Instructor_ID,@GPA)
Select @Current_Record =@Current_Record+ 1
End;
```
