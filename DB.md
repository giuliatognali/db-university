1. Seleziona tutti gli studenti nati nel 1990 (160)
SELECT * 
FROM `students` 
WHERE YEAR(`date_of_birth`) = 1990;

2. Seleziona tutti i corsi che valgono più di 10 crediti (497)
SELECT * 
FROM `courses`
 WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni
SELECT *
FROM `students`
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laure (286)
SELECT * 
FROM `courses` 
WHERE `period` = 'I semestre' 
AND `year` = 1;

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
SELECT * 
FROM `exams` 
WHERE `date` = '2020-06-20' 
AND `hour` >= '14:00:00';

SELECT * 
FROM `exams` 
WHERE `date` = '2020-06-20' 
AND HOUR (`hour`) >= 14;

6. Selezionare tutti i corsi di laurea magistrale
SELECT * 
FROM `degrees`
 WHERE `level` = 'magistrale';

7. Da quanti dipartimenti è composta l'università? (12)
SELECT COUNT('id') AS `numero_dipartimenti` 
FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT COUNT(`id`) 
FROM `teachers` 
WHERE `phone` IS NULL;

///////////

9. Corsi raggruppati in magistrali e triennali
SELECT `level`, COUNT(`id`) 
FROM `degrees` 
GROUP BY `level`;

10. Corsi raggruppati per crediti formativi
SELECT `cfu`, COUNT(`id`) 
FROM `courses` 
GROUP BY `cfu`;

11. Ragruppare gli studenti per anno di nascita
SELECT YEAR(`date_of_birth`), COUNT(`id`)
FROM `students` 
GROUP BY  YEAR(`date_of_birth`);

12. Esami di luglio raggruppati per giorno
SELECT COUNT(`id`), DAY(`date`) 
FROM `exams`
WHERE MONTH(`date`) = 7 
GROUP BY DAY(`date`);

///////////////

1. Selezionare tutti i cortsi del Corso di Laurea di Informatica

SELECT *
FROM `courses` 
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Informatica';

2. Selezionare le informazioni sul corso con id = 144, con tutti i relativi appelli d'esame
SELECT * 
FROM `courses` 
JOIN `exams` ON `courses`.`id` = `exams`.`course_id` 
WHERE `courses`.`id` = 144;

3. Selezionare a quale dipartimento appartiene il corso di Laurea in Diritto dell'Economia

SELECT `departments`.*, `degrees`.`name`
FROM `departments`
JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Diritto dell\'Economia';

4. Selezionare tutti gli appelli d'esame del corso di Laurea Magistrale in Fisica del primo anno.

SELECT `degrees`.`name`, `courses`.`year`, `exams`.`date`, `exams`.`hour`,  `exams`.`location`, `exams`.`address`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `exams` ON `courses`.`id` = `exams`.`course_id`
WHERE `degrees`.`name` = 'Corso di Laurea Magistrale in Fisica'
AND `courses`.`year` = 1;

5. Selezionare tutti i docenti che insegnano nel Corso di Laurea di Lettere.

SELECT DISTINCT `teachers`.`name`, `teachers`.`surname`, `teachers`.`phone`, `teachers`.`email`
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Lettere'
ORDER BY `teachers`.`surname`;

6. Selezionare il libretto univsrsitario di Mirco Messina 
SELECT *
FROM `students`
JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
JOIN `exams` ON `exam_student`.`student_id` = `exams`.`id`
JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
WHERE `students`.`registration_number` = 620320;

7. Selezionare il voto medio di superamento d'esame per ogni corso, con anche i dati del corso di laurea associato, ordinati per media voto decrescente

SELECT AVG (`exam_student`.`vote`) AS `media_voto`, `courses`.`name`, `degrees`.`name`
FROM `exam_student`
JOIN `exams`ON `exam_student`.`exam_id` = `exams`.`id`
JOIN `courses` ON `courses`.`id`= `exams`.`course_id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
WHERE `exam_student`.`vote` >= 18
GROUP BY `courses`.`id`;