# Przykłady użycia Gmail Auto Sorter

## Przykład 1: Pierwsze uruchomienie i podstawowa konfiguracja

Scenariusz: Masz nową instalację Gmail Auto Sorter i chcesz skonfigurować system po raz pierwszy.

### Krok 1: Konfiguracja uwierzytelniania

```bash
# Upewnij się, że katalog tokens istnieje
mkdir -p tokens

# Skopiuj plik credentials.json do katalogu tokens
cp ~/Downloads/credentials.json tokens/

# Uruchom first skrypt, aby zainicjować uwierzytelnianie
python3 direct_sort.py
```

W przeglądarce otworzy się okno logowania Google. Po zalogowaniu i udzieleniu uprawnień, token zostanie zapisany i możesz zamknąć przeglądarkę.

### Krok 2: Pierwsza analiza nadawców

```bash
python3 analyze_senders.py
```

Wyniki:
```
Analiza nadawców zakończona. Znaleziono 156 unikalnych nadawców.
Top 10 najczęstszych nadawców:
1. newsletter@example.com (45 wiadomości)
2. updates@company.com (32 wiadomości)
3. notifications@service.org (28 wiadomości)
...
Szczegółowe wyniki zapisano w pliku senders_analysis.json
```

### Krok 3: Bezpośrednie sortowanie wszystkich wiadomości

```bash
python3 direct_sort.py
```

Wyniki:
```
Znaleziono 312 wiadomości w Inbox
Pobrano szczegóły dla 312 wiadomości
Utworzono nową etykietę: Newslettery (ID: Label_42)
Utworzono nową etykietę: Newslettery/example.com (ID: Label_43)
Posortowano 45 wiadomości z domeny example.com
Utworzono nową etykietę: Newslettery/company.com (ID: Label_44)
Posortowano 32 wiadomości z domeny company.com
...
Bezpośrednie sortowanie zakończone.
```

### Krok 4: Konfiguracja automatycznego uruchamiania

```bash
./setup_cron.sh
```

Wyniki:
```
Skonfigurowano automatyczne sortowanie wiadomości co godzinę.
Logi będą zapisywane w pliku: ../logs/cron_direct_sort.log
Zadanie cron zostało pomyślnie skonfigurowane.
```

## Przykład 2: Tworzenie i stosowanie własnych reguł sortowania

Scenariusz: Chcesz dodać niestandardowe reguły sortowania dla konkretnych nadawców.

### Krok 1: Utworzenie pliku reguł sortowania

Utwórz lub edytuj plik `sorting_rules.json`:

```json
[
  {
    "sender": "newsletter@example.com",
    "label": "Newslettery/Technologia"
  },
  {
    "sender": "updates@company.com",
    "label": "Praca/Aktualizacje"
  },
  {
    "sender": "*@bank.com",
    "label": "Finanse/Bank"
  },
  {
    "sender": "notifications@socialmedia.com",
    "label": "Media Społecznościowe"
  }
]
```

### Krok 2: Zastosowanie reguł do istniejących wiadomości

```bash
python3 apply_filters_to_existing.py
```

Wyniki:
```
Wczytywanie reguł sortowania z pliku sorting_rules.json
Wczytano 4 reguły sortowania
Utworzono nową etykietę: Newslettery/Technologia (ID: Label_45)
Posortowano 45 wiadomości od nadawcy newsletter@example.com
Utworzono nową etykietę: Praca/Aktualizacje (ID: Label_46)
Posortowano 32 wiadomości od nadawcy updates@company.com
...
Zastosowano wszystkie reguły sortowania. Posortowano łącznie 117 wiadomości.
```

## Przykład 3: Identyfikacja i obsługa nieposortowanych wiadomości

Scenariusz: Pomimo zastosowania reguł sortowania, niektóre wiadomości pozostają nieprzesortowane.

### Krok 1: Identyfikacja nieposortowanych wiadomości

```bash
python3 identify_unfiltered_emails.py
```

Wyniki:
```
Sprawdzanie nieprzefiltrowanych wiadomości...
Znaleziono 45 nieprzefiltrowanych wiadomości w skrzynce odbiorczej
Top 5 nadawców nieposortowanych wiadomości:
1. notifications@forum.org (12 wiadomości)
2. info@shop.com (8 wiadomości)
3. admin@service.net (6 wiadomości)
...
Szczegółowe informacje zapisano w pliku unfiltered_emails.json
```

