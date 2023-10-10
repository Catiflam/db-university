# ESERCIZIO

Il lavoro di oggi va aggiunto alla repo di ieri.
Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema allegato, eseguite le query del file allegato.
Cosa consegnare?
Dopo aver testato le vostre query con phpMyAdmin, riportatele in un file md e caricatelo nella vostra repo.

## QUERY

1. Selezionare tutti gli studenti nati nel 1990 (160)
   SELECT `name`,`surname`,`date_of_birth` FROM `students` WHERE `date_of_birth` LIKE '1990-%';
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   SELECT `name`,`cfu` FROM `courses` WHERE `cfu` > 10;
3. Selezionare tutti gli studenti che hanno più di 30 anni
   SELECT `name`,`surname`,`date_of_birth` FROM `students` WHERE `date_of_birth`>= '1993-01-01';
   SELECT \* FROM `students` WHERE TIMESTAMPDIFF(year,`date_of_birth`,curdate()) >30;
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)
   SELECT `name`,`description`,`year`,`period` FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1;
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)
   SELECT \* FROM `exams` WHERE `date`LIKE '2020-06-20' AND `hour`>= '14-%';
6. Selezionare tutti i corsi di laurea magistrale (38)
   SELECT \* FROM `degrees` WHERE `name` LIKE '%Magistrale%';
7. Da quanti dipartimenti è composta l'università? (12)
   SELECT COUNT(\*) "numero_dipartimenti" FROM `departments`;
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
   SELECT COUNT(\*) "Insegnanti_senza_telefono" FROM `teachers` WHERE `phone` IS NULL;

## QUERY CON JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
   SELECT `students`.`name`,`students`.`surname`,`degrees`.`name`"corso di laurea in economia" FROM `students` INNER JOIN `degrees` ON `degrees`.`id`= `students`.`degree_id`;
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
   SELECT `departments`.`name`"Dipartimento di Neuroscienze",`degrees`.`name`"corsi" FROM `departments` INNER JOIN `degrees` ON `departments`.`id` =`degrees`.`department_id` WHERE `departments`.`name`LIKE "Dipartimento di Neuroscienze" AND `degrees`.`name`LIKE "%Magistrale%";
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
   SELECT \* FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id`= `course_teacher`.`teacher_id` INNER JOIN `courses` ON `course_teacher`.`course_id`=`courses`.`id` WHERE `teachers`.`id`= 44;
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
   SELECT `students`.`surname`, `students`.`name`,`students`.`registration_number`,`degrees`.`name`,`departments`.`name` FROM `students` INNER JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id`= `departments`.`id` ORDER BY `students`.`surname` ASC, `students`.`name` ASC;
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
   SELECT `degrees`.`name`,`courses`.`name`,`courses`.`period`,`courses`.`year`,`courses`.`cfu`,`teachers`.`name`,`teachers`.`surname` FROM `degrees` INNER JOIN `courses` ON `degrees`.`id`= `courses`.`degree_id` INNER JOIN `course_teacher` ON `courses`.`id`= `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`= `teachers`.`id`;
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)
   SELECT DISTINCT `teachers`.`name`,`teachers`.`surname`,`teachers`.`phone`,`teachers`.`email`,`teachers`.`office_address`,`teachers`.`office_number` FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id`= `course_teacher`.`teacher_id` INNER JOIN `courses` ON `course_teacher`.`course_id`= `courses`.`id` INNER JOIN `degrees` ON `courses`.`degree_id`= `degrees`.`id` INNER JOIN `departments` ON `degrees`.`department_id`= `department_id` WHERE `departments`.`name`= 'Dipartimento di Matematica' ORDER BY `teachers`.`name` ASC;
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.
   SELECT COUNT(\*) AS `numero tentativi`, `students`.`id` AS `id_studente`, `students`.`name`AS `nome studente`, `students`.`surname` AS `cognome studente`, MAX(`exam_student`.`vote`) AS `voto`, `courses`.`id`AS `id_corso`, `courses`.`name`AS `nome corso` FROM `students` INNER JOIN `exam_student` ON `students`.`id`= `exam_student`.`student_id` INNER JOIN `exams` ON `exam_student`.`exam_id`= `exams`.`id` INNER JOIN `courses` ON `exams`.`course_id`= `courses`.`id` GROUP BY `students`.`id`,`courses`.`id`;

## QUERY CON GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno
   SELECT COUNT(\*) AS `numero_iscritti`, YEAR(`enrolment_date`) AS `anno` FROM `students` GROUP BY `anno`;
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
   SELECT COUNT(\*) AS `numero_insegnanti`,`office_address` FROM `teachers` GROUP BY `office_address`;
3. Calcolare la media dei voti di ogni appello d'esame
   SELECT `exam_id`, AVG(`vote`) FROM `exam_student` GROUP BY `exam_id`;
4. Contare quanti corsi di laurea ci sono per ogni dipartimento.
   SELECT COUNT(\*) FROM `degrees` GROUP BY `department_id`;
