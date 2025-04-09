# Przykłady użycia Gmail Auto Sorter

## Przykład 1: Pierwsze uruchomienie i podstawowa konfiguracja

Scenariusz: Masz nową instalację Gmail Auto Sorter i chcesz skonfigurować system po raz pierwszy.

### Krok 1: Konfiguracja uwierzytelniania

```bash
# Upewnij się, że katalog tokens istnieje
mkdir -p tokens

# Skopiuj plik credentials.json do katalogu tokens
cp ~/Downloads/credentials.json tokens/

# Uruchom skrypt, aby zainicjować uwierzytelnianie
python3 direct_sort.py
```

W przeglądarce otworzy się okno logowania Google. Po zalogowaniu i udzieleniu uprawnień, token zostanie zapisany i możesz zamknąć przeglądarkę.

### Krok 2: Pierwsza analiza nadawców

```bash
python3 analyze_senders.py
```

### Krok 3: Bezpośrednie sortowanie wszystkich wiadomości

```bash
python3 direct_sort.py
```

### Krok 4: Konfiguracja automatycznego uruchamiania

```bash
./setup_cron.sh
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
  }
]
```

### Krok 2: Zastosowanie reguł do istniejących wiadomości

```bash
python3 apply_filters_to_existing.py
```

## Przykład 3: Identyfikacja i obsługa nieposortowanych wiadomości

### Krok 1: Identyfikacja nieposortowanych wiadomości

```bash
python3 identify_unfiltered_emails.py
```

### Krok 2: Automatyczne tworzenie reguł dla nieposortowanych wiadomości

```bash
python3 create_rules_for_unsorted.py
```

### Krok 3: Integracja nowych reguł z istniejącymi

```bash
python3 merge_sorting_rules.py
```

### Krok 4: Zastosowanie połączonych reguł

```bash
python3 apply_filters_to_existing.py
```

## Przykład 4: Masowe sortowanie dla bardzo dużej skrzynki

Scenariusz: Masz bardzo dużą liczbę wiadomości (1000+) i potrzebujesz skutecznie je posortować.

### Krok 1: Uruchomienie skryptu awaryjnego czyszczenia

```bash
./run_emergency_sort.sh
```

### Krok 2: Weryfikacja wyników i doczyszczenie pozostałych wiadomości

```bash
python3 identify_unfiltered_emails.py
python3 direct_sort.py
```

## Przykład 5: Monitorowanie i raportowanie

Scenariusz: Chcesz regularnie monitorować stan swojej skrzynki i otrzymywać raporty o sortowaniu.

### Sprawdzanie stanu skrzynki

```bash
python3 check_inbox_safely.py
```

### Sprawdzanie logów sortowania

```bash
cat direct_sort.log
```