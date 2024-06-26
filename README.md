# isi-lab

###  1. Git

#### 1. Utwórz nową gałąź (np. nowa) z bieżącej (najczęściej będzie to main), stwórz w nowej gałęzi nowy plik i zmerguj nową gałąź do bieżącej.

* Utwórz nową gałąź:

```git checkout -b nowa```

To polecenie tworzy nową gałąź o nazwie nowa i przełącza się na nią.

* Stwórz nowy plik i dodaj go do repozytorium:

```
echo "Zawartość nowego pliku" > nowy_plik.txt
git add nowy_plik.txt
git commit -m "Dodano nowy plik w gałęzi nowa"
```

* Przełącz się z powrotem na gałąź główną:

```git checkout main```

* Scal gałąź nowa z gałęzią main:
  
```git merge nowa```

#### 2. Pokaż jak działa pull request na jednym ze swoich repozytoriów.

wypycha zmiany z lokalngo na zdalne repo

```
git push origin nowa
```
następnie na gitcie naciskam na ```Compare & pull request```
Wypełniam szczegóły pull requesta i klikam ```Create pull request```
a potem merguje

#### 3. Omów różnicę między git fetch a git pull na przykładzie swojego repozytorium.

*  ```git fetch```
    
git fetch pobiera dane z zdalnego repozytorium, ale nie aktualizuje automatycznie lokalnych gałęzi. Pozwala to na przeglądanie zmian bez wprowadzania ich do swojego lokalnego repozytorium.

```
git log origin/main

```
*  ```git pull ```

git pull pobiera dane z zdalnego repozytorium i automatycznie łączy je z bieżącą gałęzią. Jest to połączenie git fetch i git merge.

```
git pull origin main
```

#### 4. Pokaż działanie git stash.

git stash pozwala na tymczasowe zapisanie zmian w czystym repozytorium. Jest to przydatne, gdy chcesz przełączyć się na inną gałąź bez konieczności commitowania niedokończonej pracy.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/80733c08-2169-47c8-a817-63f007382875)

* Stwórz zmiany w repozytorium:
* 
```
echo "Niezapisane zmiany" > plik.txt
git add plik.txt
```

* Użyj git stash do tymczasowego zapisania zmian:
  
```
git stash
```
* Sprawdź stan repozytorium (zmiany zostały zapisane i usunięte z bieżącej gałęzi):

```
git status
```

* Przywróć zmiany z stash:

```
git stash pop
```


#### 5. Omów działanie git rebase i wskaż różnice w stosunku do git merge (mile widziany rysunek).

```git merge```: Tworzy dodatkowy commit scalający, który łączy dwie historie.
```git rebase```: Przenosi commity z jednej gałęzi na szczyt innej, tworząc liniową historię.

Obie metody mają swoje zastosowania. merge jest bezpieczniejszy, ponieważ zachowuje pełną historię, natomiast rebase jest użyteczny, gdy chcesz utrzymać czystszą i liniową historię commitów.

A---B---C---D---E  main
     \       /
      \     /
       F---G  feature-branch


A---B---C---D---E---F'---G'  main, feature-branch
  
### 2. Bazy danych

#### 1. Za pomocą skryptu w wybranym języku dodaj kolejny rekord do wskazanej bazy danych.

```
import sqlite3

conn = sqlite3.connect(':memory:')
cursor = conn.cursor()

# tabela studenci
cursor.execute('''
    CREATE TABLE IF NOT EXISTS students (
        id INTEGER PRIMARY KEY,
        name TEXT
    )
''')

# tabela kursy
cursor.execute('''
    CREATE TABLE IF NOT EXISTS courses (
        id INTEGER PRIMARY KEY,
        student_id INTEGER,
        course_name TEXT,
        FOREIGN KEY (student_id) REFERENCES students(id)
    )
''')

def dodaj_studenta(name):
    try:
        cursor.execute('INSERT INTO students (name) VALUES (?)', (name,))
        conn.commit()
        print('Dodano nowego studenta:', name)
    except Exception as e:
        print('Błąd podczas dodawania studenta:', e)


def dodaj_kurs(student_id, course_name):
    try:
        cursor.execute('INSERT INTO courses (student_id, course_name) VALUES (?, ?)', (student_id, course_name))
        conn.commit()
        print('Dodano nowy kurs:', course_name)
    except Exception as e:
        print('Błąd podczas dodawania kursu:', e)



dodaj_studenta('Jan Nowak')
dodaj_kurs(1, 'Fizyka')


conn.close()

```


#### 2. Dla wybranej bazy danych pokaż działanie co najmniej trzech różnych typów JOIN'a.

utworzyłam tabele

