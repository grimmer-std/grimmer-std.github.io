---
layout: post
title: \#0 Wstęp do PicoCTF2019
date: 2020-06-25
tags: picoCTF writeup
description: Czas zacząć przygodę w krainie CTF!
toc: true
---

Hej Czytelniku. Witam Cię w pierwszym wpisie na temat PicoCTF. Nie przedłużając...

# Czym jest picoCTF oraz dla kogo skierowana jest ta seria wpisów?

[PicoCTF](https://picoctf.com/) to dostępny przez cały rok CTF skierowany do początkujących. Według mnie jest to najlepsze miejsce do rozpoczęcia przygody z zawodami Capture the Flag. Zawody te w dużym skrócie polegają na znalezieniu ukrytej flagi (która jest ciągiem znaków) poprzez rozwiązanie danego zadania. Rozwiązanie może polegać na złamaniu szyfru, zerknięciu do kodu czy wykorzystaniu znanej podatności. Oczywiście wszystko jest zależne od tego dla kogo (a dokładniej - dla jak bardzo doświadczonych osób) przeznaczone są dane zawody.
Czymś, co wyróżnia PicoCTF jest fakt, że oprócz listy wyzwań otrzymujemy grę opartą na silniku Unity oraz proste tło fabularne. Dodatkowo warto zaznaczyć, że każdy CTF ma swój **format flagi**. Dla PicoCTF jest to **picoCTF{treść_danej_flagi}**.

Ten oraz kolejne wpisy pisane są **celowo** z perspektywy "świeżaka" oraz będą krok po kroku opisywać ciąg myślowy od pierwszego spotkania z danym zagadnieniem do znalezienia flagi. Celem jest stworzenie samouczka pisanego jak najprostrzym językiem "od początkującego dla początkującego".

![img]({{ 'assets/images/PicoCTF2019_general/ekran_startowy.png' | relative_url }}){: .center-image }*Ekran startowy PicoCTF wraz z ciekawą "reklamą"*

# Hej przygodo! Czas na pierwsze zadanie - Lets Warm Up

Po obudzeniu się w pustej fabryce oraz oswojeniu się ze sterowaniem trafiamy na terminal wyświetlający
![img]({{ 'assets/images/PicoCTF2019_general/lets_warm_up1.png' | relative_url }}){: .center-image }*taki ekran*

<details>
  <summary>Podpowiedzi</summary>
  
  Wyślij swoją odpowiedź w naszym formacie flagi. Dla przykładu, jeśli rozwiązanie to "hello", wyślij je jako "picoCTF{hello}".
</details>


Ok, co my tutaj mamy. Jakieś dziwne "0x70" w zapisie heksadecymalnym cokolwiek to znaczy oraz coś o ASCII.

## Czym jest zapis heksadecymalny oraz ASCII?

Szybkie zapytanie do Google mówi nam, że zapis ten nazywany jest również zapisem szesnastkowym tzn. że zamiast znanej nam podstawy dziesiętnej (0123456789) korzysta z 16 znaków (0123456789ABCDF). Oznacza to, że np. 0xA w zapisie szesnastkowym to 15 w dziesiętnym. A skąd prefiks "0x"? Cóż, kiedyś się tak przyjęło w języku programowania C i już zostało.

Natomiast ASCII to metoda zakodowania znaków jako liczby. Niestety komputery "są głupie" i potrafią zrozumieć tylko liczby - dlatego musiał powstać jakiś standard zapisu znaków. I tak w dużym skrócie powstało ASCII. Na szczęście nie musimy znać na pamięć jaka liczba odpowiada za dany znak bo powstało [bardzo](https://www.ascii-code.com/) [dużo](https://www.rapidtables.com/code/text/ascii-table.html) [tabel](http://www.asciitable.com/) ułatwiających nam to zadanie.

Uzbrojeni w nową dawkę wiedzy możemy skorzystać z tabel i zdobyć naszą pierwszą flagę!

<details>
  <summary>Flaga</summary>
  
  picoCTF{p}
</details>

![img]({{ 'assets/images/PicoCTF2019_general/lets_warm_up_solution.png' | relative_url }}){: .center-image }*Mamy pierwszą flagę :D. 50 punktów ~~dla Gryffindoru~~ wpadło na nasze konto*

# Warmed Up

![img]({{ 'assets/images/PicoCTF2019_general/warmed_up.png' | relative_url }}){: .center-image }*hmm, wygląda znajomo*

<details>
  <summary>Podpowiedzi</summary>
  
  Wyślij swoją odpowiedź w naszym formacie flagi. Dla przykładu, jeśli rozwiązanie to "22", wyślij je jako "picoCTF{22}".
</details>

Posiadając wiedzę zdobytą podczas rozwiązywania poprzedniego zadania możemy wykorzystać moc prostej matematyki i zdobyć flagę.

<details>
  <summary>Flaga</summary>
  
  0x3D = 3x16 + 13 = 61. Nie zapominając o formacie flagi, rozwiązanie to picoCTF{61}
</details>

W nagrodę otrzymujemy fragment historii, dostęp do minigry (która jest oparta na realnych komendach) oraz, co najważniejsze - drzwi do pozostałych lokacji zostają otwarte.