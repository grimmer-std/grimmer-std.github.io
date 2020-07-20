---
layout: post
title: "#1 PicoCTF2019 - kategoria umiejętności ogólne"
date: 2020-06-30
tags: picoCTF writeup
description: Drugi wpis z rozwiązaniami zadań.
image: assets/images/PicoCTF2019_ogolna/ogolna.png
toc: true
---

![img]({{ 'assets/images/PicoCTF2019_ogolna/ogolna.png' | relative_url }}){: .center-image }*Cóż kryje się za tymi tajemniczymi drzwiami?*

## Kategoria umiejętności ogólne - czyli co?
W tej kategorii może znaleźć się... wszystko, w tym: zadania związane z obsługą terminalu, wyczytywanie flagi ukrytej bezpośrednio w kodzie, przeglądanie metadanych itd. itd. Zazwyczaj w tej kategorii ląduje wszystko to, czego nie da się w łatwy sposób dopasować do innych kategorii.

## Resources

![img]({{ 'assets/images/PicoCTF2019_ogolna/resources.png' | relative_url }}){: .center-image }*Klik, klik*

Szczerze polecam zapisać w zakładkach podlinkowaną stronę i wracać do niej za każdym razem, gdy postanowisz zmienić kategorię zadań. [Ten dokument](https://picoctf.com/learning_guides/Book-1-General-Skills.pdf) zawiera informacje przydatne do rozwiązania zadań z tej kategorii.

<details>
  <summary>Rozwiązanie</summary>
  
  Tę flagę bez problemu odnajdziesz bez mojej pomocy.
</details>

## 2Warm

![img]({{ 'assets/images/PicoCTF2019_ogolna/2warm.png' | relative_url }}){: .center-image }*Ciepło, coraz cieplej*

Ponownie mamy do czynienia z zamianą liczby zapisanej w danym systemie (w tym przypadku: dziesiętnym) na drugi (binarny) jednak tym razem wykorzystajmy dostęp do powłoki, jaki otrzymaliśmy przy założeniu konta. Wystarczy, że klikniemy na "Shell" w grze lub na pasku nawigacji.
### Powłoka? Po co i dlaczego.
W dużym skrócie powłoka systemowa to interfejs umożliwiający komunikowanie się z systemem operacyjnym. Zazwyczaj mówimy o powłokach tekstowych (CLI) mimo tego, że interfejsy graficzne (GUI) również zaliczają się do tego zagadnienia. Dla pełnej przejrzystości w tym oraz kolejnych wpisach będę korzystał ze słowa "shell" na określenie powłoki tekstowej. A po co z niej korzystać przy CTFach? Ponieważ wiele przydatnych narzędzi jest dostępnych tylko przy użyciu shella, albo wprost musimy skorzystać z jednej z komend shella by w ogóle mieć dostęp do danego zadania. Czasem by móc odczytać flagę musimy uruchomić shell poprzez eksploitacje danej aplikacji na serwerze.

PicoCTF w momencie pisania tych słów daje nam dostęp do Ubuntu 18.04.4 z zainstalowanym bashem w wersji 4.4.20 oraz mnóstwem przydatnych programów.s

### Prosty przykład wykorzystania interpretera języka Python przy rozwiązaniu zadania

Po zalogowaniu się na nasze konto shellowe możemy wywołać tryb interaktywny poprzez wpisanie "python3" w terminalu. Po tej komendzie wszystko, co wpiszemy zostanie zinterpretowane jako kod języka Python a by opuścić ten tryb wystarczy wpisać <code>exit()</code>. "Po co to wszystko?". Python posiada wiele przydatnych funkcji, takich jak zamiana jednego formatu zapisu na inny np. <code> bin(nasza_liczba)</code> wyświetli naszą liczbę w postaci binarnej.

<details>
  <summary>Rozwiązanie wraz z flagą</summary>
{% highlight Python %}
shell -> Python3 -> bin(42)
picoCTF{101010} {% endhighlight %}
<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/2warm_solved.png" />
<em>Kolejne 50 punktów</em>
</details>

## Bases

![img]({{ 'assets/images/PicoCTF2019_ogolna/bases.png' | relative_url }}){: .center-image }*Znaki Watsonie, co one znaczą?*

Z treści zadania wynika wprost, że ponownie mamy zapisaną jakąś wartość w jakiejś podstawie (poprzednimi razem odczytywaliśmy wartość szesnastkową a ostatnio sami zapisywaliśmy coś do postaci o bazie binarnej). Po szybkim odpytaniu wujka google o to, jakie jeszcze podstawy są wykorzystywane trafiamy na:
1. Base2 - znany nam już zapis binarny (wykorzystywane symbole do zapisu to 0 oraz 1)
2. Base8 - zapis ósemkowy, znany również jako zapis oktalny (0-7)
3. Base16 - zapis szesnastkowy, znany również jako zapis heksadecymalny (0-9 A-F)
4. Base32 - (2-7 A-Z)
5. Base64 - (0-9 a-z A-Z + -)

Patrząc po wykorzystywanych symbolach do zapisu, możemy dojść do wniosku, że w tym przypadku mamy do czynienia z ciągiem znaków zapisanych w base64.
By odkodować tę wartość posłużymy się ponownie shellem, tym razem wykorzystując polecenie <code>base64</code>.

### "Skąd mam wiedzieć jak działa dana komenda/program?"
Większość programów posiada solidnie napisany podręcznik/manual który można wywołać poleceniem <code>man nazwa_komendy</code>. Przykładowo <code>man base64</code> wyświetli nam informację o tym do czego służy ta komenda oraz jak z niej skorzystać.

### O wejściu oraz wyjściu słów kilka
Polecenie base64 pobiera dane z pliku przekazanego jako parametr (np. <code>base64 plik.txt</code>) lub jeśli nie podaliśmy żadnego pliku jako parametr - z wejścia standardowego (czyli to, co wpisujemy do terminala). Oznacza to, że po wpisaniu samego <code>base64</code> program będzie czekał na dane wprowadzone przez nas do momentu aż nie napotka na znak końca pliku (EOF - End of file) a następnie wyświetli nam **zapisaną** wiadomość w formacie base64. Możemy wpisać ten znak przy użyciu kombinacji <code>CTRL + D</code>. Aby zdekodować wiadomość musimy użyć opcji -d (decode).

Więcej o wejściu i wyjściu standardowym znajduję się [pod tym linkiem.](https://pl.wikipedia.org/wiki/Standardowe_strumienie)
<details>
  <summary>Rozwiązanie</summary>
  1. Z wykorzystaniem wejścia standardowego:
{% highlight bash %}
base64
bDNhcm5fdGgzX3IwcDM1 CTRL + D
{% endhighlight %}
  2. Z wykorzystaniem potoków: 
{% highlight bash %}
echo "bDNhcm5fdGgzX3IwcDM1" | base64 -d
{% endhighlight %}
  3. Przy użyciu przekierowania:
{% highlight bash %}
base64 -d <<<bDNhcm5fdGgzX3IwcDM1
{% endhighlight %}
"<<<" oznacza "weź daną wartość dokładnie taką, jaka jest i wyślij na wejście do komendy po lewej"<br>
  4. Z wykorzystaniem pliku: 
{% highlight bash %}
cat > plik.txt bDNhcm5fdGgzX3IwcDM1
base64 -d plik.txt
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/bases_solved.png" />
<em>Cyk, kolejne 100 punktów</em>
{% highlight bash %}
Flaga: picoCTF{l3arn_th3_r0p35}
{% endhighlight %}
</details>
<br>
## First Grep

![img]({{ 'assets/images/PicoCTF2019_ogolna/first_grep.png' | relative_url }}){: .center-image }*Grep? Czym jest grep? Dokąd zmierzamy?*

W podpowiedzi podlinkowany jest świetny tutorial do grepa, narzędzia wykorzystywanego do np. przeszukiwania plików w celu znalezienia linijki tekstu pasującej do podanego wzoru. Wzorem może być tak dany ciąg znaków (np. picoCTF) jak i znacznie bardziej skomplikowane dopasowanie (np. "znajdź mi wszystkie siedmioliterowe słowa zaczynające się na literę A a kończące się na j a w których **nie występuje** litera k"). Wzory takie nazywamy **wyrażeniami regularnymi.**

>Składnia grepa
{:.filename}
{% highlight bash %}
grep [opcje] wzór [plik, który przeszukujemy]
{% endhighlight %}

Części zawarte w nawiasach kwadratowych są opcjonalne - gdy nie podamy ścieżki do pliku grep zacznie przeszukiwać wejście standardowe.

<details>
  <summary>Rozwiązanie</summary>

{% highlight bash %}
cd /problems/first-grep_4_6b0be85ad029e98c683231bdafec396c
grep picoCTF file
{% endhighlight %}
{% highlight bash %}
Flaga: picoCTF{grep_is_good_to_find_things_ad4e9645}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/first_grep_solved.png">
<em>Greptulacje!</em>
</details>

## First Grep: Part II

![img]({{ 'assets/images/PicoCTF2019_ogolna/first_grep_part2.png' | relative_url }}){: .center-image }*Sequele zawsze są ciekawsze ;)*

Do przeszukania mamy sporą strukturę folderów - na tyle dużą, by przeszukiwanie ręczne przypominało szukanie igły w stogu siana.
Na szczęście grep z odpowiednią opcją może wykonać tę pracę za nas.

<details>
  <summary>Rozwiązanie</summary>
{% highlight bash %}
grep -r picoCTF
{% endhighlight %}
{% highlight bash %}
Flaga: picoCTF{grep_r_to_find_this_ee829ae6}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/first_grep_part2_solved.png">
<em>Grep -r FTW!</em>
</details>

## strings it

![img]({{ 'assets/images/PicoCTF2019_ogolna/strings_it.png' | relative_url }}){: .center-image }*Co może pójść nie tak z ciągiem znaków?*

Brzmi prosto: logujemy się na nasze konto shellowe, sprawdzamy co jest w środku, znajdujemy flagę i po problemie. Użyjemy tutaj polecenia <code>less</code>. W uproszczeniu less działa jak cat, tylko zamiast wyświetlać treść bezpośrednio na terminal, tworzy "osobne okno", które znika po przejrzeniu pliku. Przydatne do przeglądu dużych plików. Plik możemy przewijać strzałkami, a by wrócić do shella wystarczy wpisać "q" i nacisnąć enter.
Szybkie <code>less strings</code> ( ) zwraca nam:

![img]({{ 'assets/images/PicoCTF2019_ogolna/strings_it_less.png' | relative_url }}){: .center-image }*niezłą sieczkę*

Widzimy tutaj podpowiedź, by skupić się na poleceniu **"strings"**.

### Strings

Po przeczytaniu manuala do polecenia strings dowiadujemy się, że strings znajduje oraz wyświetla "drukowalne" ciągi znaków np. "abcd", "1323efsd3rer" itd.
Niestety samo użycie <code> strings -n 9 strings </code> (opcja -n wyświetla tylko ciągi znaków dłuższe od podanej wartości) zwraca bardzo dużo wyników. Gdybyśmy tylko mogli użyć grepa do znalezienia flagi...

### Potoki

I tutaj wchodzą potoki (pipelines), całe na biało. W skrócie - możemy przekazać dane wyjściowe z jednego polecenia jako dane wejściowe do innego. Przykładowo mamy folder o nazwie "Zdjecia", który zawiera mnóstwo innych folderów w środku, ale chcemy wyświetlić tylko te, które w nazwie mają "wakacje". Możemy to zrobić w ten sposób:
>znajdź foldery
{:.filename}
{% highlight bash %}
ls | grep wakacje
{% endhighlight %}

1. **ls** - zwróć zawartość folderu, w którym jesteś
2. **\|** - przekaż to, co otrzymałeś na wyjściu komendy po lewej na wejście komendy po prawej
3. **grep wakacje** - znajdź i wyświetl tylko te ciągi znaków (w naszym przypadku - nazwy folderów) zawierające w nazwie "wakacje"

Nic nie stoi na przeszkodzie by w ten sam sposób przefiltrować to, co zwraca nam <code>strings</code>.

<details>
  <summary>Rozwiązanie</summary>

{% highlight bash %}
strings strings | grep picoCTF
{% endhighlight %}
{% highlight bash %}
Flaga: picoCTF{5tRIng5_1T_c611cac7}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/strings_it_solved.png">
<em>Ze znajomością potoków, przychodzi wielka odpowiedzialność</em>
</details>

## what's a net cat?

![img]({{ 'assets/images/PicoCTF2019_ogolna/whats_a_net_cat.png' | relative_url }}){: .center-image }*Dude, where is my netcat?*

Proste zadanie, polegające na połączeniu się przy użyciu <code>nc</code> do danego serwera. Polecam zapoznać się z manualem <code>nc</code> ponieważ wielokrotnie będziemy korzystać z tej komendy.

<details>
  <summary>Rozwiązanie</summary>
  
{% highlight bash %}
nc 2019shell1.picoctf.com 4158 
{% endhighlight %}
{% highlight bash %}
picoCTF{nEtCat_Mast3ry_700da9c7}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/whats_a_net_cat_solved.png">
<em>Pierwsze połączenie netcatem</em>
</details>

## plumbing

![img]({{ 'assets/images/PicoCTF2019_ogolna/plumbing.png' | relative_url }}){: .center-image }*Pisałem, że netcat się przyda*

Po połączeniu przy użyciu <code>nc 2019shell1.picoctf.com 21957</code> otrzymujemy dane, w których gdzieś zapisana jest flaga. Podobnie jak w **bases** możemy te dane przefiltrować przy użyciu grepa.

<details>
  <summary>Rozwiązanie</summary>

{% highlight bash %}
nc 2019shell1.picoctf.com 21957 | grep picoCTF
{% endhighlight %}
{% highlight bash %}
Flaga: picoCTF{digital_plumb3r_c1082838}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/plumbing_solved.png">
<em>Grep2win</em>
</details>

## Based

![img]({{ 'assets/images/PicoCTF2019_ogolna/based.png' | relative_url }}){: .center-image }*Oho, 1337, zaczyna się robić poważnie*

Po połączeniu się otrzymujemy informację, że mamy tylko 45 sekund na wprowadzenie odpowiedzi. Zadania polegają na przekonwertowaniu np. zapisu binarnego na ASCII.
Mamy tutaj dwie opcje - albo próbujemy zrobić wszystko ręcznie (życzę powodzenia mając tylko 45 sekund) albo skorzystamy z języka, który ma wbudowane metody zamiany zapisu (np. Python).
Jako, że jestem leniwy wybrałem opcję numer dwa ;).

