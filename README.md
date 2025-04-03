# hackathonaishu2
CREATE DATABASE IF NOT EXISTS BlindFriendlyCodingDB;
USE BlindFriendlyCodingDB;

CREATE TABLE users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    FullName VARCHAR(255) NOT NULL,
    Email VARCHAR(255) UNIQUE NOT NULL,
    PasswordHash VARCHAR(255) NOT NULL,
    Role ENUM('Developer', 'Mentor', 'Admin') NOT NULL
);

CREATE TABLE codesnippets (
    SnippetID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    Title VARCHAR(255) NOT NULL,
    ProgrammingLanguage VARCHAR(50) NOT NULL,
    Code TEXT NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES users(UserID) ON DELETE CASCADE
);

CREATE TABLE accessibilitytools (
    ToolID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    Description TEXT NOT NULL,
    Compatibility VARCHAR(255) NOT NULL
);

CREATE TABLE debugginglogs (
    LogID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    ErrorMessage TEXT NOT NULL,
    DebuggingSteps TEXT NOT NULL,
    Timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES users(UserID) ON DELETE CASCADE
);

CREATE TABLE mentorship (
    MentorshipID INT PRIMARY KEY AUTO_INCREMENT,
    MentorID INT,
    DeveloperID INT,
    StartDate DATE,
    Status ENUM('Active', 'Completed', 'Pending'),
    FOREIGN KEY (MentorID) REFERENCES users(UserID) ON DELETE CASCADE,
    FOREIGN KEY (DeveloperID) REFERENCES users(UserID) ON DELETE CASCADE
);

-- Insert sample users
INSERT INTO Users (FullName, Email, PasswordHash, Role) VALUES 
('John Doe', 'john@example.com', 'hashedpassword1', 'Developer'),
('Alice Smith', 'alice@example.com', 'hashedpassword2', 'Mentor'),
('Bob Admin', 'bob@example.com', 'hashedpassword3', 'Admin')
AS newUser
ON DUPLICATE KEY UPDATE Email = newUser.Email;

-- Insert sample code snippets
INSERT INTO CodeSnippets (UserID, Title, ProgrammingLanguage, Code) VALUES 
(1, 'Hello World in Python', 'Python', 'print("Hello, World!")'),
(1, 'Basic HTML Structure', 'HTML', '<!DOCTYPE html><html><head><title>Test</title></head><body></body></html>')
AS newSnippet
ON DUPLICATE KEY UPDATE Title = newSnippet.Title;

-- Insert sample accessibility tools
INSERT INTO AccessibilityTools (Name, Description, Compatibility) VALUES 
('JAWS', 'Screen reader for visually impaired users.', 'Windows'),
('NVDA', 'Free and open-source screen reader.', 'Windows'),
('VoiceOver', 'Built-in screen reader for macOS.', 'macOS')
AS newTool
ON DUPLICATE KEY UPDATE Name = newTool.Name;

-- Insert sample debugging logs
INSERT INTO DebuggingLogs (UserID, ErrorMessage, DebuggingSteps) VALUES 
(1, 'SyntaxError: Unexpected token', 'Checked for missing semicolons and unmatched brackets.'),
(1, 'ModuleNotFoundError: No module named numpy', 'Installed the missing package using pip install numpy.')
AS newLog
ON DUPLICATE KEY UPDATE ErrorMessage = newLog.ErrorMessage;

-- Insert sample mentorship data
INSERT INTO Mentorship (MentorID, DeveloperID, StartDate, Status) VALUES 
(2, 1, '2025-04-01', 'Active')
AS newMentorship
ON DUPLICATE KEY UPDATE Status = newMentorship.Status;
-- Fetch Code Snippets Created by a User
SELECT Title, ProgrammingLanguage, Code FROM CodeSnippets WHERE UserID = 1;

-- Get Active Mentorships
SELECT U1.FullName AS Mentor, U2.FullName AS Developer, M.StartDate, M.Status
FROM Mentorship M
JOIN Users U1 ON M.MentorID = U1.UserID
JOIN Users U2 ON M.DeveloperID = U2.UserID
WHERE M.Status = 'Active';

-- Retrieve debugging logs for a specific user
SELECT ErrorMessage, DebuggingSteps, Timestamp FROM DebuggingLogs WHERE UserID = 1;

-- List all available accessibility tools
SELECT Name, Description, Compatibility FROM AccessibilityTools;
