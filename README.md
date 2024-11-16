## SQL-ASSIGNMENT-10  
   
### **Create a table named teachers with fields id, name, subject, experience and salary and insert 8 rows**
   
   *CREATE TABLE teachers ( teacher_id INT AUTO_INCREMENT PRIMARY KEY,   
    name VARCHAR(50),
   subject VARCHAR(30),   
    experience int,  
   salary DECIMAL(10, 2));*
   
   *INSERT INTO teachers (teacher_id, name, subject, experience,salary) VALUES  
   (1, 'Fazil', 'Mathamatics', 12,50000);*  

   *INSERT INTO teachers ( name, subject, experience,salary) VALUES
( 'Shahid', 'physics', 10,45000),( 'Kiran', 'Chemistry', 15,65000),
( 'Jonah', 'Biology', 8,47000),( 'Tessa', 'Hindi', 11,48500),
( 'Maria', 'Malayalam', 15,55000),( 'Suresh', 'English', 20,75000),
( 'Supriya', 'History', 25,88500);*

### **Create a before insert trigger named before_insert_teacher that will raise an error “salary cannot be negative” if the salary inserted to the table is less than zero**  
*DELIMITER $$   
CREATE TRIGGER before_insert_teachers  
BEFORE INSERT ON teachers   
FOR EACH ROW   
BEGIN   
if new.salary<0   
then  
SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT= "salary cannot be negative";  
end if;  
END $$   
DELIMITER ;*  

### Create an after insert trigger named after_insert_teacher that inserts a row with teacher_id,action, timestamp to a table called teacher_log when a new entry gets inserted to the teacher table. tecaher_id -> column of teacher table, action -> the trigger action, timestamp -> time at which the new row has got inserted.  
*DELIMITER $$   
CREATE TRIGGER after_teacher_insert  
after insert ON teachers  
FOR EACH ROW   
BEGIN   
INSERT into teachers_log(log_id,teacher_id,action)   
values(log_id,NEW.teacher_id,'NEW Teacher entry');  
END $$   
DELIMITER ;*  

*CREATE TABLE teachers_log ( 
log_id INT AUTO_INCREMENT PRIMARY KEY, 
teacher_id INT, 
action VARCHAR(70), 
action_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP 
);*

Select * from teachers_log;
### Create a before delete trigger that will raise an error when you try to delete a row that has experience greater than 10 years.  
*DELIMITER $$   
CREATE TRIGGER before_teacher_delete  
before delete ON teachers  
FOR EACH ROW   
BEGIN 
if old.experience>10
then
SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT= 'you dont have the rights to delete this teacher';
end if;
END $$ 
DELIMITER ;*

*Delete from teachers where name=’maria’*

Select * from teachers;

### Create an after delete trigger that will insert a row to teacher_log table when that row is deleted from teacher table.

*DELIMITER $$   
CREATE TRIGGER after_teacher_delete    
after delete ON teachers    
FOR EACH ROW     
BEGIN   
INSERT into teachers_log(log_id,action) values(old.teacher_id,'retired teacher');  
END $$   
DELIMITER ;*  

*DROP TRIGGER IF EXISTS before_teacher_delete;  
SET SQL_SAFE_UPDATES = 0 ;*  

*delete  from teachers where name ='maria';*  













   
 
