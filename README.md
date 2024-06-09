# isi-lab

###  1. Git

* Utwórz nową gałąź (np. nowa) z bieżącej (najczęściej będzie to main), stwórz w nowej gałęzi nowy plik i zmerguj nową gałąź do bieżącej.
* Pokaż jak działa pull request na jednym ze swoich repozytoriów.
* Omów różnicę między git fetch a git pull na przykładzie swojego repozytorium.
* Pokaż działanie git stash.
* Omów działanie git rebase i wskaż różnice w stosunku do git merge (mile widziany rysunek).
  
### 2. Bazy danych

* Za pomocą skryptu w wybranym języku dodaj kolejny rekord do wskazanej bazy danych.
* Dla wybranej bazy danych pokaż działanie co najmniej trzech różnych typów JOIN'a.
* Zaloguj się do bazy danych PostgreSQL w kontenerze Dockerowym i wykonaj operację SELECT dla dowolnej tabeli.
* Wskaż różnice między SQLite a PostgreSQL na wybranym przez siebie przykładzie.
* Przygotuj zapytania zawierające polecenia WHERE, LIKE, COUNT, GROUP BY, HAVING i bądz gotowy do ich uruchomienia i modyfikacji.
  
### 3. Aplikacja wg wzorca projektowego MVC (Model-View-Controller)

##### 1. Czym jest ORM, zaprezentuj praktycznie na przykładzie własnego projektu.
ORM (Object-Relational Mapping) to technika programowania, która umożliwia mapowanie obiektów w kodzie na rekordy w bazie danych. W praktyce oznacza to, że zamiast korzystać bezpośrednio z języka SQL do operacji na bazie danych, możemy używać obiektów w naszym kodzie, a ORM zajmie się tłumaczeniem tych operacji na odpowiednie zapytania SQL.

w moim projekcie mam plik ```models.py```, który zawiera klasę USER. Jest to model SQLAlchemy który mapuje na tabelę user w bazie danych. Każda kolumna w tabeli jest reprezentowana przez atrybut klasy.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/30921cda-fd5e-42e9-aab9-ca1f3c22efb4)


W pliku ```app.py``` inizjalizuje aplikację flask i tworzę tabele na podstawie zdefiniowanego modelu - robi to funkcja ```db.create_all()``` 

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/b5963dfc-a06a-4b40-8639-412c97fd43f3)

ORM umożliwia łatwe wykonywanie operacji CRUD bez potrzeby pisania skomplikowanych zapytań SQL. Zamiast tego, operujemy na obiektach Pythona. Dzieje się to w pliku ```services.py```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/73e291eb-514e-41da-80e3-c7df748c0044)

W pliku ```views.py``` wykorzystujemy powyższe funkcje i tworzymy widoki 
![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/304561ff-25e0-4ef4-86d9-753727a3a5fd)

##### 2. Czym jest wzorzec MVC? Wskaż w kodzie aplikacji poszczególne elementy tego wzorca i określ ich role.
MVC (Model-View-Controller) to wzorzec architektoniczny, który dzieli aplikację na trzy główne komponenty: Model, Widok (View) i Kontroler (Controller). Każdy z tych komponentów ma swoją własną odpowiedzialność, co umożliwia lepszą organizację kodu, jego modularność i łatwość utrzymania.

Model:
Reprezentuje dane aplikacji oraz logikę biznesową.
Odpowiada za bezpośrednią interakcję z bazą danych.
Przykład: Klasa User w pliku models.py.

Widok (View):
Odpowiada za prezentację danych użytkownikowi.
Generuje interfejs użytkownika na podstawie danych dostarczonych przez kontroler.
Przykład: Szablony HTML w katalogu templates.

Kontroler (Controller):
Odpowiada za przetwarzanie żądań od użytkownika, interakcję z modelem oraz zwracanie odpowiednich widoków.
Koordynuje przepływ danych pomiędzy modelem a widokiem.
Przykład: Funkcje widoków w pliku views.py. (@app.route)

##### 3. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawiło się Twoje imię i nazwisko.

w pliku ```views.py``` korzystam z @app.route i określam pod jakim URL (/name) ma się znajdować wskazana informacja. Następnie definiuje funkcje która zwraca moje imię i nazwisko.

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/10471b5c-8c20-44e2-89da-f3c620f7fd92)


##### 4. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawił się formularz, który pozwala dodać dwie liczby.

w pliku ```views.py``` korzystam z @app.route i określam pod jakim URL (/add_numbers) ma się znajdować formularz dodający dwie liczby. Tworzę funkcje dodająca te dwie liczby zwracam wynik a wszystko prezentuje sie w template ```form.html```

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/0804c8ea-00fa-435c-8fde-f09043f95b3b)

![image](https://github.com/patrycjaprzybysz/isi-lab/assets/100605325/fefcc189-c47f-4286-be8c-6c2d72d8ff0a)


##### 5. Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce wyświetliły się cztery zdjęcia ze strony https://jsonplaceholder.typicode.com/photos.

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
  
### Programowanie

* Przygotuj klasę Kalkulator z czterema wybranymi działaniami matematycznymi (jako metody) i bądź gotowy do utworenia obiektów i modyfikacji tejże klasy wg wytycznych.
* Napisz skrypt, pobierajacy dane w formacie JSON ze wskazanego API (online, np. https://jsonplaceholder.typicode.com/photos) i zapisz te dane do pliku tekstowego.
* Napisz skrypt, który odczytuje dane z pliku tekstowego i wyświetla je we wskazanej postaci.
* Pokaż działanie dziedziczenia w programowaniu obiektowym, poprzez utworzenie klasy Person (jej atrybuty to name i surname) i klas z niej dziedziczących, które mają dodatkowe atrybuty i metody. Może to być np. kod/program dotyczący osób na uczelni lub osób w firmie z określoną hierarchią.