```
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE
);

CREATE TABLE IF NOT EXISTS orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    product TEXT NOT NULL,
    quantity INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

umiesciałm dane do tabeli

```
INSERT INTO users (name, email) VALUES
('John Doe', 'john.doe@example.com'),
('Jane Smith', 'jane.smith@example.com'),
('Alice Johnson', 'alice.johnson@example.com');

INSERT INTO orders (user_id, product, quantity) VALUES
(1, 'Laptop', 1),
(2, 'Smartphone', 2),
(3, 'Tablet', 1),
(1, 'Headphones', 1);

```

* inner join
INNER JOIN zwraca tylko te wiersze, które mają pasujące wartości w obu tabelach.
  
```
SELECT users.name, orders.product
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

* left join
LEFT JOIN zwraca wszystkie wiersze z lewej tabeli (users), nawet jeśli nie mają pasujących rekordów w prawej tabeli (orders).

```
SELECT users.name, orders.product
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

```

* right join ale sqlite nieobsługuje right joina więc odwrociłam left joina
RIGHT JOIN zwraca wszystkie wiersze z prawej tabeli (orders), nawet jeśli nie mają pasujących rekordów w lewej tabeli (users).

```
SELECT users.name, orders.product
FROM orders
LEFT JOIN users ON users.id = orders.user_id;
```

#### 3. Zaloguj się do bazy danych PostgreSQL w kontenerze Dockerowym i wykonaj operację SELECT dla dowolnej tabeli.

```
 docker exec -it baza_postgres psql -U postgres -d postgres -c "SELECT * FROM students;"
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/e52cef08-3b1c-4ec9-b86e-4b38d5e3fd8b)

aby wstawiać rekordy do bazy np potem insert albo create

```
 docker exec -it baza_postgres psql -U postgres
```
```
CREATE TABLE students2( id SERIAL PRIMARY_KEY, student_id INT REFERENCES students2(id), course_name VARCHAR(100));
```
```
INSERT INTO students2 (name) VALUES ('Jan Kowalski');
```
#### 4. Wskaż różnice między SQLite a PostgreSQL na wybranym przez siebie przykładzie.

* PostgreSQL wymaga oddzielnego procesu serwera i konfiguracji, podczas gdy SQLite jest prostszy w użyciu i nie wymaga oddzielnego serwera ani konfiguracji.
* w SQlite nie ma RIGHT JOIN


#### 5. Przygotuj zapytania zawierające polecenia WHERE, LIKE, COUNT, GROUP BY, HAVING i bądz gotowy do ich uruchomienia i modyfikacji.

* WHERE

```
SELECT * 
FROM users
WHERE name = 'John Doe';
```

* LIKE

```
SELECT * 
FROM users 
WHERE name LIKE 'J%';
```

* COUNT

```
SELECT users.name, COUNT(orders.id) AS orders_count
FROM users
LEFT JOIN orders ON users.id = orders.user_id
GROUP BY users.id;

```

* GROUP BY
```
SELECT product, COUNT(id) AS orders_count
FROM orders
GROUP BY product;