<details>
  <summary>Rozwiązanie</summary>

Przy rozwiązaniach skorzystałem z modułu <code>sys</code>, dzięki czemu w łatwy sposób mogę przekazać dane jako parametry. Parametry przekazane są jako tablica o nazwie sys.argv, i liczone są od zera. Jako sys.argv[0] podana jest nazwa uruchamianego skryptu, stąd zaczynam od sys.argv[1]. Parametry przekazywane są jako tekst, więc zamieniamy je na liczbę przy użyciu int() a później wyświetlamy znak o podanej wartości. 
  1. Pierwsze
{% highlight Python %}
import sys

for arg in sys.argv[1:]:
  print chr(int(arg, 2))
{% endhighlight %}

  2. Drugie
{% highlight Python %}
import sys

  for arg in sys.argv[1:]:
print chr(int(arg, 8))
{% endhighlight %}
  3. Trzecie
{% highlight Python %}
import sys

for arg in sys.argv[1:]:
  print arg.decode("hex") #zamiast decode możemy użyć takiego samego podejścia jak przy poprzednich rozwiązaniach
{% endhighlight %}

{% highlight bash %}
Flaga: picoCTF{learning_about_converting_values_502ff297}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/based_solved.png">
<em>Are we L33T enough? ;)</em>
</details>