### Krok 2: Automatyczne tworzenie reguł dla nieposortowanych wiadomości

```bash
python3 create_rules_for_unsorted.py
```

Wyniki:
```
Generowanie reguł dla nieposortowanych wiadomości...
Wczytano dane o 45 nieprzefiltrowanych wiadomościach
Wygenerowano 15 nowych reguł sortowania
Zapisano nowe reguły do pliku new_sorting_rules.json
```

### Krok 3: Integracja nowych reguł z istniejącymi

```bash
python3 merge_sorting_rules.py
```

Wyniki:
```
Łączenie plików reguł sortowania...
Wczytano 4 istniejące reguły z sorting_rules.json
Wczytano 15 nowych reguł z new_sorting_rules.json
Po usunięciu duplikatów, otrzymano 19 reguł sortowania
Zapisano połączone reguły do pliku sorting_rules.json
Utworzono kopię zapasową oryginalnych reguł w sorting_rules.json.bak
```

### Krok 4: Zastosowanie połączonych reguł

```bash
python3 apply_filters_to_existing.py
```

Wyniki:
```
Wczytywanie reguł sortowania z pliku sorting_rules.json
Wczytano 19 reguł sortowania
[...]
Zastosowano wszystkie reguły sortowania. Posortowano łącznie 162 wiadomości.
```

## Przykład 4: Masowe sortowanie dla bardzo dużej skrzynki

Scenariusz: Masz bardzo dużą liczbę wiadomości (1000+) i potrzebujesz skutecznie je posortować.

### Krok 1: Uruchomienie skryptu awaryjnego czyszczenia

```bash
./run_emergency_sort.sh
```

Wyniki:
```
Rozpoczynam awaryjne sortowanie skrzynki...
Krok 1: Sortowanie według domen...
Krok 2: Identyfikacja nieposortowanych wiadomości...
Krok 3: Tworzenie reguł dla nieposortowanych wiadomości...
Krok 4: Zastosowanie nowych reguł...
Awaryjne sortowanie zakończone. Sprawdź logi w katalogu ../logs.
```

### Krok 2: Weryfikacja wyników i doczyszczenie pozostałych wiadomości

```bash
python3 identify_unfiltered_emails.py
```

Wyniki:
```
Sprawdzanie nieprzefiltrowanych wiadomości...
Znaleziono 12 nieprzefiltrowanych wiadomości w skrzynce odbiorczej
[...]
```

```bash
python3 direct_sort.py
```

Wyniki:
```
Znaleziono 12 wiadomości w Inbox
[...]
Bezpośrednie sortowanie zakończone.
```

## Przykład 5: Monitorowanie i raportowanie

Scenariusz: Chcesz regularnie monitorować stan swojej skrzynki i otrzymywać raporty o sortowaniu.

### Sprawdzanie stanu skrzynki

```bash
python3 check_inbox_safely.py
```

Wyniki:
```
Analiza skrzynki odbiorczej:
- Łączna liczba wiadomości w skrzynce: 1245
- Liczba wiadomości w Inbox: 0
- Liczba nieprzeczytanych wiadomości: 37

Statystyki etykiet:
- Newslettery: 856 wiadomości
- Newslettery/example.com: 145 wiadomości
- Newslettery/company.com: 78 wiadomości
- Praca/Aktualizacje: 67 wiadomości
...

Raport zapisano w pliku inbox_report.txt
```

### Sprawdzanie logów sortowania

```bash
cat direct_sort.log
```

```
2025-04-08 15:30:22 - INFO - Rozpoczynam bezpośrednie sortowanie wiadomości...
2025-04-08 15:30:23 - INFO - Znaleziono 12 wiadomości w Inbox
2025-04-08 15:30:25 - INFO - Pobrano szczegóły dla 12 wiadomości
...
2025-04-08 15:30:45 - INFO - Bezpośrednie sortowanie zakończone.
2025-04-08 15:30:45 - INFO - Sortowanie zakończone sukcesem
```

Te przykłady pokazują typowe scenariusze użycia Gmail Auto Sorter i mogą służyć jako przewodnik do efektywnego korzystania z systemu w różnych sytuacjach.