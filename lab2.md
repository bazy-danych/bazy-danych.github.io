---
layout: default
title: Laboratorium 2
---

Posiłkując się menu *Object Browser* oraz *SQL Commands*, napisz zapytanie SQL do bazy danych `HR`, które spowoduje wyświetlenie:

1. Kodów stanowisk (`job_id`) bez powtarzania oraz numerów kierowników (`manager_id`) dla kierowników o numerach `100` lub `102`. *(`DISTINCT`, `AND`/`OR` tabela employees -> 6 rows, 2 columns)*
2. Nazwisk (`last_name`) oraz wypłat (`salary`) pracowników przypisanych do kierownika o numerze (`manager_id`) `100`, których wypłaty wynoszą ponad `10000`, posortowanych malejąco pod względem wypłaty. *(`AND`/`OR`, tabela `employees` -> 9 rows, 2 columns)*
3. Nazwisk (`last_name`), wypłat (`salary`) oraz kodów stanowisk (`job_id`) pracowników, których kod stanowiska kończy się na "`_MGR`", posortowanych malejąco pod względem wypłaty (`salary`). Uzyj polskich nazw kolumn. *(`LIKE`, tabela `employees` -> 2 rows, 3 columns)*
4. Kodów stanowisk (`job_id`) oraz nazw stanowisk (`job_title`), dla których nazwa stanowiska składa się co najmniej z dwóch wyrazów. Użyj polskich nazw kolumn. *(tabela `jobs` -> 16 rows, 2 columns)*
5. Numerów (`employee_id`), wyplat (`salary`) oraz kodów stanowisk (`job_id`) pracowników, których wypłata wynosi ponad `3000` oraz kod stanowiska rozpoczyna się od "`ST`" lub "`SH`", posortowanych rosnąco pod względem numeru pracownika. *(tabela `employees` -> 23 rows, 3 columns)*
6. Liczby pracowników przypisanych do każdego kodu stanowiska (`job_id`) wraz z odpowiadającymi kodami stanowisk, posortowanych malejąco pod względem liczby pracowników. *(`COUNT(*)`, `GROUP BY`, tabela `employees` -> 19 rows, 2 columns)*
7. Numerów kierowników (`manager_id`) oraz największych wypłat (`salary`) pracowników, którzy są do nich przypisani. *(`MAX`, `GROUP BY`, tabela `employees` -> 19 rows, 2 columns)*
8. Kodów stanowisk (`job_id`) oraz średnich wypłat (`salary`) pracowników, którzy są do nich przypisani, posortowanych malejąco po średnich wypłatach. Użyj polskich nazw kolumn. *(`AVG`, `GROUP BY`, tabela `employees` -> 19 rows, 2 columns)*
9. Kodów stanowisk (`job_id`), liczby przypisanych do nich pracowników oraz najmniejszych, średnich i największych wypłat (`salary`) dla pracowników, którzy są do nich przypisani oraz zarabiają ponad `5000`, posortowanych rosnąco po najmniejszych wypłatach. Użyj polskich nazw kolumn. *(`COUNT(*)`, `MIN`, `AVG`, `MAX`, `GROUP BY`, tabela `employees` -> 15 rows, 5 columns)*
10. [\*] Numerów kierowników (`manager_id`), których pracownicy zarabiają średnio ponad `5000` oraz średnich wypłat wszystkich pracowników do nich przypisanych. *(tip: Google "SQL HAVING" ;), tabela `employees` -> 12 rows, 2 columns)*
