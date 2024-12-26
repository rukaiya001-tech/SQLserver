CREATE DATABASE TechGuide; GO

-- Drop database TechGuide;

USE TechGuide; GO

CREATE TABLE Chapters ( ChapterID INT PRIMARY KEY IDENTITY(1,1), ChapterName NVARCHAR(50) NOT NULL, -- Chapter name ChapterVideo NVARCHAR(MAX) -- Chapter video );

CREATE TABLE Subjects ( SubCode INT PRIMARY KEY, -- Subject code SubName NVARCHAR(50) NOT NULL, -- Subject name ChapterID INT NULL, -- References Chapters FOREIGN KEY (ChapterID) REFERENCES Chapters(ChapterID) ON DELETE CASCADE );

CREATE TABLE Department ( DepID INT PRIMARY KEY IDENTITY(1,1), DepName NVARCHAR(30) NOT NULL, -- Department name SubCode INT NULL, -- References Subjects FOREIGN KEY (SubCode) REFERENCES Subjects(SubCode) ON DELETE CASCADE );

CREATE TABLE Semester ( SemesterID INT PRIMARY KEY IDENTITY(1,1), SemesterName NVARCHAR(40) NOT NULL, -- Semester name DepID INT NULL, -- References Department FOREIGN KEY (DepID) REFERENCES Department(DepID) ON DELETE CASCADE );

CREATE TABLE Types ( TypeID INT PRIMARY KEY IDENTITY(1,1), TypeName NVARCHAR(20) NOT NULL -- Type name );

CREATE TABLE Course ( CourseID INT PRIMARY KEY IDENTITY(1,1), CourseName NVARCHAR(40), -- Course name SemesterID INT NULL, -- References Semester TypeID INT NULL, -- References Types StartDate DATE DEFAULT GETDATE(), -- Default start date EndDate DATE, -- End date FOREIGN KEY (SemesterID) REFERENCES Semester(SemesterID) ON DELETE CASCADE, FOREIGN KEY (TypeID) REFERENCES Types(TypeID) ON DELETE CASCADE );

CREATE TABLE Users ( UserID INT PRIMARY KEY IDENTITY(1,1), UserName NVARCHAR(50) NOT NULL UNIQUE, -- User name PasswordHash NVARCHAR(256) NOT NULL, -- Encrypted password Role NVARCHAR(20) CHECK (Role IN ('Admin', 'Student', 'Teacher')), -- User role CreatedDate DATE DEFAULT GETDATE() -- Account creation date );

CREATE TABLE AuditLogs ( LogID INT PRIMARY KEY IDENTITY(1,1), TableName NVARCHAR(50), -- Affected table Operation NVARCHAR(20), -- Type of operation OperationDate DATETIME DEFAULT GETDATE(), -- Timestamp of operation UserID INT, -- References Users FOREIGN KEY (UserID) REFERENCES Users(UserID) );

CREATE TABLE ErrorLogs ( ErrorID INT PRIMARY KEY IDENTITY(1,1), ErrorMessage NVARCHAR(MAX), -- Error details ErrorDate DATETIME DEFAULT GETDATE(), -- Error occurrence date UserID INT NULL, -- Optional reference to Users FOREIGN KEY (UserID) REFERENCES Users(UserID) );

CREATE TABLE Configurations ( ConfigID INT PRIMARY KEY IDENTITY(1,1), ConfigKey NVARCHAR(50) NOT NULL UNIQUE, -- Configuration key ConfigValue NVARCHAR(MAX), -- Configuration value LastUpdated DATE DEFAULT GETDATE() -- Last update date );

CREATE TABLE CoursePrerequisites ( CourseID INT NOT NULL, -- Course PrerequisiteID INT NOT NULL, -- Prerequisite course PRIMARY KEY (CourseID, PrerequisiteID), FOREIGN KEY (CourseID) REFERENCES Course(CourseID) ON DELETE CASCADE, FOREIGN KEY (PrerequisiteID) REFERENCES Course(CourseID) ON DELETE NO ACTION );
