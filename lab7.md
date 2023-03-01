---
layout: default
title: Laboratorium 7
---

**UWAGA - działamy na własnych kontach! Uśmiech**

Przed rozpoczęciem ćwiczeń proszę przywrócić bazę danych HR (uruchomić skrypt resetujący bazę danych)

1. Stwórz tabelę o nazwie `STUDENCI` i kolumnach:
  - `Numer_indeksu` - klucz główny, wartości numeryczne do 6-ciu cyfr
  - `Imie` - ciąg znaków o długości do 16 znaków, nie może być pusta
  - `Nazwisko` - ciąg znaków o długości do 32 znaków, nie może być pusta
  - `Data_ur` - data
  - `Srednia_ocen` - wartość numeryczna o długości 2 cyfr i dokładności do 1 miejsca po przecinku, zawierająca wartości od 2.0 do 5.5 (użyj ograniczenia`CHECK`)
  - `Plec` - ciąg znaków o stałej długości (1), dopuszczający jedynie wartości `M` lub `F`, nie może być pusta
2. Dodaj do tabeli `STUDENCI` studenta(-tkę) o Twoich danych oraz studenta(-tkę) o przeciwnej płci.
3. Zmień w tabeli `STUDENCI` kolumnę `Srednia_ocen` ustalając wartość domyślną równą `5.5`.
4. Stwórz widok o nazwie `STUD_VIEW`, zawierający średnie ocen wszystkich studentek w bazie danych `STUDENCI`. Sprawdź jego działanie. *(Odpowiedź nie musi zawierać zapytania sprawdzającego działanie.)*
5. Usuń tabelę `STUDENCI` oraz widok `STUD_VIEW` *(dwa osobne zapytania)*.
6. [\*\*]Stwórz tabelę o nazwie `BANDS` i kolumnach:
  - `band_level` - wartość numeryczna ograniczona do jednocyfrowych liczb całkowitych, będąca kluczem głównym przyjmujacym wartości od 1 do 3,
  - `value` - ciąg znaków o zmiennej długości do 8 znaków, nie może być pusta
  - `position` - ciąg znaków o stałej długości 2 znaków, nie może być pusta

  Następnie, dodaj do tabeli `BANDS` 3 wiersze (za pomocą pojedynczego zapytania), podając wartości w polach:
  - `band_level` - wartości od 1 do 3,
  - `value` - wartości `low`, `medium`, `high` (odpowiednio dla wierszy 1 - 3),
  - `position` - wartości `OP`, `TL`, `MG` (odpowiednio dla wierszy 1 - 3).

