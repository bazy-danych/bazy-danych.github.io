---
layout: default
title: Laboratorium 6
---

**UWAGA !! Działamy na własnych kontach! :)**

Napisz zapytanie SQL do bazy danych, które spowoduje:

1. Dodanie do tabeli `employees` wiersza opisującego pracownika o numerze (`employee_id`) `10`, o dowolnych wartościach we wszystkich wymaganych kolumnach (atrybut kolumny `Nullable` = "`No`") oraz numerze kierownika (`manager_id`) takim samym, jak numer kierownika (`manager_id`) pracownika o nazwisku `Chung`. Nie wypełniaj pozostałych kolumn. *(podpowiedź: Wykorzystaj podzapytanie. Pamiętaj, że wartości typu `VARCHAR2` oraz `DATE` muszą być wpisywane w pojedynczych apostrofach, sprawdź też ograniczenia nałożone na kolumny w tabeli `employees`, np. w widoku "`Constraints`")*
2. Zmodyfikowanie w tabeli `employees` wiersza opisującego pracownika o numerze (`employee_id`) `10`, nadając mu pensję (`salary`) o wartości średniej pensji pracowników, których nazwisko (`last_name`) rozpoczyna się od litery `K`. *(podpowiedź: Wykorzystaj podzapytanie.)*
3. [\*] Zmodyfikowanie w tabeli `employees` wiersza opisującego pracownika o numerze (`employee_id`) `10`, nadając mu numer kierownika (`manager_id`) taki sam jak numer kierownika pracownika, który zarabia najwięcej w firmie. *(tak, to ma być ta wartość ;) )*
4. [\*] Usunięcie z tabeli `employees` wszystkich wierszy opisujących pracowników, którzy nie mają przypisanego numeru telefonu.

Jeśli cos poszło nie tak i chcesz przywrócić początkową bazę danych, uruchom [skrypt tworzący bazę danych](hr.sql).

**UWAGA !! Wszystkie polecenia modyfikujące bazy danych (`INSERT INTO`, `UPDATE` lub `DELETE FROM`) wykonujemy na własnej bazie danych! ;)**



##Przykłady

### Insert

`INSERT INTO .. (..) VALUES (..)` - dodaje do tabeli określonej po `INTO` jeden wiersz o wartościach określonych po `VALUES` - wartości przypisywane są kolumnom w nawiasach odpowiednio pierwsza wartośc pierwsze kolumnie itd..

#### Format ogólny:

> INSERT INTO table_name (column1, column2, column3,...)
  VALUES (value1, value2, value3,...)

#### Przykład:

* Dodaj dział o nazwie `Studenci` i numerze `280` do tabeli `departments`, podaj jedynie wymagane wartości (atrybut `Nullable = 'No'`).

> INSERT INTO departments (department_id, department_name)
  VALUES (280,'Studenci');

### Update

`UPDATE .. SET .. WHERE ..` - zmienia wartości w tabeli określonej po `UPDATE`, w kolumnach określonych po `SET`, w wierszach określonych przez warunek `WHERE`

#### Format ogólny:

> UPDATE table_name
  SET column1=value, column2=value2,...
  WHERE some_column=some_value

#### Przykład:

* Zmień nazwę działu `Studenci` na `Studentki` w tabeli `departments`.

> UPDATE departments SET department_name='Studentki' WHERE department_name='Studenci';

### Delete

`DELETE FROM .. WHERE ..` - usuwa z tabeli określonej po `FROM` wiersze określone w warunku `WHERE`

#### Format ogólny:
> DELETE FROM table_name
  WHERE some_column=some_value

#### Przykład:

* Usuń dział `Studenki` z tabeli `departments`.

> DELETE FROM departments WHERE department_name='Studentki';