## whats-the-difference

![img]({{ 'assets/images/PicoCTF2019_ogolna/whats_the_difference.png' | relative_url }}){: .center-image }*Kotki, koteczki :3*
Tym razem otrzymujemy dwa obrazki ze słodkimi kotkami - po pobraniu na pierwszy rzut oka widać, że obrazek o nazwie "cattos.jpg" jest modyfikacją pliku "kitters.jpg".
W podpowiedziach otrzymujemy informację, że "zrzucenie" danych z hexedytora może pomóc przy porównywaniu plików oraz pytanie o to jak porównuje się pliki. Zacznijmy od zapisania danych w przydatnym dla nas formacie. Skorzystamy przy tym z narzędzia <code>hexdump</code>.

<code>hexdump</code> służy do "zrzucenia" danych w interesującym nas formacie. Szczególnie przydatna dla nas będzie opcja <code>-C</code> która zapisuje dane w postaci heksadecymalnej jednocześnie zapisując wartość tych danych jako znaki ASCII umieszczone na końcu linii. By zapisać dane do pliku skorzystamy z przekierowania.

<details>
  <summary>Zrzut danych</summary>
  Zakładając, że korzystamy z konta shellowego dostarczonego przez PicoCTF
{% highlight bash %}
hexdump -C /problems/whats-the-difference_0_00862749a2aeb45993f36cc9cf98a47a/kitters.jpg > kitters.hex 
hexdump -C /problems/whats-the-difference_0_00862749a2aeb45993f36cc9cf98a47a/cattos.jpg > cattos.hex 
{% endhighlight %}

