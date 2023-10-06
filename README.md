# ESERCIZIO

Il lavoro di oggi va aggiunto alla repo di ieri.
Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema allegato, eseguite le query del file allegato.
Cosa consegnare?
Dopo aver testato le vostre query con phpMyAdmin, riportatele in un file md e caricatelo nella vostra repo.

## QUERY PHPMYADMIN

1. Selezionare tutti gli studenti nati nel 1990 (160)
   SELECT `name`,`surname`,`date_of_birth` FROM `students` WHERE `date_of_birth` LIKE '1990-%';
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   SELECT `name`,`cfu` FROM `courses` WHERE `cfu` > 10;
3. Selezionare tutti gli studenti che hanno più di 30 anni
   SELECT `name`,`surname`,`date_of_birth` FROM `students` WHERE `date_of_birth`>= '1993-01-01';
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)
   SELECT `name`,`description`,`year`,`period` FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1;
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)
   SELECT \* FROM `exams` WHERE `date`LIKE '2020-06-20' AND `hour`>= '14-%';
6. Selezionare tutti i corsi di laurea magistrale (38)
   SELECT \* FROM `degrees` WHERE `name` LIKE '%Magistrale%';
7. Da quanti dipartimenti è composta l'università? (12)
   SELECT `id`,`name`COUNT(`name`) AS `totale_dipartimenti`FROM `departments`;
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
