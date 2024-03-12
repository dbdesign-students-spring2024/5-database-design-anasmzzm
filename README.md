# Data Normalization and Entity-Relationship Diagramming
#### Anas Moazzam

## Original Form
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

 ## Why Isn't This Table 4NF-Compliant?
To begin with, for a database to be 4NF-Compliant, it must first be 2NF and 3NF compliant. In this case, the table does not fall under these levels of compliancy for the following reasons:

- Multi-valued Dependencies
  - The professor field depends on the course_id but not on the student_id or assignment_id
- Non-key Attributes
  - Attributes like relevant_reading depend on other non-key attributes, such as assignment_topic, instead of the entire key of the relation.
- Redundancy
  - Repetition of course_name for every section and assignment associated with a course leads to unnecessary duplication.

## How Can we Fix This?
In order to get this table 4NF-Compliant, we must break it down into multiple table such that there are no tables that contain multiple independent multi-valued facts regarding an entity.

### Tables

#### Students
| student_id | student_name  |
|------------|---------------|
| 1          | John Doe      |
| 2          | Peter Griffen |
| 3          | James Bond    |
| 4          | Albert Smith  |
| ...        | ...           |

The primary key is student_id and identifies the name of the student. Each record represents a unique student with attributes solely dependent on the student entity.

#### Professors
| professor_id | professor_name | professor_email    |
|--------------|----------------|--------------------|
| 1            | Hawking         | s.hawking@foo.edu   |
| 2            | Armstrong       | n.armstrong@foo.edu  |
| 3            | Einstein        | a.einstein@foo.edu  |
| ...          | ...             | ...                |

The primary key is professor_id which gives information on the professor's name and contact information. Each record corresponds to a single professor with attributes dependent solely on the professor entity. 

#### Courses
| course_id | course_name     |
|-----------|-----------------|
| 1         | Database Design |
| 2         | Intro to CS |
| 3         | Spanish 1     |
| 4         | Text and Ideas          |
| 5         | Piano     |

The primary key is course_id which identifies the specific course being accessed. Each course as a unique entity with attributes dependent solely on the course.

#### Sections
| section_id | course_id | professor_id | classroom  |
|------------|-----------|--------------|------------|
| 1          | 1         | 1            | WWH 101    |
| 2          | 2         | 2            | 60FA 314   |
| 3          | 3         | 3            | WWH 201    |
| ...        | ...       | ...          | ...        |

The primary key is section_id which uses course_id and professor-id as foreign keys to tell us information about the course and which professor is teaching. This table will also store the classroom information. Each record will tell us about a single section entity without any unaccounted multilevel facts.

#### Assignments

| assignment_id | course_id | section_id | assignment_topic   | due_date |
|---------------|-----------|------------|--------------------|----------|
| 1             | 1         | 1          | Data normalization | 23.02.21 |
| 2             | 4         | 2          | Descartes Key Points | 18.11.21 |
| 3             | 2         | 1          | Python and pandas  | 15.03.21 |
| ...           | ...       | ...        | ...                | ...      |

The primary key is assignment_id which uses course-id and section_id as foreign keys. The table will tell us information about the course, course section, assignment topic, and the due date of the assignment. Each assignment is a unique entity with attributes (assignment_id, course_id, section_id, assignment_topic, due_date) dependent solely on the assignment entity.

#### Readings

| reading_id | course_id | section_id | assignment_id | relevant_reading   |
|------------|-----------|------------|---------------|--------------------|
| 1          | 1         | 1          | 1             | Deumlich Chapter 3 |
| 2          | 1         | 3          | 1             | Loopin Chapter 3 |
| 3          | 5         | 1          | 3             | Bach Chapter 14 |
| ...        | ...       | ...        | ...           | ...                |

The primary key is reading_id with assignment_id as a foreign key. This table provides information on relevant readings within the class. A seperate table is necessary in case the reading isn't specific to an assignment.

#### Grades

| student_id | assignment_id | grade |
|------------|---------------|-------|
| 1          | 1             | 90    |
| 5          | 2             | 78    |
| 4          | 1             | 83    |
| 2          | 12             | 90    |
| 2          | 2             | 55    |
| ...        | ...           | ...   |

The primary key is student_id and assignment_id is the foreign key. The table allows us to input grades specific to each student per assignment.

## Entity-Relationship Diagram