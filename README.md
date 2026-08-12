# The Arsenal Ledger

Statyczna strona ze statystykami Arsenalu, czytana bezpośrednio z `arsenal_stats.xlsx`.
Bez backendu, bez bazy danych, bez API. Jeden plik HTML plus Twój Excel.

## Struktura repo

```
/
  index.html
  data/
    arsenal_stats.xlsx
```

To wszystko. GitHub Pages, branch main, katalog root.

## Jak aktualizować statystyki

1. Wypełniasz Excela dokładnie tak jak dotąd (`1` przy występie, `1` pod `G` przy golu).
2. Wchodzisz na GitHuba, do katalogu `data`.
3. "Add file" > "Upload files", przeciągasz plik, ta sama nazwa, commit.
4. Odświeżasz stronę.

Strona pobiera plik z `cache: no-store` i cache busterem, więc nowe dane widać od razu.

## Jak to czyta plik

Parser nie zna żadnych adresów komórek. Dla każdego arkusza szuka wiersza,
w którym kolumna B to `Name`, a potem:

- nagłówki `Apps`/`Goals` (arkusze składu) albo pary `A`/`G` (arkusze meczowe),
- nazwę rozgrywek bierze z wiersza wyżej,
- przeciwników z wiersza nad nagłówkiem, etap rozgrywek z wiersza jeszcze wyżej,
- zawodników czyta w dół aż do pierwszego pustego nazwiska.

Efekt: możesz dodać zawodnika, dołożyć rundę pucharową, zmienić przeciwnika
albo dopisać sezon i nic się nie sypie. Nowy arkusz `Apps & Goals 27-28`
sam pojawi się w przełączniku sezonów.

Arkusz `#` nie jest czytany. Zostaje Twoim backendem w Excelu.

## Podstrony

| Adres | Źródło w pliku |
|---|---|
| `#/` | Season Breakdown (macierz kolejek) plus podsumowanie |
| `#/squad` | Apps & Goals |
| `#/season` | Apps & Goals 16-17 ... 26-27 |
| `#/premier-league` | Premier League |
| `#/europe` | Europe |
| `#/fa-cup` | FA Cup |
| `#/league-cup` | League Cup |
| `#/community-shield` | Other (CS) |
| `#/history` | Season Breakdown |
| `#/ex-players` | Ex-Players |
| `#/records` | All-time records |
| `#/file` | status wczytania, podgląd pliku przed commitem |

## Podgląd przed commitem

Na podstronie `#/file` możesz przeciągnąć kopię pliku i zobaczyć,
jak wygląda po Twoich zmianach, bez ruszania repo.

## Kopia awaryjna

W `index.html` jest wbudowany snapshot danych z 12.08.2026. Używany tylko wtedy,
gdy nie da się pobrać `data/arsenal_stats.xlsx` (np. przy otwarciu pliku lokalnie
przez `file://`). Wtedy w lewym dolnym rogu świeci "Bundled snapshot".

## Do poprawki w Excelu

- `League Cup`, wiersz 41: sumy to `SUM(E5:E40)` zamiast `SUM(E4:E40)`, więc wiersz 4 wypada z sum.
- `Other (CS)`: zawodnicy zaczynają się w wierszu 3, a nie 4 jak wszędzie indziej. Parser to obsługuje.