</details>

Jak znaleźć różnicę? Możemy skorzystać np. z programu <code>diff</code> czy <code>git diff</code>. Oba są dostępne jeśli korzystamy z konta na serwerze. Osobiście polecam <code>git diff</code> ponieważ zwrócone dane są w przyjaźniejszym do przeczytania formacie.

<details>
  <summary>Rozwiązanie</summary>

{% highlight bash %}
git diff kitters.hex cattos.hex
{% endhighlight %}
Teraz wystarczy złożyć flagę ze znaków, które występują tylko w pliku cattos.hex

Flaga:
{% highlight bash %}
picoCTF{th3yr3_a5_d1ff3r3nt_4s_bu773r_4nd_j311y_aslkjfdsalkfslkflkjdsfdszmz10548}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/whats_the_difference_solved.png">
<em>Składanie flagi było nużące, ciekawe czy da się to zrobić w inny sposób?</em>

</details>

### Rozwiązanie przy użyciu cmp oraz awk

Możemy również porównać te pliki bezpośrednio, przy użyciu polecenia <code>cmp</code> a następnie wyświetlić to co nas interesuje przy użyciu <code>awk</code>.

<details>
  <summary>Rozwiązanie numer 2</summary>

  Zakładając, że jesteśmy w folderze z obrazkami
{% highlight bash %}
cmp -bl kitters.jpg cattos.jpg | awk '{print $5}' | tr -d "\n"
{% endhighlight %}

