# Ceneo Web Scraper

Prosty web scraper pobierający opinie o produktach z serwisu [Ceneo.pl](https://www.ceneo.pl).
Projekt pobiera stronę produktu, parsuje wszystkie opinie i zapisuje je w ustrukturyzowanym
pliku JSON. Powstał jako ćwiczenie z podstaw web scrapingu w Pythonie.

## Funkcje

- Pobiera opinie z wybranej strony produktu na Ceneo
- Wyciąga z każdej opinii jej składowe (autor, ocena, treść, zalety, wady, daty itd.)
- Obsługuje brakujące pola (np. rekomendacja czy data zakupu mogą nie występować)
- Zapisuje wynik do czytelnego pliku `opinions.json` z polskimi znakami

## Wykorzystane technologie

- **Python 3.11**
- **requests** – pobieranie stron HTTP
- **BeautifulSoup4** – parsowanie HTML
- **pandas** – praca z danymi (przygotowane pod dalszą analizę)
- **Jupyter Notebook** – środowisko uruchomieniowe

## Wymagania

Wszystkie zależności znajdują się w pliku `requirements.txt`. Najważniejsze z nich:

```
requests
beautifulsoup4
pandas
```

## Instalacja

1. Sklonuj repozytorium:
   ```bash
   git clone <adres-repozytorium>
   cd WebScraper
   ```

2. (Zalecane) Utwórz i aktywuj wirtualne środowisko:
   ```bash
   python -m venv .venv
   # Windows
   .venv\Scripts\activate
   # macOS / Linux
   source .venv/bin/activate
   ```

3. Zainstaluj zależności:
   ```bash
   pip install -r requirements.txt
   ```

## Użycie

1. Otwórz notebook:
   ```bash
   jupyter notebook ceneo_scrape.ipynb
   ```

2. W komórce z zapytaniem podmień adres URL na stronę produktu, którego opinie chcesz pobrać:
   ```python
   response = requests.get("https://www.ceneo.pl/<ID_PRODUKTU>#tab=reviews", headers=headers)
   ```

3. Uruchom kolejno wszystkie komórki. Po zakończeniu opinie zostaną zapisane do pliku
   `opinions.json` w katalogu projektu.

## Struktura danych

Każda opinia zapisywana jest jako obiekt JSON o następujących polach:

| Pole             | Opis                                              |
|------------------|---------------------------------------------------|
| `opinion_id`     | Unikalny identyfikator opinii                     |
| `author`         | Nazwa autora opinii                               |
| `recommendation` | Rekomendacja (np. „Polecam”) lub `null`           |
| `rating`         | Ocena w skali, np. `5/5`                           |
| `content`        | Treść opinii                                       |
| `pros`           | Lista zalet                                        |
| `cons`           | Lista wad                                          |
| `helpful`        | Liczba głosów „pomocna”                            |
| `unhelpful`      | Liczba głosów „niepomocna”                         |
| `publish_date`   | Data publikacji opinii                            |
| `purchase_date`  | Data zakupu produktu lub `null`                   |

Przykład:

```json
{
    "opinion_id": "17671771",
    "author": "Tomasz",
    "recommendation": "Polecam",
    "rating": "5/5",
    "content": "Dobra jakość wydruku, tanie tusze, dobra współpraca z Android i Apple",
    "pros": ["jakość wydruków", "szybkość wydruku", "wydajność"],
    "cons": [],
    "helpful": "0",
    "unhelpful": "0",
    "publish_date": "2023-07-03 22:28:41",
    "purchase_date": "2023-06-12 11:39:26"
}
```

## Znane ograniczenia

- **Tylko jedna strona** – scraper pobiera opinie wyłącznie z pierwszej strony.
  Ceneo stronicuje opinie, więc aby zebrać wszystkie, należałoby dodać obsługę paginacji.
- **Brak obsługi błędów** – kod nie sprawdza kodu odpowiedzi HTTP ani nie obsługuje timeoutów.
- **Zahardkodowany URL** – adres produktu jest wpisany na sztywno w kodzie.
- **Ocena jako tekst** – `rating` zapisywany jest jako napis (np. `"5/5"`), a nie liczba.
- **Treść renderowana po stronie serwera** – scraper zadziała tylko dla treści obecnej
  w surowym HTML; nie obsługuje elementów ładowanych dynamicznie przez JavaScript.

## Możliwe rozszerzenia

- Dodanie paginacji (zbieranie opinii ze wszystkich stron)
- Walidacja odpowiedzi HTTP i obsługa wyjątków
- Pobieranie URL/ID produktu jako argumentu zamiast wartości na sztywno
- Konwersja ocen na wartości liczbowe i eksport do CSV
- Prosta analiza danych (rozkład ocen, najczęstsze zalety/wady, analiza sentymentu)

## Uwagi prawne i etyczne

Projekt powstał w celach edukacyjnych. Przed scrapingiem dowolnego serwisu warto sprawdzić
plik `robots.txt` oraz jego regulamin. Pobrane dane należy wykorzystywać odpowiedzialnie,
z poszanowaniem praw autorskich oraz przepisów o ochronie danych osobowych (RODO).

## Licencja

Projekt edukacyjny – do dowolnego wykorzystania w celach naukowych.
