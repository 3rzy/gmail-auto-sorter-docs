# Procedura awaryjnego czyszczenia skrzynki Gmail

## Wprowadzenie

Ten dokument opisuje procedurę szybkiego i skutecznego czyszczenia przepełnionej skrzynki Gmail. Jeśli masz setki lub tysiące nieprzeczytanych wiadomości i potrzebujesz natychmiast uporządkować skrzynkę, postępuj zgodnie z tą instrukcją.

## Przygotowanie

1. Upewnij się, że masz zainstalowane wszystkie wymagane komponenty:
   - Python 3.7+
   - Biblioteki: google-api-python-client, google-auth-httplib2, google-auth-oauthlib
   - Skonfigurowane uwierzytelnianie Google API

2. Zatrzymaj istniejące zadania automatycznego sortowania (jeśli są uruchomione):
   ```bash
   crontab -l | grep -v "direct_sort.py" | crontab -
   ```

## Procedura awaryjnego sortowania

### Krok 1: Uruchom bezpośrednie sortowanie z maksymalnymi parametrami

Skrypt `direct_sort.py` został zaprojektowany do szybkiego porządkowania dużej liczby wiadomości. W sytuacji awaryjnej możemy zwiększyć jego możliwości:

1. Edytuj plik direct_sort.py i zmień parametry:

   ```python
   # Znajdź funkcję get_inbox_messages i zwiększ limit wiadomości
   def get_inbox_messages(service, max_results=2000):  # Zmień z 500 na 2000
   ```

2. Uruchom skrypt z priorytetem:

   ```bash
   nice -n -20 python3 direct_sort.py > emergency_sort.log 2>&1
   ```

### Krok 2: Wykonaj równoległe sortowanie według etykiet systemowych

Uruchom skrypt `run_emergency_sort.sh`, który automatycznie:
1. Sortuje wszystkie newslettery według domen
2. Przesuwa wszystkie promocje do oddzielnej etykiety
3. Sortuje wiadomości automatyczne (powiadomienia, potwierdzenia)
4. Archiwizuje stare wiadomości

```bash
./run_emergency_sort.sh
```

Jeśli ten skrypt nie jest dostępny, możesz go utworzyć ręcznie:

```bash
#!/bin/bash

SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )"
LOG_DIR="${SCRIPT_DIR}/../logs"
mkdir -p "${LOG_DIR}"

echo "Rozpoczynam awaryjne sortowanie skrzynki..."

# Krok 1: Bezpośrednie sortowanie wszystkich wiadomości
echo "Krok 1: Sortowanie według domen..."
python3 "${SCRIPT_DIR}/direct_sort.py" > "${LOG_DIR}/emergency_direct_sort.log" 2>&1

# Krok 2: Identyfikacja i sortowanie pozostałych wiadomości
echo "Krok 2: Identyfikacja nieposortowanych wiadomości..."
python3 "${SCRIPT_DIR}/identify_unfiltered_emails.py" > "${LOG_DIR}/emergency_identify.log" 2>&1

# Krok 3: Twórz reguły dla nieposortowanych wiadomości
echo "Krok 3: Tworzenie reguł dla nieposortowanych wiadomości..."
python3 "${SCRIPT_DIR}/create_rules_for_unsorted.py" > "${LOG_DIR}/emergency_create_rules.log" 2>&1

# Krok 4: Zastosuj nowe reguły
echo "Krok 4: Zastosowanie nowych reguł..."
python3 "${SCRIPT_DIR}/apply_filters_to_existing.py" > "${LOG_DIR}/emergency_apply_filters.log" 2>&1

echo "Awaryjne sortowanie zakończone. Sprawdź logi w katalogu ${LOG_DIR}."
```

Nadaj uprawnienia wykonywania:

```bash
chmod +x run_emergency_sort.sh
```

### Krok 3: Weryfikacja wyników

1. Sprawdź logi sortowania:
   ```bash
   cat emergency_sort.log
   ```

2. Sprawdź skrzynkę Gmail i zweryfikuj, czy:
   - Wiadomości zostały posortowane do odpowiednich etykiet
   - Skrzynka odbiorcza została opróżniona
   - Etykiety zostały poprawnie utworzone

3. Jeśli pozostały jeszcze nieposortowane wiadomości, uruchom ponownie skrypt direct_sort.py:
   ```bash
   python3 direct_sort.py
   ```

## Ręczne sortowanie jako ostateczność

Jeśli automatyczne skrypty nie rozwiązały problemu w całości, możesz ręcznie posortować pozostałe wiadomości przy użyciu interfejsu Gmail:

1. **Sortowanie według nadawcy**:
   - Użyj operatora `from:` w wyszukiwarce Gmail, np. `from:newsletter@example.com`
   - Zaznacz wszystkie wiadomości (wybierz "Wszystkie wiadomości", jeśli jest ich więcej niż na jednej stronie)
   - Zastosuj odpowiednią etykietę (lub utwórz nową)
   - Kliknij "Archiwizuj", aby usunąć je ze skrzynki odbiorczej

2. **Sortowanie według domeny**:
   - Wyszukaj `from:(*@example.com)` aby znaleźć wszystkie wiadomości z danej domeny
   - Grupuj i etykietuj jak powyżej

3. **Grupowanie podobnych wiadomości**:
   - Wyszukuj według słów kluczowych w tematach, np. `subject:(newsletter OR update)`
   - Sortuj i archiwizuj partiami

## Zapobieganie przyszłym problemom

Po przeprowadzeniu awaryjnego czyszczenia, wdróż następujące środki zapobiegawcze:

1. **Skonfiguruj automatyczne sortowanie**:
   ```bash
   ./setup_cron.sh
   ```

2. **Regularnie monitoruj liczbę nieposortowanych wiadomości**:
   ```bash
   python3 identify_unfiltered_emails.py
   ```

3. **Aktualizuj reguły sortowania** w miarę pojawiania się nowych nadawców:
   ```bash
   python3 analyze_senders.py
   python3 create_rules_for_unsorted.py
   ```

## Wskaźniki skuteczności

Skuteczne czyszczenie awaryjne powinno dać następujące rezultaty:

- **Skrzynka odbiorcza**: 0 wiadomości lub tylko bardzo świeże wiadomości
- **Struktura etykiet**: Przejrzysta hierarchia z główną etykietą Newslettery i podetykietami dla domen
- **Organizacja**: Podobne wiadomości pogrupowane razem w odpowiednich etykietach

Po zakończonym procesie czyszczenia, Twoja skrzynka powinna być zorganizowana, przejrzysta i łatwa w zarządzaniu.