# Java-Kompendium-Wiedzy-
W tym repozytorium, dla samego siebie chciałbym zrobić kompletny zbiór wiedzy przydatnych realnie rzeczy z javy. Postaram się podsumować zagadnienia związane z wszelakim pojętym działaniem języka wraz z kodem lub bez. W skrócie? Będą tu rzeczy typu co to jest JVM, ale bez definicji tylko bardziej na logiczny rozum bez kucia na pamięć definicji!


---


# 1. JVM - Java Virtual Machine

## 1.1 Podstawa

Na starcie chciałbym określic jasno, Java Virtual Machine to aplikacja, dokładniej to aplikacja napisana w większości w języku C++ oczywiście nie zawsze w pełni (tak to ten język gdzię można używać znacznik na znacznik). JVM jest dość złożona, posiada wiele algorytmów oraz optymalizacje co oczywiście jest dość logiczne wkońcu dzięki Java Virtual Machine jesteśmy w stanie interpretować nasz kod wczęsniej skompilowany przez kompilator java ( javac ), następnie ten bytecode jest interpretowany na różnych systemach operacyjnych dzięki róznym implementacjom JVM. Ale czym tak własciwie jest bytecode? Jak można sobie go wyobraźić?

## Krótki wstęp

Wyobrażmy sobie, że mamy 100zł(skompilowany program). Załóżmy również, że każdy kraj ( system operacyjny ) na świecie ma swój kantor (JVM), w którym można wymienić złotówki na lokalną walute. Przez chwile pomyślmy o walucie jak o implementacji złotówki. W takim przypadku możemy wybrać się do każdego kraju na świecie i wymienić złotówki na lokalną walutę. W taki oto sposób w wielkim skrócie działa nasza możliwość interpretacji bytecode'u na różnych systemach operacyjnych. Zakładając, że złotówka to uniwersalna waluta możemy ją wymienić w dowolnym kantorze czyli powiedzmy zinterpetować ją gdziekolwiek gdzie kantor istnieje. Wyobrażenie jak to działa jest o wiele ważniejsze niż znanie definicji więc przejdzmy teraz do tej nudniejszej części. 

## 1.2 Bytecode
Bytecode czyli kod bajtowy, nie jest kodem maszynowym oraz nie jest kodem w pełni czytelnym dla każdego. To swojego rodzaju stan kodu pomiędzy kodem źródłowym a kodem maszynowym. Liste instrukcji bytecode'u można zobaczyć na przykład tutaj - https://en.m.wikipedia.org/wiki/List_of_Java_bytecode_instructions. Pojawia się pewna kwestia jak już wspomniałem wcześniej bytecode jest interpretowany przez rózne systemy operacyjne ale co to dokładnie oznacza? Warto wiedzieć po prostu, że na różnych systemach operacyjnych bytecode jest wykonywany przez JVM, dzięki temu oczywiście mamy tzw. "platform-independent". Warto jeszcze dodać, że bytecode możemy zdefiniować jako instrukcje dla maszyny wirtualnej (VM). Jedyna rzecz, która moim skromnym zdaniem warto wiedzieć - JVM interpretuje bytecode do kodu natywnego za pomocą JIT (Just-In-Time), dzięki temu mamy kluczową optymalizację w nowoczesnych implementacjach JVM.

## 1.3 Jak to wygląda po kompilacji kodu
A więc, mamy prosty program, standard:
public static void main.... { System.out.println("ByteCode"); } i powiedzmy, że chcemy aby ten program uruchomił się gdy wcześniej skompilujemy go w naszym środowisku np. IntellJ Idea na Windows'ie. 
W moim przypadku używam maven'a, może o tym czym jest maven później, narazie zostawmy temat.
Podstawowo tylko powiem, że w maven'ie w pliku pom.xml potrzebujemy zdefiniować sekcje build gdzie najważniejsze jest zdefiniowanie głównej klasy. I tutaj pojawia się zagadka, po co główna klasa?

### 1.3.1 Kompilacja kodu - Main class
Klasa główna z angielskiego Main class to miejsce w aplikacji gdzie powiedzmy bije serce naszego programu, to tutaj definiujemy wszystkie rzeczy które są gdzieś tam uruchamiane w aplikacji albo coś robią, wszystko w naszej metodzie głównej nazwanej main która wygląda następująco:
```java
    public static void main(String[] args) {
        //jakiś kod
    }
```
Odrazu chciałbym podkreślić, że nie będę się zagłębiał w przypadki gdzie nie trzeba mieć tej metody oraz głównej klasy np. statyczny inicjalizator, ale warto wiedzieć, że się da 😊.
Chciałbym jeszcze dopowiedzieć, że da sie skompilować osobno jedną klase używając komendy:
```bash
javac ClassName.java
```
Ale również nie chce się skupiać na takich przypadkach bo w sumie po co.
Jako ciekawostkę tak wygląda nasz mega skomplikowany kod po przeczytaniu go:

![cat .class](https://i.imgur.com/W1yYzg7.png)

A wieć wracając wykonuje polecenie "mvn clean install", oraz otrzymuje skompilowany program w postacji .jar czyli Java Archive - nasz program!
Możemy oczywiście z bliżej nieznanych powodów otworzyć go np. winrar'em aby zobaczyć ze mamy tam foldery oraz skompilowane pliki do .class zamiast .java w naszym projekcie 😵.
No i teraz finalnie możemy odpalić sobie program wpisując prostą komende:
```bash
java -jar NazwaPliku.jar
```
😎
## 1.4 Dlaczego 'java -jar plik.jar' działa?

Finalnie przechodzimy do tego co już wcześniej napisałem, kompiler przekształcił nasz kod na bytecode a następnie dzięki temu, że mamy zainstalowaną jave na komputerze możemy na swoim systemie operacyjnym zinterpretować kod od komplilatora i uruchomić program.
Program, żeby działał musi posiadać plik MANIFEST.MF w archiwum .jar, który zawiera wpis Main-Class - wskazujący na główną klase programu, bez tego JVM nie wie, która klase uruchomić (w moim przypadku wygenerowała się dzięki maven).
