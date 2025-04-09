# Traccia:

`Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
ogni Corso può essere tenuto da diversi Insegnanti;
ogni Corso prevede più appelli d'Esame;
ogni Studente è iscritto ad un solo Corso di Laurea;
ogni Studente può iscriversi a più appelli di Esame;
per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella`

## University DB

## Departments

**Columsn**
- id (INT, primary key)
- name (VARCHAR(255), NOT NULL)

## Degree_Courses

**Columsn**
- id (INT, primary key)
- name (VARCHAR(255), NOT NULL)
- department_id (INT, foreign key references Departments(id), NOT NULL)
- description (TEXT, NOT NULL)
- duration (INT, NOT NULL) # in years
- degree_type (enum('Bachelor', 'Master'), NOT NULL)


## Courses

**Columsn**
- id (INT, primary key)
- name (VARCHAR(255), NOT NULL)
- department_id (INT, foreign key references Departments(id), NOT NULL)
- description (TEXT, NOT NULL)


## Teachers

**Columsn**
- id (INT, primary key)
- name (VARCHAR(255), NOT NULL)
- surname (VARCHAR(255), NOT NULL)
- email (VARCHAR(255), not null, unique)
- phone (VARCHAR(20), NOT NULL)
- department_id (INT, foreign key references Departments(id), NOT NULL)


## Students
**Columsn**
- id (INT, primary key)
- name (VARCHAR(255), NOT NULL)
- surname (VARCHAR(255), NOT NULL)
- email (VARCHAR(255), not null, unique)
- phone (VARCHAR(20), NOT NULL)
- degree_course_id (INT, foreign key references Degree_Courses(id), NOT NULL)
- enrollment_date (DATE, NOT NULL)

## Exams
**Columsn**
- id (INT, primary key)
- course_id (INT, foreign key references Courses(id), NOT NULL)
- date (DATE, NOT NULL)
- location (VARCHAR(255), NOT NULL)
- teacher_id (INT, foreign key references Teachers(id), NOT NULL)
- student_id (INT, foreign key references Students(id), NOT NULL)

## Exam_Results
**Columsn**
- id (INT, primary key)
- exam_id (INT, foreign key references Exams(id), NOT NULL)
- student_id (INT, foreign key references Students(id), NOT NULL)
- grade (INT, NOT NULL)
- date (DATE, NOT NULL)
- status (enum('Passed', 'Failed'), NOT NULL) # Passed or Failed
- feedback (TEXT, NOT NULL)