{% highlight none %}
cmp -bl - porównaj i "wypisz" (w naszym przypadku - przekaż do awk) informację o tym gdzie znajdują się różnice oraz jakie wartości są od siebie różne

awk '{print $5}' - wypisz (przekaż dalej) tylko piątą kolumnę z otrzymanych danych na wejściu

tr -d "\n" - usuń z otrzymanych danych znak końca linii i wyświetl na terminal
{% endhighlight %}
</details>

## where-is-the-file

![img]({{ 'assets/images/PicoCTF2019_ogolna/where_is_the_file.png' | relative_url }}){: .center-image }*pliku och pliku, gdzie jesteś?*

Banalnie proste zadanie. Podpowiedź wskazuje na <code>ls</code>, więc po szybkim rzuceniu okiem na manual możemy rozwiązać to zadanie.

<details>
  <summary>Rozwiązanie</summary>

{% highlight bash %}
ls -a
{% endhighlight %}

{% highlight bash %}
Flaga: picoCTF{w3ll_that_d1dnt_w0RK_a871629e}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/where_is_the_file_solved.png">
<em>Can't hide from me</em>
</details>

## Flagshop

![img]({{ 'assets/images/PicoCTF2019_ogolna/flag_shop.png' | relative_url }}){: .center-image }*Idziemy na zakupy*

