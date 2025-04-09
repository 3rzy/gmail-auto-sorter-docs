# Instalacja i konfiguracja Gmail Auto Sorter

## Wymagania wstępne

Przed rozpoczęciem instalacji upewnij się, że masz zainstalowane następujące komponenty:

- Python 3.7 lub nowszy
- Pip (menedżer pakietów Python)
- Konto Google z dostępem do Gmaila

## 1. Przygotowanie środowiska

### Instalacja Pythona

Jeśli nie masz jeszcze zainstalowanego Pythona:

#### Na macOS (z Homebrew):
```bash
brew install python3
```

#### Na Ubuntu/Debian:
```bash
sudo apt-get install python3 python3-pip
```

#### Na Windows:
Pobierz i zainstaluj Python z [oficjalnej strony](https://www.python.org/downloads/).

### Sprawdzenie wersji Pythona

```bash
python3 --version
```

Upewnij się, że wyświetlana wersja to 3.7 lub nowsza.

## 2. Pobranie repozytorium

```bash
git clone https://github.com/krzyk/gmail-auto-sorter.git
cd gmail-auto-sorter
```

Alternatywnie, możesz pobrać kod jako plik ZIP z [GitHuba](https://github.com/krzyk/gmail-auto-sorter) i rozpakować go.

## 3. Instalacja zależności

Zainstaluj wymagane biblioteki Python:

```bash
pip3 install google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

## 4. Konfiguracja API Google

Aby korzystać z API Gmail, musisz skonfigurować projekt w Google Cloud Platform:

1. Przejdź do [Google Cloud Console](https://console.cloud.google.com/)
2. Utwórz nowy projekt (np. "Gmail Auto Sorter")
3. Włącz API Gmail dla swojego projektu:
   - Przejdź do "APIs & Services" > "Library"
   - Wyszukaj "Gmail API" i włącz je

4. Skonfiguruj ekran zgody OAuth:
   - Przejdź do "APIs & Services" > "OAuth consent screen"
   - Wybierz typ użytkownika (External)
   - Wprowadź wymagane informacje (nazwa aplikacji, email)
   - Dodaj następujące zakresy uprawnień:
     - https://www.googleapis.com/auth/gmail.modify
     - https://www.googleapis.com/auth/gmail.settings.basic
     - https://www.googleapis.com/auth/gmail.labels
   - Dodaj swój adres email jako użytkownika testowego

5. Utwórz dane uwierzytelniające:
   - Przejdź do "APIs & Services" > "Credentials"
   - Kliknij "Create Credentials" > "OAuth client ID"
   - Wybierz typ aplikacji: "Desktop app"
   - Nadaj nazwę (np. "Gmail Auto Sorter Client")
   - Pobierz plik JSON z danymi uwierzytelniającymi

6. Umieść plik w katalogu projektu:
   - Zmień nazwę pobranego pliku na `credentials.json`
   - Umieść go w katalogu `tokens` wewnątrz projektu

```bash
mkdir -p tokens
mv ~/Downloads/client_secret_*.json tokens/credentials.json
```

## 5. Inicjalizacja aplikacji

Przy pierwszym uruchomieniu skryptu zostaniesz przekierowany do przeglądarki, aby zalogować się na swoje konto Google i udzielić aplikacji wymaganych uprawnień:

```bash
python3 setup_gmail_api.py
```

Po zalogowaniu i zaakceptowaniu uprawnień, token uwierzytelniający zostanie zapisany w katalogu `tokens` jako `gmail_modify_token.pickle`.

## 6. Konfiguracja automatycznego uruchamiania (opcjonalnie)

Aby skonfigurować automatyczne uruchamianie sortera co godzinę, użyj skryptu setup_cron.sh:

```bash
chmod +x scripts/setup_cron.sh
./scripts/setup_cron.sh
```

Po wykonaniu tej operacji zadanie cron będzie uruchamiać skrypt direct_sort.py co godzinę.

## 7. Weryfikacja instalacji

Sprawdź, czy wszystko działa poprawnie, uruchamiając bezpośrednie sortowanie:

```bash
python3 direct_sort.py
```

Jeśli skrypt zakończy się powodzeniem, powinieneś zobaczyć komunikat o pomyślnym zakończeniu sortowania oraz nowe etykiety w swoim koncie Gmail.

## Następne kroki

Po zakończeniu instalacji zapoznaj się z [przewodnikiem użytkownika](USAGE.md), aby dowiedzieć się, jak skutecznie korzystać z Gmail Auto Sorter.

W przypadku problemów z instalacją, zapoznaj się z sekcją [rozwiązywania problemów](TROUBLESHOOTING.md).