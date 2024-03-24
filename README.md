# Audiobajki Disney plus Albik

## Opis
Sposób pozwalający uruchomić Audiobajki Disney na Albiku.

<filnik>

## Po co?

- Głośnik do Audiobajek Disney często się psuje, a kupienie nowego nierzadko kosztuje bardzo wiele pieniędzy. Dodatkowo, też nie mamy pewności, czy uda nam się go kupić.
- Głośnik jest mało poręczny i z figurkami zajmuje dużo miejsca. Biorąc Albika z listą bajek, z łatwością weżmiemy go do samochodu, dziadków czy na wakacje. Gdybyśmy chcieli zabrać 99 figurek ze sobą, byłoby to niemożliwe
- Albik z listą bajek jest bardziej dostępny. Sam mam wszystkie figurki na półce po które dziecko bez mojej pomocy nie ma łatwego dostępu


## Co jest potrzebne:
- Pliki z audiobajkami
- Albik - jest to stosunkowo duży koszt, więc jak ktoś chce go kupić tylko dlatego aby posłuchać bajek Disney to raczej nie ma to sensu. Ja już go miałem i polecam go z innymi książeczkami (https://albipolska.pl/zestawy-z-piorem/)
- Naklejki do nagrywania Albik - https://albipolska.pl/naklejki-do-nagrywania/
 
## Instrukcja

1. Pobieramy pliki mp3 z głośnika (choć są też bardzo łatwo dostępne w Internecie)
a) Wyciągamy kartę z głośnika i wkładamy ją do komputera
b) Kopiujemy pliki na komputer. W zależności od wersji są różne stopnie trudności. Na jednym głośniku miałem łatwo dostępne pliki, a na innym folder miał chińskie znaczki. Dostałem się do niego kopiując pliki na komputer i zmieniając nazwę folderu używając konsoli bash np. z git (https://git-scm.com/downloads):
```
cp -r F:/* D:/Audiobajki/
mv 波兰语1-99号故事. Audiobajki
```

2. Konwertujemy pliki mp3 na wav i zmieniamy nazwy pasujące do naklejek np. `Rec001.wav`.
a) Możemy zrobić to ręcznie, używając programu AudaCity - https://www.youtube.com/watch?v=9mTua6ZwFJU, ale jest to czasochłonne - musimy zrobić to na każdego pliku
b) Ja zrobiłem to w inny sposób (dla bardziej zaawansowanych użytkowników):
* Zmieniłem nazwy plików takie jakie są na planszy - tylko trzeba uważać na sortowanie spacji i polskich znaków. Np.`Bambi 2.mp3` będzie przed `Bambi.mp3` , oraz `Pinokio.mp3` będzie przed `Piękna i Bestia`. Dlatego polecam nie używać polskich znaków, oraz kończyć je dodatkowymi spacjami: `Bambi.mp3` -> `Bambi  .mp3` tak aby był przed `Bambi 2.mp3`
* Mając posortowaną listę wykonujemy skrypt w PowerShell który iteruje po posortowanych plikach i zamienia je na `wav` oraz zmienai im nazwy na `RecXXX.wav`:
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
* Po tej operacji pliki działały na komputerze, ale Albik ich nie potrafił odtworzyć. Problem był w metadanych/tagach (ale nie wiem co dokładnie). Za pomocą programu mp3Tag oworzyłem wszystkie pliki i zmieniłem im datę na `2020` - taka zmiana sprawiła, że Albik potrafił odtworzyć pliki.
3. Mają gotowe pliki wav (`RecXXX.wav`) podłączamy Albika z komputerem i kopiujemy je wszystkie do folder `Rec` (jeżeli go nie mamy to tworzymy go)
4. Drukujemy planszę - najlepiej zaznaczając opcję "bez krawędzi" (ja zrobiłem to dwustronnie na twardym papierze). Pliki dostępne tutaj: XXXXXX
5. Naklejamy naklejki zgodnie z plikami - np. jeżeli plik `Rec001.wav` zawiera bajkę "101 Dalmatyńczyków" to koło tej bajki naklejamy naklejkę "REC001"
6. Laminujemy (krok opcjonalny - Albik wykrywa naklejki po laminowaniu)
7. Gotowe! Możemy teraz używać pióra Albika do słuchania bajek Disneya
