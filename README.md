# The Arsenal Dashboard

Statyczna strona ze statystykami Arsenalu, czytana bezpośrednio z `arsenal_stats.xlsx`.
Bez backendu, bez bazy danych, bez API. Jeden plik HTML plus Twój Excel.

## Struktura repo

```
/
  index.html
  admin.html
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
| `#/` | Apps & Goals (kadra, sortowana po numerze) |

| `#/season` | Apps & Goals 16-17 ... 26-27 |
| `#/premier-league` | Premier League |
| `#/europe` | Europe |
| `#/fa-cup` | FA Cup |
| `#/league-cup` | League Cup |
| `#/community-shield` | Other (CS) |
| `#/history` | Season Breakdown (macierz kolejek, wykresy, tabela) |
| `#/ex-players` | Ex-Players |
| `#/records` | All-time records |

## Panel admina

`admin.html` nie jest nigdzie podlinkowany z dashboardu i ma `noindex`.
Wchodzisz na `https://przemke92.github.io/arsenal/admin.html`, przeciągasz kopię pliku
i dostajesz raport: czy wszystkie arkusze są na miejscu, ilu zawodników, ile meczów,
kto nie ma numeru, czy nie ma duplikatów nazwisk. Przycisk "Open the dashboard with this file"
otwiera pełny dashboard na tych danych, tylko w Twojej karcie przeglądarki.
Wyjście z podglądu: "exit preview" w lewym dolnym rogu dashboardu.

## Kopia awaryjna

W `index.html` jest wbudowany snapshot danych z 12.08.2026. Używany tylko wtedy,
gdy nie da się pobrać `data/arsenal_stats.xlsx` (np. przy otwarciu pliku lokalnie
przez `file://`). Wtedy w lewym dolnym rogu świeci "Bundled snapshot".
