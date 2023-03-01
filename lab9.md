---
layout: default
title: Laboratorium 9
---

**UWAGA! Działamy na własnym koncie!**

Przed rozpoczęciem ćwiczeń, uruchom skrypt tworzący bazę danych `HR` i stwórz tabele pomocnicze do triggerów:

> CREATE TABLE emp_gotrich ( emp_id NUMBER(6), raise_date DATE, old_sal NUMBER(8,2), new_sal NUMBER(8,2) );<br>
CREATE TABLE emp_count ( ilosc NUMBER(5), data DATE );<br>
CREATE TABLE emp_promoted ( emp_id NUMBER(6), prom_date DATE );<br>
CREATE TABLE dept_changes ( data DATE, akcja VARCHAR2(16) );<br>

1. Utwórz trigger, który wpisuje do tabeli `emp_gotrich` numer pracownika, datę zmiany, starą wypłatę oraz nową wypłatę kiedy nowa wypłata (`salary`) pracownika przekroczy `5000` (a poprzednia była mniejsza lub równa). *(wykorzystaj `IF` w bloku `BEGIN` `END` lub jeśli chcesz korzystać z `WHEN` w nagłówku triggera, użyj `NEW.salary` bez ":")*
2. Utwórz trigger, który podnosi płacę (`salary`) pracownika o 10%, zanim zmieni się jego numer działu (`department_id`). *(podpowiedź: wykorzystaj operator przyporzadkowania `:NEW.costam:=:OLD.costam*costam`)*
3. Utwórz trigger, który wprowadza do tabeli `dept_changes` datę oraz informację o typie zmiany wprowadzonej na tabeli `departments` poprzez `UPDATE`, `INSERT` lub `DELETE` *(wpisując w kolumnie akcja wartość `upd`, `ins` lub `del`)*.
4. [\*] Utwórz trigger, który loguje w tabeli `emp_count` liczbę wszystkich pracowników oraz datę w momencie każdej zmiany tej ilości. *(podpowiedź: przechowaj ilość pracowników w zmiennej poprzez użycie składni `SELECT` coś `INTO` zmienna `FROM` .. w bloku `BEGIN-END`) Sprawdź poprawność triggera.*
5. [\*\*] Utwórz trigger, który wpisuje w tabeli `emp_promoted` numer pracownika oraz datę, kiedy staje się on kierownikiem. *(podpowiedź: stwórz tabelę będącą kopią tabeli `employees` przez `CREATE TABLE AS SELECT ...``, aby obejść błąd 'table is mutating')*


UWAGA ! W RAZIE BŁĘDU TYPU "trigger 'nazwa' is invalid and failed re-validation", USUŃ TRIGGER LUB UTWÓRZ GO PONOWNIE PRZEZ 'CREATE OR REPLACE TRIGGER'.

## Przykłady

**Trigger** — obiekt w bazie danych równorzędny z tabelami, widokami itd. uruchamiany przed lub po wykonaniu pewnej akcji na tabeli, wykonuje szereg instrukcji lub zapytań, napisanych w języku `PL/SQL`.


### Schemat ogólny tworzenia/modyfikowania triggerów (kwadratowe nawiasy zawierają polecenia opcjonalne):

> CREATE OR REPLACE TRIGGER nazwa_triggera BEFORE/AFTER akcja [OF kolumna] ON tabela [FOR EACH ROW] [WHEN warunek]<br>
[DECLARE
zmienne]<br>
BEGIN<br>
lista instrukcji<br>
END;

### Parametry wykorzystywane przy triggerach:

- Definicja kiedy ma się trigger wykonać:
- `CREATE OR REPLACE TRIGGER` - instrukcja tworząca lub modyfikująca istniejący trigger
- `AFTER` - wywołuje trigger po danej akcji,
- `BEFORE` - przed nią,
- akcja - akcją może być `INSERT`, `UPDATE` lub `DELETE` (bez podania kolumny w bloku [PF kolumna], dotyczy każdej kolumny w tabeli),
- `FOR EACH ROW` (opcjonalne) - wywołuje trigger dla każdego wiersza, którego dotyczy dana akcja (UWAGA! wielokrotne wywołanie triggera!), umożliwia dostęp do konkretnych wartości (tzw. "row level trigger"), bez tego fragmentu mamy do czynienia z tzw. "table level trigger"

### *Ciało* triggera:

- `DECLARE` (opcjonalne) - blok deklaracji zmiennych (nazwa + typ + średnik), zawsze występuje przed blokiem `BEGIN` - `END`,
- Operatory `OLD` oraz `NEW` - wymagane przy odwoływaniu się do wartości w kolumnach, wskazują na starą i nową wartość (UWAGA! pomiędzy BEGIN a END w formie ":NEW" oraz ":OLD"!)
- `SYSDATE` - zwraca aktualną datę w systemie.
- `IF (warunek) THEN (akcja) END IF;` - standardowy warunek `IF`

### PRZYKŁADY WYKORZYSTANIA TRIGGERÓW:

#### Stwórz trigger, który zamienia nazwisko i email pracownika, którego wypłata uległa zmianie

> CREATE OR REPLACE TRIGGER change_name BEFORE UPDATE OF salary ON employees FOR EACH ROW<br>
BEGIN<br>
:NEW.last_name:=:OLD.email;<br>
:NEW.email:=:OLD.last_name;<br>
END;<br>

Sprawdzamy trigger - np. wprowadzamy zmianę w płacy pracownika o nr 122:

> UPDATE employees SET salary = 1234 WHERE employee_id = 122;

i sprawdzamy efekt:

> SELECT last_name, email FROM employees WHERE employee_id = 122;

#### Stwórz trigger oraz tabelę, w której przechowywane będą wszelkie zmiany wartości wypłaty (stara i nowa wartość), id pracownika oraz czas, w który zostały wprowadzone:

Tworzymy tabelę, w której przechowywane bedą wszelkie zmiany wypłat (stara i nowa wartość), id pracownika oraz czas, w który zostały wprowadzone:

> CREATE TABLE emp_audit ( emp_id NUMBER(6), up_date DATE, old_sal NUMBER(8,2), new_sal NUMBER(8,2) );

Następnie, tworzymy trigger (polecenie CREATE OR REPLACE tworzy lub zastępuje obiekt istniejący już, o tej samej nazwie - oprócz tabel):

> CREATE OR REPLACE TRIGGER audit_sal AFTER UPDATE OF salary ON employees FOR EACH ROW<br>
BEGIN<br>
INSERT INTO emp_audit VALUES( :OLD.employee_id, SYSDATE, :OLD.salary, :NEW.salary );<br>
END;

Sprawdzamy trigger - np. wprowadzamy zmianę w płacach wszystkich pracowników, których kierownik ma nr 122:

> UPDATE employees SET salary = salary * 1.01 WHERE manager_id = 122;

i sprawdzamy naszą tabelę:

> SELECT * FROM emp_audit;

#### Stwórz trigger, który wpisuje do tabeli emp_updt_log datę oraz typ zmiany wprowadzonej na tabeli employees (UWAGA! Bez FOR EACH ROW!):

Tworzymy tabelę, w której przechowywane będą logi:

> CREATE TABLE emp_updt_log ( log_date DATE, action VARCHAR2(50) );

Tworzymy trigger, deklarując zmienną "akcja", która przechowuje typ naszej akcji, po czym wpisujemy jej zawartość do kolumny action w tabeli emp_updt_log:

> CREATE OR REPLACE TRIGGER log_emp_updt AFTER UPDATE OR INSERT ON employees<br>
DECLARE<br>
akcja VARCHAR2(50);<br>
BEGIN<br>
IF UPDATING THEN<br>
akcja:='Wiersz w tabeli employees aktualizowany';<br>
END IF;<br>
IF INSERTING THEN<br>
akcja:='Wiersz dodany do tabeli employees';<br>
END IF;<br>
INSERT INTO emp_updt_log VALUES( SYSDATE, akcja );<br>
END;

Sprawdzamy trigger - poprzez modyfikację wiersza w tabeli employees:

> UPDATE employees SET salary=1212 WHERE employee_id=122;

i wyświetlamy naszą tabelę logującą:

> SELECT * FROM emp_updt_log;



### Usuwanie triggerów:

> DROP TRIGGER nazwa_triggera;
