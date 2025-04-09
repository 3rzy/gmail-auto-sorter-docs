# Gmail Auto Sorter

## Opis

Gmail Auto Sorter to zaawansowany system do automatycznego organizowania skrzynki Gmail. Opracowany, aby rozwiązać problem zatłoczonej skrzynki odbiorczej, program automatycznie sortuje wiadomości do odpowiednich kategorii, oszczędzając czas i poprawiając produktywność.

## Kluczowe funkcje

- **Automatyczne sortowanie** - wiadomości są automatycznie kategoryzowane według domen nadawców
- **Niestandardowe reguły** - możliwość tworzenia własnych reguł sortowania
- **Analiza nadawców** - funkcja identyfikująca najczęstszych nadawców w skrzynce
- **Obsługa dużych skrzynek** - skuteczne radzenie sobie z setkami wiadomości
- **Automatyzacja** - możliwość skonfigurowania automatycznego uruchamiania co godzinę
- **Bezpieczeństwo** - działanie lokalnie bez dostępu osób trzecich do Twojej poczty

## Komponenty systemu

### Główne skrypty

- **direct_sort.py** - najskuteczniejsze narzędzie do bezpośredniego sortowania wiadomości według domeny nadawcy
- **gmail_sorter.py** - podstawowy sorter używający zdefiniowanych reguł z pliku JSON
- **analyze_senders.py** - narzędzie do identyfikacji najczęstszych nadawców

### Narzędzia pomocnicze

- **run_gmail_sorter.sh** - wygodny skrypt do uruchamiania sortowania
- **setup_cron.sh** - konfiguracja automatycznego uruchamiania
- **identify_unfiltered_emails.py** - analiza wiadomości, które nie zostały posortowane

## Rozpoczęcie pracy

Aby rozpocząć korzystanie z Gmail Auto Sorter, zapoznaj się z naszą [dokumentacją instalacyjną](INSTALLATION.md) oraz [przewodnikiem użytkownika](USAGE.md).

## Dokumentacja

- [Instalacja](INSTALLATION.md) - Kompletny przewodnik instalacji i konfiguracji
- [Używanie](USAGE.md) - Instrukcja korzystania z systemu
- [Rozwiązywanie problemów](TROUBLESHOOTING.md) - Pomoc przy typowych problemach
- [Awaryjne czyszczenie skrzynki](EMERGENCY.md) - Szybkie rozwiązanie dla przepełnionej skrzynki
- [Przykłady](EXAMPLES.md) - Praktyczne scenariusze użycia

## Wymagania

- Python 3.7 lub nowszy
- Biblioteki: google-api-python-client, google-auth-httplib2, google-auth-oauthlib
- Konto Google z dostępem do API Gmail

## Licencja

Ten projekt jest udostępniany na licencji MIT. Szczegóły znajdziesz w pliku [LICENSE](LICENSE).

## Autor

Krzysztof - [krzyk](https://github.com/krzyk)