# Lumo® — strona oferty mieszkania do nagrania rolki

Wersja „nieruchomości” stron Solen®, Halden® i Vela®: fikcyjne biuro Lumo, przykładowe mieszkanie 70 m² na Mokotowie.
Otwórz `index.html` w Chrome lub Safari, włącz pełny ekran i scrolluj (albo z folderu `vizita-rolki`:
`python3 -m http.server 8766`, potem http://localhost:8766/lumo/).

## Skróty
- **A**: auto-scroll przez całą stronę (ok. 16 s, równe tempo, najlepszy do nagrywania). Ponowne A zatrzymuje, kółko myszy też.
- **C**: ukryj / pokaż kursor.
- **0**: powrót na górę.

## Co jest czym
- `frames/hero/`: klatki wideo hero, kamera z salonu wznosi się ponad mieszkanie bez sufitu (Kling 3.0, OpenArt).
- `frames/product/`: klatki ruchu kamery wokół modelu mieszkania na kremowym tle: z góry → widok 3/4 (Veo 3.1, OpenArt).
  Tylko 0–5,1 s z `source/model-move.mp4`: dalej Veo przenika („duch” modelu) do klatki końcowej, zamiast dokończyć ruch.
- `frames/product-mask.png`: wycięty model z pierwszej klatki (Vision na Macu), maska na czas przejścia.
- `frames/manifest.js`: liczba klatek + krzywe ruchu modelu i hero (równe tempo przy scrollu; w hero ściany „opadają”
  szybko, krzywa daje temu momentowi więcej scrolla).
- `source/`: oryginały z OpenArt (obrazy GPT Image 2 + oba wideo .mp4).

## Przejście
1. Hero kończy się widokiem z góry na całe mieszkanie bez sufitu.
2. Wnętrze się rozmywa.
3. Ostry model tego samego mieszkania pojawia się dokładnie na nim (maska + `MATCH`, zmierzone po obrysie modelu:
   `narzedzia-stron/obrys.py`).
4. Rozmyte wnętrze znika w kolor strony (`--paper`, próbka tła wideo z modelem), kamera schodzi do widoku 3/4.

## Jasny motyw
- Kolory w `:root`: `--ink` (tekst), `--paper` (tło strony = tło modelu). Brzegi klatek modelu są wygaszane, żeby tło wideo
  zlewało się ze stroną.

## Strojenie (w `index.html`)
- `.track { height: 900vh }`: długość scrolla (więcej = wolniej).
- `T = { ... }`: kiedy gra hero, kiedy jest przejście, kiedy rusza kamera nad modelem.
- `data-in` / `data-out` na `.callout`: kiedy pojawiają się teksty.
- `AUTO_SECONDS`: czas auto-scrolla.
