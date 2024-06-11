# isi-lab

###  1. Git

* Utwórz nową gałąź (np. nowa) z bieżącej (najczęściej będzie to main), stwórz w nowej gałęzi nowy plik i zmerguj nową gałąź do bieżącej.
* Pokaż jak działa pull request na jednym ze swoich repozytoriów.
* Omów różnicę między git fetch a git pull na przykładzie swojego repozytorium.
* Pokaż działanie git stash.
* Omów działanie git rebase i wskaż różnice w stosunku do git merge (mile widziany rysunek).
  
### 2. Bazy danych

#### 1. Za pomocą skryptu w wybranym języku dodaj kolejny rekord do wskazanej bazy danych.

utworzyłam kontener dla bazy danych 

```
docker run --name postgres-container -e POSTGRES_PASSWORD=password -d postgres
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/ad6fc479-c1a4-4e1f-a6ff-57160e108ace)

utworzyłam baze danych

```
docker exec -it postgres-container psql -U postgres
```
```
CREATE DATABASE test_db;
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/d14a1bac-db9a-43f4-bfdf-142829e13527)

połaczyłam sie z bazą danych

```
 \c test_db
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/8f731d36-3c2a-4892-a01c-c21f16b70922)

utworzyłam tabele

```
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    order_date DATE,
    customer_id INT REFERENCES customers(customer_id)
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    price NUMERIC(10, 2)
);

CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT REFERENCES orders(order_id),
    product_id INT REFERENCES products(product_id),
    quantity INT
);

```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/ed2d7bf1-3511-4900-8720-a59a5c4bd8d8)

i wstawiłam rekordy do tabeli

```
INSERT INTO customers (name, email) VALUES
('John Doe', 'john@example.com'),
('Jane Smith', 'jane@example.com');

INSERT INTO products (product_name, price) VALUES
('Laptop', 999.99),
('Phone', 499.99);

INSERT INTO orders (order_date, customer_id) VALUES
('2023-01-01', 1),
('2023-01-02', 2);

INSERT INTO order_items (order_id, product_id, quantity) VALUES
(1, 1, 1),
(1, 2, 2),
(2, 2, 1);
```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/9ee16d4d-7f3d-477d-8d80-dd307bd37aba)



#### 2. Dla wybranej bazy danych pokaż działanie co najmniej trzech różnych typów JOIN'a.
#### 3. Zaloguj się do bazy danych PostgreSQL w kontenerze Dockerowym i wykonaj operację SELECT dla dowolnej tabeli.
#### 4. Wskaż różnice między SQLite a PostgreSQL na wybranym przez siebie przykładzie.
#### 5. Przygotuj zapytania zawierające polecenia WHERE, LIKE, COUNT, GROUP BY, HAVING i bądz gotowy do ich uruchomienia i modyfikacji.
  
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


  
### Docker

* Utwórz plik z obrazem Dockerfile, w którym z hosta do kontenera kopiowany będzie folder code (zawiera np. jeden skrypt w języku Python 🐍) i zbuduj go:
** uruchom ww. skrypt wewnątrz kontenera.
* Skopiuj wybrany plik tekstowy z hosta (swojego komputera) do kontenera Dockerowego.
* Skopiuj wybrany plik tekstowy z kontenera Dockerowego do hosta (swojego komputera).
* Pokaż użycie komend ENTRYPOINT i CMD.
* Pokaż użycie komend ADD i COPY i WORKDIR w swoim projekcie.
* Pokaż działanie docker compose w swoim projekcie.
* Omów na podstawie swojej aplikacji komendy docker inspect i docker logs.
* Czym są sieci w Dockerze? Zaprezentuj przykład na bazie swojego projektu.
  
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
