---
layout: default
title: Laboratorium 3
---

Napisz zapytanie SQL do bazy danych `HR`, które spowoduje wyświetlenie:

1. Nazw działów (`department_name`), nazwisk (`last_name`) oraz płac (`salary`) pracowników, którzy zarabiają ponad `12000`, bez wykorzystania klauzuli `JOIN` *(tabele `employees` oraz `departments` -> 8 rows, 3 columns)*
2. Nazw działów (`department_name`), nazwisk (`last_name`) oraz płac (`salary`) pracowników, którzy zarabiają ponad `12000`, wykorzystując klauzulę `JOIN` *(tabele `employees` oraz `departments` -> 8 rows, 3 columns)*
3. Nazwisk pracowników (`last_name`), nazw działów (`department_name`) i miast (`city`), w których pracują, bez wykorzystania klauzuli `JOIN` *(tabele `employees`, `departments` oraz `locations` -> 106 rows, 3 columns)*
4. Nazwisk pracowników (`last_name`), nazw działów (`department_name`) i miast (`city`), w których pracują, wykorzystując klauzulę `JOIN` *(tabele `employees`, `departments` oraz `locations` -> 106 rows, 3 columns)*
5. Nazw działów (`department_name`) i ilości pracowników, którzy w nich pracują *(tabele `employees` oraz `departments` -> 11 rows, 2 columns)*
6. [\*] Nazw państw (`country_name`) i ilości pracowników, którzy w nich pracują *(4 rows, 2 columns)*
7. [\*\*] Nazwisk pracowników (`last_name`) i nazwisk ich kierowników (`last_name`), posortowane rosnąco po nazwiskach kierowników. Użyj aliasów kolumn. *(106 rows)*
