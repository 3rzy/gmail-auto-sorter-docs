# Rozwiązywanie problemów Gmail Auto Sorter

## Najczęstsze problemy i ich rozwiązania

### Problem 1: Błędy uwierzytelniania

**Objawy:**
- Komunikat "Invalid Credentials"
- Skrypt kończy działanie z błędem autoryzacji
- Komunikat o wygaśnięciu tokena

**Rozwiązania:**

1. **Odnowienie tokenu uwierzytelniającego**

   Usuń istniejący token i wygeneruj nowy:

   ```bash
   rm tokens/gmail_modify_token.pickle
   python3 direct_sort.py
   ```

   Przy kolejnym uruchomieniu skryptu zostaniesz przekierowany do przeglądarki, aby ponownie zalogować się na swoje konto Google.

2. **Weryfikacja uprawnień API**

   Upewnij się, że masz włączone wszystkie niezbędne zakresy uprawnień w Google Cloud Console:
   - https://www.googleapis.com/auth/gmail.modify
   - https://www.googleapis.com/auth/gmail.settings.basic
   - https://www.googleapis.com/auth/gmail.labels

3. **Sprawdzenie pliku credentials.json**

   Upewnij się, że plik credentials.json jest poprawny i znajduje się w katalogu tokens/.

### Problem 2: Limity API Gmail

**Objawy:**
- Błędy "Rate Limit Exceeded"
- Sortowanie zatrzymuje się w połowie
- Komunikaty o zbyt wielu żądaniach

**Rozwiązania:**

1. **Zmniejszenie liczby jednoczesnych żądań**

   Edytuj plik direct_sort.py i zwiększ czas opóźnienia między operacjami:

   ```python
   # Zmień wartość z 0.5 na wyższą, np. 1.0 lub 2.0
   time.sleep(1.0)  # Opóźnienie, aby uniknąć limitów API
   ```

2. **Przetwarzanie mniejszych partii**

   Zmniejsz rozmiar partii przetwarzanych wiadomości:

   ```python
   # Zmień wartość z 50 na mniejszą, np. 20
   batch_size = 20
   ```

3. **Uruchamianie w godzinach mniejszego obciążenia**

   Uruchamiaj sortowanie w nocy lub w godzinach mniejszego obciążenia serwerów Google.

### Problem 3: Niepełne sortowanie

**Objawy:**
- Niektóre wiadomości pozostają nieprzesortowane
- Mniejsza liczba posortowanych wiadomości niż oczekiwano
- Brak wiadomości w odpowiednich etykietach

**Rozwiązania:**

1. **Użyj direct_sort.py**

   Najbardziej skutecznym rozwiązaniem jest użycie skryptu direct_sort.py, który sortuje wszystkie wiadomości według domeny:

   ```bash
   python3 direct_sort.py
   ```

2. **Analiza nieposortowanych wiadomości**

   Uruchom skrypt do identyfikacji nieposortowanych wiadomości:

   ```bash
   python3 identify_unfiltered_emails.py
   ```

   Następnie dodaj odpowiednie reguły dla zidentyfikowanych nadawców.

3. **Zwiększ limit pobieranych wiadomości**

   Edytuj funkcję get_inbox_messages w pliku direct_sort.py:

   ```python
   # Zmień wartość max_results na wyższą, np. 1000
   def get_inbox_messages(service, max_results=1000):
   ```

### Problem 4: Błędy podczas uruchamiania skryptów

**Objawy:**
- Komunikaty o błędach składni Python
- Brak wymaganych modułów
- Błędy wykonania skryptu

**Rozwiązania:**

1. **Sprawdź wersję Pythona**

   Upewnij się, że używasz Python 3.7 lub nowszej wersji:

   ```bash
   python3 --version
   ```

2. **Zainstaluj ponownie wymagane biblioteki**

   ```bash
   pip3 install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib
   ```

3. **Sprawdź uprawnienia do wykonania**

   Upewnij się, że skrypty mają uprawnienia do wykonania:

   ```bash
   chmod +x *.py
   chmod +x *.sh
   ```

### Problem 5: Zadanie cron nie działa

**Objawy:**
- Automatyczne sortowanie nie uruchamia się
- Brak nowych wpisów w logach
- Wiadomości pozostają nieposortowane

**Rozwiązania:**

1. **Sprawdź konfigurację cron**

   Zweryfikuj, czy zadanie cron jest poprawnie skonfigurowane:

   ```bash
   crontab -l
   ```

   Powinien być widoczny wpis podobny do:
   ```
   0 * * * * cd /ścieżka/do/skryptów && python3 direct_sort.py > ../logs/cron_direct_sort.log 2>&1
   ```

2. **Ręczne ponowne skonfigurowanie cron**

   ```bash
   ./setup_cron.sh
   ```

3. **Sprawdź logi cron**

   ```bash
   cat /var/log/syslog | grep CRON
   ```

   lub

   ```bash
   cat ../logs/cron_direct_sort.log
   ```

## Procedury diagnostyczne

### Procedura 1: Pełna diagnostyka systemu

1. Sprawdź wersję Pythona i zainstalowane biblioteki:
   ```bash
   python3 --version
   pip3 list | grep google
   ```

2. Sprawdź logi z ostatnich uruchomień:
   ```bash
   cat direct_sort.log
   cat gmail_sorter.log
   ```

3. Sprawdź konfigurację API:
   ```bash
   ls -la tokens/
   ```

4. Uruchom skrypt w trybie debugowania (dodaj w kodzie):
   ```python
   logging.basicConfig(
       level=logging.DEBUG,  # Zmień z INFO na DEBUG
       # ...
   )
   ```

### Procedura 2: Resetowanie konfiguracji

Jeśli masz poważne problemy, możesz zresetować całą konfigurację:

1. Usuń tokeny uwierzytelniające:
   ```bash
   rm tokens/gmail_modify_token.pickle
   ```

2. Uruchom ponownie proces uwierzytelniania:
   ```bash
   python3 direct_sort.py
   ```

3. Jeśli to nie pomoże, rozważ ponowne utworzenie projektu w Google Cloud Console i wygenerowanie nowych danych uwierzytelniających.

## Problemy z Gmail API

### Limity API

API Gmail ma następujące limity, które mogą wpływać na działanie systemu:

- **Dzienny limit zapytań**: 1,000,000,000 zapytań na dzień
- **Limit zapytań na użytkownika na sekundę**: 250 zapytań na sekundę na użytkownika
- **Limit zapytań na projekt na sekundę**: 1,000 zapytań na sekundę na projekt

Jeśli napotykasz błędy limitów, skrypty są wyposażone w mechanizm wykładniczego wycofania (exponential backoff), który automatycznie ponawia próby z coraz dłuższymi przerwami.

## Kontakt i pomoc

Jeśli napotkasz problemy, których nie możesz rozwiązać za pomocą tego przewodnika:

1. Sprawdź [dokumentację API Gmail](https://developers.google.com/gmail/api/guides)
2. Zgłoś problem w [repozytorium GitHub](https://github.com/krzyk/gmail-auto-sorter/issues)
3. Sprawdź logi błędów i dołącz je do zgłoszenia (po usunięciu danych wrażliwych)