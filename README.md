# isi-lab

###  Git

* Utwórz nową gałąź (np. nowa) z bieżącej (najczęściej będzie to main), stwórz w nowej gałęzi nowy plik i zmerguj nową gałąź do bieżącej.
* Pokaż jak działa pull request na jednym ze swoich repozytoriów.
* Omów różnicę między git fetch a git pull na przykładzie swojego repozytorium.
* Pokaż działanie git stash.
* Omów działanie git rebase i wskaż różnice w stosunku do git merge (mile widziany rysunek).
  
### Bazy danych

* Za pomocą skryptu w wybranym języku dodaj kolejny rekord do wskazanej bazy danych.
* Dla wybranej bazy danych pokaż działanie co najmniej trzech różnych typów JOIN'a.
* Zaloguj się do bazy danych PostgreSQL w kontenerze Dockerowym i wykonaj operację SELECT dla dowolnej tabeli.
* Wskaż różnice między SQLite a PostgreSQL na wybranym przez siebie przykładzie.
* Przygotuj zapytania zawierające polecenia WHERE, LIKE, COUNT, GROUP BY, HAVING i bądz gotowy do ich uruchomienia i modyfikacji.
  
### Aplikacja wg wzorca projektowego MVC (Model-View-Controller)

* Czym jest ORM, zaprezentuj praktycznie na przykładzie własnego projektu.
* Czym jest wzorzec MVC? Wskaż w kodzie aplikacji poszczególne elementy tego wzorca i określ ich role.
* Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawiło się Twoje imię i nazwisko.
* Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce pojawił się formularz, który pozwala dodać dwie liczby.
* Dodaj nowy URL w aplikacji i spraw, aby po uruchomieniu go w przeglądarce wyświetliły się cztery zdjęcia ze strony https://jsonplaceholder.typicode.com/photos.
  
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