```
* HAVING

```
SELECT product, COUNT(id) AS orders_count
FROM orders
GROUP BY product
HAVING COUNT(id) > 0;
```

### 3. Aplikacja wg wzorca projektowego MVC (Model-View-Controller)

#### 1. Czym jest ORM, zaprezentuj praktycznie na przykładzie własnego projektu.
ORM (Object-Relational Mapping) to technika programowania, która umożliwia mapowanie obiektów w kodzie na rekordy w bazie danych. W praktyce oznacza to, że zamiast korzystać bezpośrednio z języka SQL do operacji na bazie danych, możemy używać obiektów w naszym kodzie, a ORM zajmie się tłumaczeniem tych operacji na odpowiednie zapytania SQL.

w moim projekcie mam plik ```models.py```, który zawiera klasę USER. Jest to model SQLAlchemy który mapuje na tabelę user w bazie danych. Każda kolumna w tabeli jest reprezentowana przez atrybut klasy.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/30921cda-fd5e-42e9-aab9-ca1f3c22efb4)


W pliku ```app.py``` inizjalizuje aplikację flask i tworzę tabele na podstawie zdefiniowanego modelu - robi to funkcja ```db.create_all()``` 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/b5963dfc-a06a-4b40-8639-412c97fd43f3)

ORM umożliwia łatwe wykonywanie operacji CRUD bez potrzeby pisania skomplikowanych zapytań SQL. Zamiast tego, operujemy na obiektach Pythona. Dzieje się to w pliku ```services.py```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/73e291eb-514e-41da-80e3-c7df748c0044)

W pliku ```views.py``` wykorzystujemy powyższe funkcje i tworzymy widoki 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/304561ff-25e0-4ef4-86d9-753727a3a5fd)

#### 2. Czym jest wzorzec MVC? Wskaż w kodzie aplikacji poszczególne elementy tego wzorca i określ ich role.

MVC (Model-View-Controller) to wzorzec architektoniczny, który dzieli aplikację na trzy główne komponenty: Model, Widok (View) i Kontroler (Controller). Każdy z tych komponentów ma swoją własną odpowiedzialność, co umożliwia lepszą organizację kodu, jego modularność i łatwość utrzymania.

* Model:
Reprezentuje dane aplikacji oraz logikę biznesową.
Odpowiada za bezpośrednią interakcję z bazą danych.
Przykład: Klasa User w pliku models.py.

* Widok (View):
Odpowiada za prezentację danych użytkownikowi.
Generuje interfejs użytkownika na podstawie danych dostarczonych przez kontroler.
Przykład: Szablony HTML w katalogu templates.

* Kontroler (Controller):
Odpowiada za przetwarzanie żądań od użytkownika, interakcję z modelem oraz zwracanie odpowiednich widoków.
Koordynuje przepływ danych pomiędzy modelem a widokiem.
Przykład: Funkcje widoków w pliku views.py. (@app.route)

#### 3. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawiło się Twoje imię i nazwisko.

w pliku ```views.py``` korzystam z @app.route i określam pod jakim URL (/name) ma się znajdować wskazana informacja. Następnie definiuje funkcje która zwraca moje imię i nazwisko.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/10471b5c-8c20-44e2-89da-f3c620f7fd92)


#### 4. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawił się formularz, który pozwala dodać dwie liczby.

w pliku ```views.py``` korzystam z @app.route i określam pod jakim URL (/add_numbers) ma się znajdować formularz dodający dwie liczby. Tworzę funkcje dodająca te dwie liczby zwracam wynik a wszystko prezentuje sie w template ```form.html```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/0804c8ea-00fa-435c-8fde-f09043f95b3b)

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/fefcc189-c47f-4286-be8c-6c2d72d8ff0a)


#### 5. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce wyświetliły się cztery zdjęcia ze strony https://jsonplaceholder.typicode.com/photos.

w pliku ```views.py``` korzystam z @app.route i określam pod jakim URL (/photos) ma się znajdować strona która wyświetla 4 piwerwsze zdjęcia z podanej strony.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/727bd7b5-a609-4000-8be7-a89212413748)

a tutaj html do tej strony

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/872b5822-12eb-4674-85b7-18eca9d4f6cb)


  
### 4. Docker

#### 1. Utwórz plik z obrazem Dockerfile, w którym z hosta do kontenera kopiowany będzie folder code (zawiera np. jeden skrypt w języku Python 🐍) i zbuduj go:

utworzyłam skrypt w pythonie ```script.py```

utworzyłam plik ```Dockerfile``` 

```
FROM python:3.9

# katalog /app w kontenerze
WORKDIR /app

# kopiowanie zawartosci folderu pythonProject z hosta do kontenera w katalogu /app
COPY code/pythonProject/ /app

# Skrypt pythona uruchomiony
CMD ["python", "script.py"]


```

zbudowałam go

```
docker build -t my-python-app .

```
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/ed9f0d9e-1b27-4158-b1e0-e1113e283344)


uruchom ww. skrypt wewnątrz kontenera.

```
docker run my-python-app
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/66b3899c-65cf-4025-8f0b-7e5dc531c805)


#### 2. Skopiuj wybrany plik tekstowy z hosta (swojego komputera) do kontenera Dockerowego.

utworzyłam nowy kontener

```
docker run -it --name my_container python
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/59d894bc-f0a4-41ab-99ef-a7dc4a57b916)

kopiowanie pliku tekstewego do kontenera

```
docker cp C:/Users/patip/ISI-laby/docker/data/sample.txt my_container:/app
```
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/1467d894-66cd-4679-953b-894de83495cd)

#### 3. Skopiuj wybrany plik tekstowy z kontenera Dockerowego do hosta (swojego komputera).

```
docker cp my_container:/app C:/Users/patip/ISI-laby/docker/data/
```
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/a09ac02e-041b-4799-936e-eccb452edcfb)


#### 4. Pokaż użycie komend ENTRYPOINT i CMD.

utworzyłam nowy projket z ```app.py``

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/b0ad3d34-f195-41e3-b58a-87a93a478044)

utworzyłam ```cmd.Dockerfile```

```
FROM python:3.9

WORKDIR /app

COPY . /app

RUN pip install flask

CMD ["python", "app.py"]
```
 zbudowałam go

 ```
 docker build -f cmd.Dockerfile -t cmd .
```
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/dfe10360-b0b5-4f3b-b01b-22ff2adbb985)