7. [\*\*] Połącz tabelę `BANDS` z tabelą `EMPLOYEES`, dodając do tabeli `EMPLOYEES` klucz obcy, czyli kolumnę o nazwie `band_level` (użyj składni: `ALTER TABLE table_name ADD column_name data_type REFERENCES table_name(column_name)`), następnie wypełnij ją wartościami wypłat pracowników w dziesiątkach tysięcy zaokrąglonych do góry, np. pracownik zarabiający 12345 otrzyma `band_level = 2`, natomiast pracownik zarabiający 7777 - `band_level = 1`. *(Podpowiedź: użyj CEIL)*
8. [\*\*] Stwórz widok o nazwie `EMP_BAND` przechowujący informacje o nazwiskach pracowników i ich `position` z tabeli `BANDS`. Sprawdź jego działanie. Następnie usuń tabelę `BANDS`. ((w odpowiedzi na to ćwiczenie wyślij jedynie kod tworzący widok `EMP_BAND`)*

##Przykłady

### CREATE TABLE - tworzy nową tabelę w bazie danych

Schemat ogólny:

> CREATE TABLE table_name
(
column_name1 DATA_TYPE OGRANICZENIA,
column_name2 DATA_TYPE OGRANICZENIA,
column_name3 DATA_TYPE OGRANICZENIA,
....
);

### Najważniejsze typy danych:

- `NUMBER(p,s)` - wartość numeryczna o ilości cyfr p i dokładności s (p - ilość wszystkich niezerowych cyfr, s - ilość cyfr po przecinku)
- `VARCHAR2(v)` - ciąg znaków o dowolnej długości (v - maksymalna ilość znaków)
- `CHAR(c)` - ciąg znaków o stałej długości (c - ilość znaków)
- `DATE` - data

### Podstawowe OGRANICZENIA danych, jakich możemy użyć do kolumn:

- `NOT NULL` - kolumna musi zawierać wartość
- `UNIQUE` - wartości w kolumnie nie mogą się powtarzać
- `PRIMARY KEY` - kolmuna not null oraz unique, klucz główny (każda tabela może mieć tylko jeden)
- `FOREIGN KEY` - kolumna wskazująca na klucz główny innej tabeli (również o takiej samej nazwie)
- `CHECK` - ogranicza wartości w kolumnie tylko do podanych w warunku
- `DEFAULT` - określa wartość domyślną, jaką będzie miała kolumna

Przykład użycia wszystkich podstawowych ograniczeń i kilku typów danych:

> CREATE TABLE Niezly_przyklad <br />
( <br />
GlownyK NUMBER(5,0) PRIMARY KEY, <br />
Niezerowa VARCHAR2(10) NOT NULL, <br />
Niepowtarzalna CHAR(2) UNIQUE, <br />
ObcyK NUMBER(3,0) REFERENCES EMPLOYEES(employee_id), <br />
Sprawdzajaca NUMBER(5,2) CHECK (Sprawdzajaca>36.6), <br />
Domyslna VARCHAR2(32) DEFAULT 'domyslna_wartosc', <br />
Mieszana NUMBER(8,5) NOT NULL UNIQUE CHECK (Mieszana>123.45678) <br />
);

Ograniczenia można również nakładać na kolumny w osobnych krokach, poprzez klauzulę `CONSTRAINT` przy tworzeniu tabeli:

> CREATE TABLE Niezly_przyklad <br />
( <br />
GlownyK NUMBER(5,0), <br />
Niezerowa VARCHAR2(10) NOT NULL, <br />
Niepowtarzalna CHAR(2), <br />
ObcyK NUMBER(3,0), <br />
Sprawdzajaca NUMBER(5,2), <br />
Domyslna VARCHAR2(32) DEFAULT 'domyslna_wartosc', <br />
Mieszana NUMBER(8,5) NOT NULL, <br />
CONSTRAINT glowny PRIMARY KEY (GlownyK), <br />
CONSTRAINT unikaty UNIQUE (Niepowtarzalna,Mieszana), <br />
CONSTRAINT obce FOREIGN KEY (ObcyK) REFERENCES EMPLOYEES(employee_id), <br />
CONSTRAINT sprawdzacz CHECK (Sprawdzajaca>36.6 AND Mieszana>123.45678) <br />
);

### ALTER TABLE - modyfikuje daną tabelę poprzez dodanie, modyfikację lub usuwanie kolumn lub ograniczeń

Dodawanie kolumny:

> ALTER TABLE table_name <br />
ADD column_name data_type OGRANICZENIA;

Modyfikowanie atrybutów kolumny:

> ALTER TABLE table_name <br />
MODIFY column_name data_type OGRANICZENIA;

Usuwanie kolumny:

> ALTER TABLE table_name <br />
DROP COLUMN column_name;

Ograniczeń nie trzeba definiować przy tworzeniu tabeli, można później dodać je poprzez klauzulę `ADD CONSTRAINT`:

> ALTER TABLE table_name <br />
ADD CONSTRAINT constraint_name CONTRAINT_TYPE (column_name);

Usuwanie ograniczeń:

> ALTER TABLE table_name <br />
DROP CONSTRAINT constraint_name;

### CREATE VIEW - tworzy widok, czyli wirtualną tabelę, która automatycznie się aktualizuje, gdy się do niej odwołujemy

Schemat ogólny:

> CREATE VIEW view_name AS <br />
SELECT column_name,... <br />
FROM table_name <br />
WHERE condition

**UWAGA - Widok może bazować na dowolnym podzapytaniu SELECT**

Przykład - tworzenie widoku:

> CREATE VIEW Widoczek AS <br />
SELECT department_id,COUNT(*) "asd" FROM employees GROUP BY (department_id);

Przykład - wykorzystanie widoku:

> SELECT * FROM Widoczek;

### DROP TABLE / DROP VIEW - usuwa tabelę / widok

Schemat ogólny:

> DROP TABLE table_name; <br />
DROP VIEW view_name;

### dodawanie kilku wierszy do w jednym poleceniu:

Schemat ogólny:

> INSERT ALL <br />
  INTO table_name (col_name1,...) VALUES (value1,...) <br />
  INTO table_name (col_name1,...) VALUES (value1,...) <br />
  INTO table_name (col_name1,...) VALUES (value1,...) <br />
SELECT * FROM dual;

(tabela 'dual' w środowisku Oracle jest pomocniczą tabelą, wykorzystywaną w wielu ciekawych sytuacjach, polecam poczytać/poszukać/Google)
