---
layout: default
title: Laboratorium 5
subtitle: Podzapytania.
---

Napisz zapytanie SQL do bazy danych `HR`, które spowoduje wyświetlenie:

1. Nazwisk (`last_name`) oraz wypłat (`salary`) pracowników, którzy zarabiają ponad 90% wypłaty pracownika o nazwisku `Chen` *(tabela `employees` -> 42 rows, 2 columns)*
2. Nazwisk (`last_name`), nazw działów (`department_name`) oraz wypłat (`salary`) pracowników, którzy zarabiają więcej, niż każdy pracownik działu '`Marketing`' *(`ALL`, tabele `employees` oraz `departments` -> 5 rows, 3 columns)*.
3. Nazw działów (`department_name`) i ilości pracowników, w których pracuje więcej pracowników, niż w dziale '`IT`' *(tabele `employees` oraz `departments` -> 4 rows, 2 columns)*
4. [\*] Nazwisk managerów (`last_name`), ich wypłat oraz ilości pracowników, którzy są do nich przypisani *(tabela `employees` -> 18 rows, 3 columns) (podpowiedź: użyj podzapytania do stworzenia osobnej tabeli, w której są numery kierowników i ilości ich pracowników, którą łączymy z tabelą employees poprzez JOIN ON, a ilość pracowników z podzapytania do zapytania głównego przenosimy poprzez alias..)*
5. [\*\*] Nazwisk pracowników (`last_name`) oraz ich wypłat (`salary`), którzy nie są kierownikami, a zarabiają więcej niż ich szefowie *(tabela `employees` -> 2 rows, 2 columns) UWAGA! Wykorzystując klauzulę "NOT IN" musimy zapewnić interpreter, że lista nie posiada pustych wartości, inaczej zapytanie nie wyświetli wyników! (np. poprzez dodanie warunku WHERE cośtam > 0 ;) )*

##Przykłady

* Pozdapytanie - zapytanie SQL zawarte wewnątrz zapytania, poprzez umieszczenie go w nawiasach; wynik podzapytania może być wykorzystany jako tabela lub wartość pojedyncza, np. Wyświetl nazwiska pracowników o numerze większym, niż pracownik o nazwisku (`last_name`) Chen:

>	SELECT last_name FROM employees
	WHERE employee_id >
	(SELECT employee_id FROM employees WHERE last_name='Chen')

* Podzapytanie jako tabela - wynik podzapytania możemy wykorzystać jako tabelę, którą możemy połączyć z oryginalną tabelą w bazie danych, np. Wyświetl nazwy działów (`department_name`), ich numery (`department_id`) oraz ilości pracowników, którzy w nich pracują *(uwaga! COUNT(\*) musi być używany jedynie z kolumną, która występuje w `GROUP BY`! Nie możemy wyświetlać innych kolumn!)*:

>	SELECT department_name, department_id, "ilosc prac"
	FROM departments JOIN (SELECT department_id, COUNT(*) "ilosc prac"
	FROM employees GROUP BY department_id) USING(department_id)

* Klauzule `ALL` / `ANY` - określają, że warunek muszą spełnić wszystkie wiersze (`ALL`) lub co najmniej jeden wiersz (`ANY`), np. Wyświetl nazwiska pracowników (`last_name`) o numerach (`employee_id`) większych, niż co najmniej jeden (`ANY`) z pracowników działu (`department_name`) `IT`:

>	SELECT last_name FROM employees
	WHERE employee_id > ANY
	(SELECT employee_id FROM employees JOIN departments
	USING (department_id) WHERE department_name='IT')

* Ograniczenie `IN` - odnosi się do warunków, jeśli chcemy aby wartości w ograniczanej kolumnie znajdowały się wśród wartości będących wynikiem podzapytania, np. Wyświetl nazwiska pracowników (`last_name`), których wypłata (`salary`) jest taka sama, jak któregokolwiek pracownika działu (`department_name`) `IT`:

>	SELECT last_name FROM employees WHERE salary IN
	(SELECT salary FROM employees JOIN departments USING (department_id)
	WHERE department_name='IT')
