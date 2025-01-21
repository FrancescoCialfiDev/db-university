// 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql

SELECT GROUP_CONCAT(CONCAT(`students`.`name`, " ", `students`.`surname`) SEPARATOR ", ") AS `students`, `degrees`.`name` AS `degrees` FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`department_id`
GROUP BY `degrees`.`name`

```

// 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

```sql

SELECT  `departments`.`id`, `departments`.`name`,  `degrees`.`name` FROM departments
JOIN `degrees` ON `department_id` = `departments`.`id`
WHERE `departments`.`name` = "Dipartimento di Neuroscienze"

```

// 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql

SELECT CONCAT(`teachers`.`name`, " ", `teachers`.`surname`) AS `full_name`, GROUP_CONCAT(`courses`.`name` SEPARATOR ", ") AS `name_course` FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `teacher_id`
JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
WHERE `teachers`.`id` = "44"
GROUP BY `full_name`

```

// 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

```sql

SELECT CONCAT(`students`.`surname`," ", `students`.`name`  ) AS `full_name`, `degrees`.`name` AS `cours_name`, `degrees`.`level`, `degrees`.`address`, `degrees`.`email`, `degrees`.`website`  FROM `students`
JOIN `degrees` ON `degree_id` = `degrees`.`id`
ORDER BY (`full_name`) ASC

```

// 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql

SELECT
`teachers`.`name` AS `teacher_name`,
`degrees`.`name` AS `name_degree`,
 `degrees`.`level` AS `level_degree`,
 `courses`.`name` AS `name_course`,
 `courses`.`period` AS `period_course`
 FROM `degrees`
JOIN `courses` ON `degree_id` = `degrees`.`id`
JOIN `course_teacher` ON `courses`.`id` = `course_id`
JOIN `teachers` ON `teachers`.`id` = `teacher_id`;

```

// 6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

```sql

SELECT DISTINCT CONCAT(`teachers`.`name`," ",  `teachers`.`surname`) AS `teacher_fullname`,  `departments`.`name` AS `department_name`
FROM `teachers`
JOIN  `course_teacher` ON `teacher_id` = `teachers`.`id`
JOIN `courses` ON `course_id` = `courses`.`id`
JOIN `degrees` ON `degree_id` = `degrees`.`id`
JOIN  `departments` ON `departments`.`id` = `department_id`
WHERE `departments`.`name` = "Dipartimento di Matematica"

```

// 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18

```sql

SELECT
    CONCAT(`students`.`name`, " ", `students`.`surname`) AS `student_fullname`,
    MAX(`exam_student`.`vote`) AS `max_vote_exam`,
    COUNT(`exams`.`date`) AS `total_try`
FROM `students`
JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
WHERE `exam_student`.`vote` >= 18
GROUP BY `student_fullname`
HAVING `total_try` > 0;

```
