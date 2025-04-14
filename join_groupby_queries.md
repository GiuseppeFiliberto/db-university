1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
    - degrees
    - students
SELECT `students`.`id` as `students_id`, `students`.`name`, `students`.`surname`, `degrees`.`name`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = "Corso di Laurea in Economia"

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
	- courses 
	- degrees 
	- departments
SELECT `courses`.`id` as `course_id`, `courses`.`name` as `course_name`, `courses`.`cfu`, `degrees`.`name`as `degree_name`, `departments`.`name` as `department_name`  
FROM `courses`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `degrees`.`name` LIKE "Corso di Laurea Magistrale%"
AND `departments`.`name` = "Dipartimento di Neuroscienze"

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
	-course_teacher
	-courses 
    -teachers
SELECT * 
FROM `course_teacher`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`name` = "Fulvio"
AND `teachers`.`surname` = "Amato"

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
	 - students
	 - degrees
	 - departments
SELECT `students`.`name`,`students`.`surname`,`degrees`.`name` as `degree_name`, `departments`.`name` as department_name
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname`, `students`.`name`

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
	-degrees
    -teachers
    -course
    -course_teacher
SELECT DISTINCT `degrees`.`name` as `degree_name`, `courses`.`name` as `courses_name`, `teachers`.`name`, `teachers`.`surname`
FROM `course_teacher`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
	-course_teacher
    -teachers
    -courses
    -degrees
    -departments
SELECT `teachers`.`name`, `teachers`.`surname`,`departments`.`name` as `department_name`
FROM `course_teacher`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = "Dipartimento di Matematica"

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18
 - exam_student
 - students
 - exams
SELECT `students`.`id` as `student_id`, `students`.`name`, `students`.`surname`, `courses`.`name` as `course_name` , COUNT(`courses`.`name`) as exam_attempts, MAX(`exam_student`.`vote`) as `highest_vote`
FROM `exam_student`
JOIN `students` ON `exam_student`.`student_id` = `students`.`id`
JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
WHERE `exam_student`.`vote` >= 18
GROUP BY `student_id` , `course_name`


## Group By

1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(`students`.`enrolment_date`), COUNT(`id`)
FROM `students`
GROUP BY YEAR(`students`.`enrolment_date`)

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT `teachers`.`office_address`, COUNT(`id`)
FROM `teachers`
GROUP BY `teachers`.`office_address`
*/

3. Calcolare la media dei voti di ogni appello d'esame
SELECT AVG (`exam_student`.`vote`) as `vote_average`, `exam_student`.`exam_id` 
FROM `exam_student`
GROUP BY `exam_id`

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT COUNT(`degrees`.`id`) as `num_of_degrees`, `departments`.`name`
FROM `degrees`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
GROUP BY `departments`.`name`