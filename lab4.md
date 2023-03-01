---
layout: default
title: Laboratorium 4
---

Napisz zapytanie SQL do bazy danych `HR`, które spowoduje wyświetlenie:

1. Nazwisk pracowników (`last_name`), wszystkich nazw działów (`department_name`) oraz wszystkich miast (`city`), w których mogą pracować *(`OUTER JOIN`, tabele `employees`, `departments` oraz `locations` -> 138 rows, 3 columns)*
2. Nazw działów (`department_name`), które zatrudniają ponad 5 pracowników wraz z ilościami pracowników, którzy w nich pracują, wykorzystując klauzulę `NATURAL JOIN` *(`HAVING`, tabele `departments` oraz `employees` -> 2 rows, 2 columns)*
3. Nazw działów (`department_name`), które zatrudniają ponad 5 pracowników wraz z ilościami pracowników, którzy w nich pracują, nie wykorzystując klauzuli `NATURAL JOIN` *(`HAVING`, tabele `departments` oraz `employees` -> 4 rows, 2 columns)*
4. Nazwisk pracowników (`last_name`) oraz ich wypłat (`salary`) wyrażone w procentach wartości środkowej płacy ustalonej dla danego stanowiska, zaokrąglone do wartości całkowitych. Posortuj wyniki malejąco po wartościach procentowych. *(kolumny `min_salary` oraz `max_salary` w tabeli `jobs` to minimalne oraz maksymalne płace ustalone dla każdego stanowiska) (`ROUND`, tabele `employees` oraz `jobs` -> 107 rows, 2 columns)*
5. [\*] Nazw działów (`department_name`), w których pracuje co najmniej dwóch pracowników, którzy zarabiają ponad 80% maksymalnej płacy ustalonej na swoim stanowisku (`max_salary`), wraz z ilościami pracowników spełniającymi to kryterium. *(tabele `employees`, `departments` oraz `jobs` -> 3 rows, 2 columns)*
6. [\*] Nazw działów (`department_name`), ilości pracowników, którzy w nich pracują oraz rozpiętości płac (`salary`), wyrażone w procentach zaokrąglonych do dwóch miejsc po przecinku. *(tabele `employees` oraz `departments` -> 11 rows, 3 columns) (small tip: rozpiętość szeregu wartości liczymy ze wzorku: (MAX-MIN)/(AVG*2), gdzie MAX to największa wartość w szeregu, MIN to najmniejsza wartość w szeregu, a AVG to średnia wartość wszystkich w szeregu - w tym wypadku są to funkcje na kolumnie `salary`)*
7. [\*\*] Nazwisk pracowników (`last_name`) i dat ich zatrudnienia (`hire_date`), którzy zostali zatrudnieni wcześniej, niż pracownik o numerze `123`. *(tabela `employees` -> 47 rows, 2 columns) (uwaga! trzeba użyć podzapytania (ang. nested query), nie było tego jeszcze na laboratorium.)*
