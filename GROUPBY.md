1. Contare quanti iscritti ci sono stati ogni anno

SELECT YEAR(`students`.`enrolment_date`) AS `anno_iscrizione`, COUNT(`id`) AS `studenti`
FROM `students`
GROUP BY `anno_iscrizione`;

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT COUNT(`id`) AS `insegnanti`, `office_address` 
FROM `teachers`
GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame

SELECT `exam_id`, FLOOR(AVG(`vote`)) AS `media_voti`
FROM `exam_student`
GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT COUNT(`id`), `department_id`
FROM `degrees` 
GROUP BY `department_id`;