teraz gdy chce dodac nowe polecenie do cmd np /dev to reszta polecen sie nie wykonana

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/da64daac-4972-4d41-bf3f-b39a25b5a7d2)

gdy uzyje entrypoint bede mogła dopisac polecenie

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/ac6e0192-e834-4ea1-ac14-c960d418e4f9)

```
docker build -f entrypoint.Dockerfile -t entrypoint .
```
i następnie dopisze komende ```docker run entrypoint /dev``` zostanie ona wykonana na końcu, dopisana.

#### 5. Pokaż użycie komend ADD i COPY i WORKDIR w swoim projekcie.

```add``` działa jak ```copy``` Kopiuje pliki i katalogi z lokalnego systemu plików do systemu plików obrazu Docker z tą różnica ze moze kopiować z linku i pliki tar.
```workdir``` utworzy katalog jesli nie istenije, ustawia katalog roboczy dla wszystkich kolejnych instrukcji tzn. jesli ustawimy katalog /app to wszytskie polecenia np COPY test.txt będą zapisywane w tym katalogu

```
FROM ubuntu
WORKDIR /app
COPY text.txt .
CMD pwd && ls
```

#### 6. Pokaż działanie docker compose w swoim projekcie.

podstawowy ```docker-compose```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/1ac7f821-1158-41cf-9f16-52a8f7014bd9)

aby zbudować nalezy wpisac ```docker-compose up```


#### 7. Omów na podstawie swojej aplikacji komendy docker inspect i docker logs.

```docker inspect``` zawiera wszytskie informacje o kontenerze, jak nazywa się jego sieć z jakiego portu korzysta, jaki ma obraz, czego uzywa dockerfile (cmd).
```docker logs``` służy do wyświetlania dzienników kontenera. Pozwala na przeglądanie wszystkich logów wypisywanych przez procesy wewnątrz kontenera.

#### 8. Czym są sieci w Dockerze? Zaprezentuj przykład na bazie swojego projektu.

Sieci w Dockerze służą do zarządzania łącznością między kontenerami i hostami. Umożliwiają komunikację między różnymi kontenerami oraz łączność z zasobami sieciowymi na hoście. Docker oferuje kilka rodzajów sieci, takich jak bridge, host, overlay, macvlan itp.

```docker-compose``` z siecią my_network
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/9cba261a-c5a0-465a-a6d2-99b7d970bbc3)

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/b13284df-57f3-4027-8589-310074a78478)

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/2b7e72a2-bafc-40d4-82eb-3a5224f95b61)

  
### 5. Programowanie

#### 1. Przygotuj klasę Kalkulator z czterema wybranymi działaniami matematycznymi (jako metody) i bądź gotowy do utworenia obiektów i modyfikacji tejże klasy wg wytycznych.

utworzyłam plik ``` kalkulator.py``` 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/b19f4c87-c13a-4cf3-9809-f757949cc339)

w pliku ```main.py``` odwołuje sie do tego pliku tworzac nowy obiekt ```kalkulator = Kalkulator()``` 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/f391f5ba-2a26-45bb-9b27-181370a57fca)

utworzyłam plik ```zaawansowany.py``` który dziedziczy po klasie kalkulator

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/3be9093f-59dc-43f6-b0a9-896f244459e6)

w pliku ```main.py``` tworze obiekt na jego podstawie

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/31442738-fa8e-4851-824f-f0ce307bcc8a)

tworzenie nowych obiektów wyglada tak

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/3e22f21d-c2ba-4569-8cd2-5854909ab4aa)

#### 2. Napisz skrypt, pobierajacy dane w formacie JSON ze wskazanego API (online, np. https://jsonplaceholder.typicode.com/photos) i zapisz te dane do pliku tekstowego.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/4febadac-7ed0-436b-b872-d862c1ca30c0)

zapisany plik person.json

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/0986c913-d727-41a6-ad0d-2dedb518d354)


#### 3. Napisz skrypt, który odczytuje dane z pliku tekstowego i wyświetla je we wskazanej postaci.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/2196cd6a-a177-4e3b-b9f6-b196129eb776)

#### 4. Pokaż działanie dziedziczenia w programowaniu obiektowym, poprzez utworzenie klasy Person (jej atrybuty to name i surname) i klas z niej dziedziczących, które mają dodatkowe atrybuty i metody. Może to być np. kod/program dotyczący osób na uczelni lub osób w firmie z określoną hierarchią.

klasa Person 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/81c5b9f8-776d-41be-94f1-b27b33689f4a)

klasa Student

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/24959ec5-f138-4c10-b99b-88668ca62ae1)

klasa Teacher

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/3218534d-1d24-49b6-95e4-87dd0164ce26)

plik ```main2.py``` 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/4e723dbf-03b6-49ac-9716-b9886f90a1e9)
