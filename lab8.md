---
layout: default
title: Laboratorium 8
---

**UWAGA - dzisiaj nie wysyłamy pojedynczych odpowiedzi !**

1. Wyczyść bazę danych `HR` poprzez uruchomienie skryptu [`drop_HR.sql`](drop_HR.sql)
2. Posiłkując się przykładami z poprzednich laboratoriów oraz diagramem bazy danych poniżej, stwórz trzytabelową bazę danych *(UWAGA! Nazwy kolumn nie powinny zawierać polskich znaków!)*, następnie dodaj do każdej tabeli po 3 wiersze — **uwaga, tworząc tabele i dodając wartości, zacznij od tych, które mają najmniej połączeń.**
3. Utwórz indeks do kolumny `Nazwa` w tabeli `Użytkownicy`, trzyliterowy synonim do każdej z tabel (razem trzy synonimy) oraz jedną sekwencję.
4. Wyeksportuj utworzoną bazę danych do pliku `*.sql` w narzędziu **SQL Developer** i prześlij plik. *(instrukcja w SQL Developer: wchodzimy w menu Tools > klikamy Database Export... > wybieramy nasze Connection > odznaczamy opcję Show Schema! (w sekcji Export DDL) > wybieramy gdzie chcemy zapisać nasz plik (Browse...) > zaznaczamy "Proceed to summary." > klikamy Dalej > klikamy Zakończ)*

##Diagram bazy danych

![diagram](/img/bd.JPG)

##Przykłady

### SEKWENCJE

#### Sekwencja:

- Obiekt w bazie danych równorzędny np. z tabelami i widokami, posiadający własną nazwę (sprawdź na liście rozwijanej w menu Object Browser).
- Automatycznie generuje kolejne wartości numeryczne (wykorzystywane przy dodawaniu wierszy, najczęściej dla kluczy głównych, aby nie musieć sprawdzać ostatniej wartości).

#### Parametry używane w sekwencjach:

- `INCREMENT BY` — określa wartość dodawaną do kolejnej wartości sekwencji *(domyślnie 1)*.
- `START WITH` — określa początkową wartość sekwencji *(domyślnie 1)*.
- `MAXVALUE` — określa maksymalną wartość sekwencji *(domyślnie 999999999999999999999999999)*.
- `MINVALUE` — określa minimalną wartość sekwencji *(domyślnie 1)*.
- `CYCLE` — opcja dodatkowa, włącza powtarzanie wartości od minimalnej po osiągnięciu wartości maksymalnej.
- `CACHE` — opcja dodatkowa, określa ile wartości sekwencji ma być pre-alokowanych w pamięci w celu szybszego wywoływania.

#### *Przykład* — tworzenie, modyfikacja oraz usuwanie sekwencji:

> CREATE SEQUENCE dept_deptid_seq
INCREMENT BY 10
START WITH 120
MAXVALUE 9999
NOCACHE
NOCYCLE

> ALTER SEQUENCE dept_deptid_seq
INCREMENT BY 20

> DROP SEQUENCE dept_deptid_seq

#### *Przykład* — wykorzystanie sekwencji `dept_deptid_seq` przy dodawaniu wiersza (w kluczu prywatnym):

> INSERT INTO departments(department_id,department_name, location_id)
VALUES (dept_deptid_seq.NEXTVAL,'Support', 2500)

#### *Przykład* — Wyświetlenie aktualnej wartości sekwencji `dept_deptid_seq`:

> SELECT dept_deptid_seq.CURRVAL FROM dual

Wywołania:

- `CURRVAL` - zwraca aktualną wartość sekwencji
- `NEXTVAL` - dodaje kolejną wartość do sekwencji

#### *Przykład* — Wyświetlanie wszystkich sekwencji (w kolumnie `last_number` wyświetlony jest następny wolny numer sekwencji):

> SELECT * FROM user_sequences

### INDEKSY

####Indeks:
- Obiekt w bazie danych równorzędny np. z tabelami i widokami, posiadający własną nazwę (sprawdź na liście rozwijanej w menu Object Browser)
- przyspiesza operacje wyświetlania danych z bazy (przydatne w dużych bazach danych, jak Facebook czy Google)

#### Indeks powinien być stworzony, gdy:
- w kolumnie znajduje się wiele różnych wartości
- kolumna jest często wykorzystywana w klauzuli `WHERE`
- tabela zawiera wiele rekordów i większość zapytań wywołuje mniej niż 4% wszystkich wierszy

#### *Przykład* — tworzenie oraz usuwanie indeksu na kolumnie `last_name` w tabeli `EMPLOYEES`:

> CREATE INDEX emp_last_name_idx ON employees(last_name)

> DROP INDEX emp_last_name_idx

#### *Przykład* — Wyświetlanie wszystkich indeksów (przykład dla tabeli `EMPLOYEES`):

> SELECT * FROM user_indexes

### SYNONIMY

#### Synonim:
- Obiekt w bazie danych równorzędny np. z tabelami i widokami, posiadający własną nazwę (sprawdź na liście rozwijanej w menu Object Browser)
- ułatwia dostęp do obiektów w bazie danych poprzez stworzenie opcjonalnej (np. krótszej) nazwy

#### *Przykład* — tworzenie oraz usuwanie synonimu dla widoku `dept_sum_vu`:

> CREATE SYNONYM d_sum FOR dept_sum_vu

> DROP SYNONYM d_sum
