# Audiobajki Disney plus Albik

## Opis
Sposób pozwalający uruchomić Audiobajki Disney na Albiku.


Filmik: https://www.youtube.com/shorts/5ct0cT9k29E

| Strona 1 | Strona 2 |
:-------------------------:|:-------------------------:
![](https://raw.githubusercontent.com/Patresss/Audiobajki-Disney-plus-Albik/main/Przyk%C5%82ady/Plansza%201.jpg) | ![](https://raw.githubusercontent.com/Patresss/Audiobajki-Disney-plus-Albik/main/Przyk%C5%82ady/Plansza%202.jpg)






## Po co?

- Głośnik do Audiobajek Disney często się psuje, a kupienie nowego nierzadko kosztuje bardzo wiele pieniędzy. Dodatkowo, też nie mamy pewności, czy uda nam się go kupić.
- Głośnik jest mało poręczny i z figurkami zajmuje dużo miejsca. Biorąc Albika z listą bajek, z łatwością weżmiemy go do samochodu, dziadków czy na wakacje. Gdybyśmy chcieli zabrać 99 figurek ze sobą, byłoby to niemożliwe
- Albik z listą bajek jest bardziej dostępny. Sam mam wszystkie figurki na półce po które dziecko bez mojej pomocy nie ma łatwego dostępu


## Co jest potrzebne:
- Pliki z audiobajkami
- Albik - jest to stosunkowo duży koszt, więc jak ktoś chce go kupić tylko dlatego aby posłuchać bajek Disney to raczej nie ma to sensu. Ja już go miałem i polecam go z innymi książeczkami (https://albipolska.pl/zestawy-z-piorem/)
- Naklejki do nagrywania Albik - https://albipolska.pl/naklejki-do-nagrywania/
 
## Instrukcja

1. Dostajemy się do plików
 - a) Czasami są łatwo dostepne na karcie. Jeżeli tak to je kopiujemy.
 - b) Gdy nie możemy ich skopiować na komputerze to możemy spróbować włożyć karte do telefonu. Windows nie radzi sobie z chińskimi znaczkami, więc może jest to jakieś rozwiązanie (nie testowałem tego).
 - c) Możemy zmienić nazwę plików na Windows używajać git bash konsoli (https://git-scm.com/downloads):
Gdzie `F:` to jest nasza karta, a `D:Audiobajki` docelowe miejsce na naszym komputerze. Komenda `mv` zmienia nazwe folderu.
```
cp -r F:/* D:/Audiobajki/
```
Po skopiowanie proszę przejść do odpowiedniego folderu w konsoli np.
```
cd D:/Audiobajki
```
Następnie wykonać komendę do zmiany nazwy:
```
mv 波兰语1-99号故事. Audiobajki
``` 
Jeżeli nazwa folderu jest inna niż powyżej (inne chińskie znaczki) to należy wykonać komendę "ls" i skopiować nową nazwę i wtedy 
```
mv <skopiowana nazwa folderu z chińskimi znaczkami> Audiobajki
``` 
- d) Pliki też są łatwo dostep1ne w Internecie (przynajmniej wersje z 80 bajkami), jednak nie podam bezpośredneigo linku - naprawdę łatwo je znaleźć.
2. Konwertujemy pliki mp3 na wav i zmieniamy nazwy pasujące do naklejek np. `Rec001.wav`.
- a) Możemy zrobić to ręcznie, używając programu AudaCity - https://www.youtube.com/watch?v=9mTua6ZwFJU, ale jest to czasochłonne - musimy zrobić to na każdego pliku
- b) Ja zrobiłem to w inny sposób (dla bardziej zaawansowanych użytkowników):
  - Zmieniłem nazwy plików takie jakie są na planszy - tylko trzeba uważać na sortowanie spacji i polskich znaków. Np.`Bambi 2.mp3` będzie przed `Bambi.mp3` , oraz `Pinokio.mp3` będzie przed `Piękna i Bestia`. Dlatego polecam nie używać polskich znaków, oraz kończyć je dodatkowymi spacjami: `Bambi.mp3` -> `Bambi  .mp3` tak aby był przed `Bambi 2.mp3`
  - Mając posortowaną listę wykonujemy skrypt w PowerShell który iteruje po posortowanych plikach i zamienia je na `wav` oraz zmienia im nazwy na `RecXXX.wav`:
```
# Inicjalizacja licznika dla nazw plików
$licznik = 1

# Znajdź wszystkie pliki MP3 w bieżącym folderze
$plikiMp3 = Get-ChildItem -Path . -Filter *.mp3

# Dla każdego pliku MP3 w folderze, przekonwertuj go na WAV używając ffmpeg
foreach ($plik in $plikiMp3) {
    # Formatowanie nazwy pliku wyjściowego z wiodącymi zerami
    $nazwaPlikuWav = "REC{0:D3}.wav" -f $licznik
    
    # Wykonanie konwersji
    ffmpeg -i $plik.FullName -acodec pcm_s16le -ar 44100 -ac 2 $nazwaPlikuWav
    
    # Inkrementacja licznika dla kolejnej nazwy pliku
    $licznik++
}

# Informacja o zakończeniu procesu
Write-Host "Konwersja zakonczona."
```
  - Po tej operacji pliki działały na komputerze, ale Albik ich nie potrafił odtworzyć. Problem był w metadanych/tagach (ale nie wiem co dokładnie). Za pomocą programu mp3Tag otworzyłem wszystkie pliki i zmieniłem im datę na `2020` - taka zmiana sprawiła, że Albik potrafił odtworzyć pliki.
3. Mają gotowe pliki wav (`RecXXX.wav`) podłączamy Albika z komputerem i kopiujemy je wszystkie do folder `Rec` (jeżeli go nie mamy to tworzymy go)
4. Drukujemy planszę - najlepiej zaznaczając opcję "bez krawędzi" (ja zrobiłem to dwustronnie na twardym papierze). Pliki dostępne tutaj: https://github.com/Patresss/Audiobajki-Disney-plus-Albik/tree/main/Pliki%20do%20druku
5. Naklejamy naklejki zgodnie z plikami - np. jeżeli plik `Rec001.wav` zawiera bajkę "101 Dalmatyńczyków" to koło tej bajki naklejamy naklejkę "REC001"
6. Laminujemy (krok opcjonalny - Albik wykrywa naklejki po laminowaniu)
7. Gotowe! Możemy teraz używać pióra Albika do słuchania bajek Disneya