Tym razem mamy dostęp do kodu źródłowego napisanego w C a w podpowiedzi mamy informację o tym, że bardzo duże liczby pomogą nam w rozwiązaniu tego problemu. Nawet nie znając języka C fragment `if(number_flags > 0)` mówi nam, że podanie ujemnej liczby flag do kupienia nie zadziała.
Googlując o tym, "czym" jest `int` w C dowiadujemy się, że `int` to typ zmiennej, w której zapisać można liczbę całkowitą [w systemie U2](https://pl.wikipedia.org/wiki/Kod_uzupe%C5%82nie%C5%84_do_dw%C3%B3ch). Zapisywana jest standardowo wraz **ze znakiem**.  Dodatkowo - to jak dużej wielkości jest `int` jest zależne od platformy - może być 16, 32 lub 64 bitów.

### Co się stanie, gdy podamy zbyt dużą liczbę?

Załóżmy, że nasza zmienna `A` może pomieścić maksymalnie 4 bity. Następnie:

```
A = 7 = 0111 w systemie U2
Spróbujmy teraz dodać do A wartość 1
0111 + 0001 = 1000 co w zapisie U2 oznacza wartość -8 
```
Jako, że w kodzie występuje mnożenie podanej przez nas wartości przez 900 wystarczy, że podamy liczbę na tyle dużą, że po przemnożeniu da nam wartość ujemną.

<details>
  <summary>Rozwiązanie</summary>
{% highlight bash %}
Wystarczająco duża liczba, np. 32100000
{% endhighlight %}

{% highlight bash %}
Flaga: picoCTF{m0n3y_bag5_b9f469b5}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/flag_shop_solved.png">
<em>Easy money!</em>
</details>

## mus1c
![img]({{ 'assets/images/PicoCTF2019_ogolna/mus1c.png' | relative_url }}){: .center-image }*Piosenka? Dla mnie? Owwww.*

Nie będę ściemniać - to zadanie zajęło mi znacznie dłużej niż powinno mimo solidnej podpowiedzi. Na szczęście jedno dobre zapytanie do google [znacznie rozjaśniło sytuację](https://www.google.com/search?q=rockstar+language) kierując mnie na odpowiednią [stronę](https://codewithrockstar.com/).

<details>
  <summary>Rozwiązanie</summary>

Interpreter Rockstara zwraca ciąg liczb, który po konwersji na ASCII da nam flagę.

{% highlight Python %}
rock = [114,114,114,111,99,107,110,114,110,48,49,49,51,114]
flag = ""
for e in rock:
    flag+= chr(rock[rock.index(e)])
print flag
{% endhighlight %}

{% highlight none %}
Flaga: picoCTF{rrrocknrn0113r}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/mus1c_solved.png">
<em>Ezoteryczny Rockstar</em>
</details>

## 1_wanna_b3_a_r0ck5tar

![img]({{ 'assets/images/PicoCTF2019_ogolna/1_wanna_b3_a_r0ck5tar.png' | relative_url }}){: .center-image }*Are you ready to rock?*

Kolejna piosenka, kolejne spotkanie z Rockstarem. Tym razem po analizie tekstu naszej piosenki (wraz z pomocą [dokumentacji](https://codewithrockstar.com/docs)) możemy zauważyć, że są oczekiwane od nas dane, przykładowo:
>Zapis do zmiennej
{:.filename}
{% highlight rockstar %}
Listen to the music         = Zapisz dane z wejścia w zmiennej music             
If the music is a guitar    = Sprawdzenie, czy zmienna music ma taką samą wartość jak zmienna guitar
Say "Keep on rocking!"      = Jeżeli tak, to wypisz na wyjście "Keep on rocking!"
{% endhighlight %}

Zadanie to możemy rozwiązać na dwa sposoby: albo podając odpowiednie dane na wejście albo poprzez modyfikację kodu w taki sposób, by ominąć wszystkie "pytania".

<details>
  <summary>Rozwiązanie</summary>

{% highlight rockstar %}
Wystarczy podać jako input liczby 10 oraz 170 w dwóch liniach.
{% endhighlight %}

{% highlight none %}
Flaga: picoCTF{BONJOVI}
{% endhighlight %}

<img src="https://grimmer-std.github.io//assets/images/PicoCTF2019_ogolna/1_wanna_b3_a_r0ck5tar_solved.png">
<em>"I walk these streets <br>
A loaded six-string on my back"</em>
</details>

## Bonusowe zadanie

Poruszając się wewnątrz gry po pomieszczeniu z zadaniami trafić możemy na migający kwadrat oraz prostokąt. Jeśli się do nich zbliżymy, usłyszymy powtarzany w pętli ciąg tonów.

![img]({{ 'assets/images/PicoCTF2019_ogolna/secret.png' | relative_url }}){: .center-image }*Beep, boop*

Jest to wiadomość zapisana kodem Morse'a.

<details>
  <summary>Rozwiązanie?</summary>
  
  Wystarczy nagrać ścieżkę audio przy użyciu np. <a href="https://www.audacityteam.org/download/">Audacity</a> a następnie skorzystać z dekodera online np. <a href="https://morsecode.world/international/decoder/audio-decoder-adaptive.html">tutaj</a>.

  Odszyfrowana wiadomość: P I L L A R 2 4 1 3
</details>

Uff, to by było na tyle jeśli chodzi o kategorię ogólną. Następna w kolejce jest eksploitacja plików binarnych (i nie tylko!)