# Przewodnik użytkowania Gmail Auto Sorter

## Wprowadzenie

Gmail Auto Sorter został zaprojektowany, aby pomóc w organizacji skrzynki Gmail poprzez automatyczne sortowanie wiadomości według nadawców lub domen. Ten przewodnik przeprowadzi Cię przez wszystkie funkcje systemu i najlepsze praktyki jego używania.

## Podstawowe koncepcje

### Etykiety Gmail

System wykorzystuje etykiety Gmail do organizacji wiadomości. Główna struktura to:

- **Newslettery** - główna etykieta dla wszystkich posortowanych wiadomości
- **Newslettery/[domena]** - podetykiety dla wiadomości z konkretnych domen (np. Newslettery/gmail.com)

### Sortowanie według domen

Podstawowa strategia sortowania polega na grupowaniu wiadomości według domen nadawców. Na przykład, wszystkie wiadomości od *@google.com będą w etykiecie Newslettery/google.com.

## Główne funkcje

### 1. Bezpośrednie sortowanie (zalecane)

Skrypt `direct_sort.py` to najskuteczniejsze narzędzie do szybkiego i efektywnego porządkowania skrzynki. Sortuje on wszystkie wiadomości według domeny nadawcy bez potrzeby definiowania reguł.

```bash
python3 direct_sort.py
```

Użyj tego skryptu, gdy:
- Masz dużą liczbę nieprzeczytanych wiadomości
- Potrzebujesz szybko oczyścić skrzynkę odbiorczą
- Nie chcesz definiować niestandardowych reguł sortowania

### 2. Sortowanie według reguł

Skrypt `gmail_sorter.py` sortuje wiadomości według zdefiniowanych reguł w pliku `sorting_rules.json`.

```bash
./run_gmail_sorter.sh
```

Użyj tego, gdy:
- Chcesz mieć większą kontrolę nad sposobem sortowania
- Potrzebujesz niestandardowych reguł dla konkretnych nadawców

### 3. Analiza nadawców

Skrypt `analyze_senders.py` analizuje Twoją skrzynkę i identyfikuje najczęstszych nadawców. Jest to pomocne przy tworzeniu reguł sortowania.

```bash
python3 analyze_senders.py
```

Wyniki są zapisywane w pliku `senders_analysis.json`.

### 4. Identyfikacja nieposortowanych wiadomości

Skrypt `identify_unfiltered_emails.py` znajduje wiadomości, które nie zostały posortowane przez istniejące reguły.

```bash
python3 identify_unfiltered_emails.py
```

### 5. Automatyczne uruchamianie

Aby sortowanie odbywało się automatycznie, możesz użyć zadania cron skonfigurowanego przez `setup_cron.sh`. Domyślnie sortowanie odbywa się co godzinę.

```bash
./setup_cron.sh
```

## Scenariusze użycia

### Scenariusz 1: Pierwsza konfiguracja i czyszczenie

1. Uruchom analizę nadawców:
   ```bash
   python3 analyze_senders.py
   ```

2. Uruchom bezpośrednie sortowanie:
   ```bash
   python3 direct_sort.py
   ```

3. Skonfiguruj automatyczne uruchamianie:
   ```bash
   ./setup_cron.sh
   ```

### Scenariusz 2: Okresowe ręczne sortowanie

1. Uruchom bezpośrednie sortowanie:
   ```bash
   python3 direct_sort.py
   ```

2. Sprawdź logi, aby zobaczyć wyniki:
   ```bash
   cat direct_sort.log
   ```

### Scenariusz 3: Dostosowanie reguł sortowania

1. Edytuj plik `sorting_rules.json` dodając nowe reguły:
   ```json
   [
     {
       "sender": "newsletter@example.com",
       "label": "Newslettery/Przykład"
     },
     {
       "sender": "*@company.com",
       "label": "Praca/Company"
     }
   ]
   ```

2. Uruchom sorter z niestandardowymi regułami:
   ```bash
   ./run_gmail_sorter.sh
   ```

## Najlepsze praktyki

1. **Regularnie uruchamiaj sortowanie** - Najlepiej skonfigurować automatyczne uruchamianie

2. **Twórz sensowne etykiety** - Używaj hierarchicznej struktury etykiet (np. Newslettery/Tech/Google)

3. **Obserwuj logi** - Regularnie sprawdzaj pliki logów, aby wykryć potencjalne problemy

4. **Aktualizuj reguły** - Okresowo analizuj nowych nadawców i aktualizuj reguły sortowania

5. **Używaj direct_sort.py dla dużych czyszczeń** - Kiedy masz setki nieprzeczytanych wiadomości

## Monitorowanie i logi

Wszystkie skrypty zapisują szczegółowe logi w katalogu scripts/. Najważniejsze pliki logów to:

- `direct_sort.log` - Wyniki bezpośredniego sortowania
- `gmail_sorter.log` - Wyniki sortowania według reguł
- `analyze_senders.log` - Wyniki analizy nadawców

Monitoruj te pliki, aby śledzić wydajność systemu i diagnozować potencjalne problemy.

## Następne kroki

Teraz, gdy znasz już podstawy korzystania z Gmail Auto Sorter, możesz:

- Zapoznać się z [instrukcją rozwiązywania problemów](TROUBLESHOOTING.md)
- Przejrzeć [przykłady użycia](EXAMPLES.md)
- Poznać [procedury awaryjne](EMERGENCY.md) dla sytuacji kryzysowych