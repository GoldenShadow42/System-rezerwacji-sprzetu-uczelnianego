Specyfikacja Wymagań Systemu (SRS)
System Rezerwacji Sprzętu Uczelnianego (SRSU)
Wersja: 1.0
Data: 19.10.2025 

Jednostka: Wydział Informatyki, WSPA 
Prowadzący: mgr Wojciech Moniuszko 
Opis krótki: Aplikacja webowa do rezerwacji i zarządzania sprzętem (np. laptopy, kamery, projektory, mikrofony) z obsługą statusów rezerwacji, powiadomień i potwierdzania wydania/zwrotu 
________________________________________
1. Wprowadzenie
1.1 Cel dokumentu
Dokument definiuje wymagania funkcjonalne i niefunkcjonalne dla systemu SRSU. Stanowi podstawę do implementacji, testów oraz oceny zgodności gotowego rozwiązania z oczekiwaniami.
1.2 Zakres systemu
System zapewnia:
•	katalog sprzętu i informacje o zasobach,
•	proces rezerwacji na wybrany termin,
•	workflow zatwierdzania (dla wskazanych typów sprzętu),
•	powiadomienia e-mail,
•	obsługę wydania i zwrotu sprzętu,
•	raportowanie i eksport danych 

1.3 Definicje i skróty
•	SRSU – System Rezerwacji Sprzętu Uczelnianego
•	Zasób / sprzęt – element możliwy do wypożyczenia (np. laptop)
•	Rezerwacja – zapis okresu i celu użycia zasobu
•	Opiekun/technik – rola zatwierdzająca i obsługująca wydanie/zwrot 
•	Administrator – rola utrzymaniowa (konfiguracja systemu i słowników; bez operacyjnego udziału w procesach rezerwacji)
1.4 Odbiorcy dokumentu
Zespół projektowy, prowadzący, osoby testujące i oceniające.
________________________________________
2. Cel i uzasadnienie projektu
2.1 Cel biznesowy
Celem jest wdrożenie uczelnianego prototypu systemu, który usprawnia i porządkuje rezerwację sprzętu poza salami dydaktycznymi, eliminując procesy oparte o e-mail/papier 
2.2 Uzasadnienie
System ma ograniczyć problemy: brak kontroli dostępności, zagubienie informacji, opóźnienia w wydaniu/zwrocie, zwiększone obciążenie personelu 
________________________________________
3. Kontekst systemu i założenia
3.1 Perspektywa systemu
System jest aplikacją webową uruchamianą w przeglądarce, z bazą danych (MySQL) i warstwą backend oraz frontend (HTML/CSS/JS) 
3.2 Role użytkowników
•	Użytkownik (student/pracownik): wyszukiwanie sprzętu, tworzenie i zarządzanie rezerwacjami.
•	Opiekun/technik: zatwierdzanie/odrzucanie rezerwacji, zarządzanie dostępnością sprzętu, potwierdzanie wydania i zwrotu 
•	Administrator: administracja systemem (konfiguracja, słowniki, ewentualnie konta), bez codziennej obsługi wypożyczeń.
3.3 Założenia biznesowe
•	dane początkowe o sprzęcie i opiekunach są dostarczane przez uczelnię,
•	zasady (np. czy sprzęt wymaga zatwierdzenia, maks. długość rezerwacji) są konfigurowalne 
•  system jest prototypem (bez produkcyjnego SLA na start) 
3.4 Założenia użytkownika
•	użytkownicy potrafią korzystać z przeglądarki i e-mail,
•	użytkownik ma konto uczelniane lub konto utworzone przez administratora,
•	odbiór sprzętu odbywa się w punkcie wydania (magazyn) 
3.5 Poza zakresem
•	integracja z zewnętrznymi systemami inwentaryzacji,
•	natywna aplikacja mobilna 
________________________________________
4. Wymagania funkcjonalne (FR)
Konwencja:
•	FR-xx – identyfikator wymagania
•	Priorytet: Wysoki / Średni / Niski
•	Weryfikacja: test funkcjonalny / inspekcja / test integracyjny
•	Kryteria akceptacji – mierzalne warunki zaliczenia
________________________________________
4.1 Uwierzytelnianie i role
FR-01 Logowanie
•	Aktorzy: Użytkownik, Opiekun/technik, Administrator
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać logowanie użytkowników (studenci, pracownicy, administratorzy) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Poprawne dane → dostęp do systemu.
2.	Błędne dane → komunikat błędu, brak dostępu.
FR-02 Autoryzacja wg roli
•	Aktorzy: System
•	Priorytet: Wysoki
•	Opis: System musi ograniczać dostęp do funkcji zgodnie z rolą (Użytkownik, Opiekun/technik, Administrator) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Użytkownik nie ma dostępu do ekranów opiekuna/administratora.
2.	Opiekun widzi funkcje zatwierdzania i wydania/zwrotu.
3.	Administrator widzi wyłącznie funkcje administracji systemem.
FR-03 Profil użytkownika
•	Aktorzy: Użytkownik
•	Priorytet: Średni
•	Opis: System powinien umożliwiać podgląd i edycję podstawowych danych profilu (np. e-mail, telefon) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Zmiana danych profilu jest zapisywana i widoczna po odświeżeniu.
________________________________________
4.2 Katalog sprzętu
FR-04 Zarządzanie pozycjami sprzętu (CRUD)
•	Aktorzy: Opiekun/technik (główny), Administrator (opcjonalnie utrzymaniowo)
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać dodawanie, edycję i usuwanie pozycji sprzętu przez uprawnione role 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Nowa pozycja pojawia się w katalogu.
2.	Edycja zmienia dane w widoku szczegółów.
3.	Usunięcie powoduje brak pozycji na liście.
FR-05 Dane sprzętu
•	Aktorzy: System
•	Priorytet: Wysoki
•	Opis: System musi przechowywać: nazwę, kategorię, lokalizację, opis, (opcjonalnie) zdjęcie, liczbę egzemplarzy, opiekuna zasobu, status (sprawny/w naprawie/wycofany) 
•	Weryfikacja: inspekcja + test funkcjonalny
•	Kryteria akceptacji:
1.	Każda pozycja ma komplet pól wymaganych.
2.	Status wpływa na możliwość rezerwacji.
FR-06 Czasowa niedostępność
•	Aktorzy: Opiekun/technik
•	Priorytet: Średni
•	Opis: System musi umożliwiać oznaczenie sprzętu jako czasowo niedostępnego (np. serwis, inwentaryzacja) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Sprzęt oznaczony jako niedostępny nie może zostać zarezerwowany w tym okresie.
________________________________________
4.3 Wyszukiwanie i przeglądanie
FR-07 Wyszukiwanie sprzętu
•	Aktorzy: Użytkownik
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać wyszukiwanie po nazwie, kategorii i lokalizacji 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Wyszukiwanie po nazwie zwraca dopasowania.
2.	Filtry kategorii i lokalizacji zawężają wyniki.
FR-08 Filtrowanie po dostępności w terminie
•	Aktorzy: Użytkownik
•	Priorytet: Średni
•	Opis: System powinien umożliwiać filtrowanie sprzętu po dostępności w podanym terminie 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Sprzęt zajęty w terminie nie jest prezentowany jako dostępny.
FR-09 Kalendarz dostępności
•	Aktorzy: Użytkownik
•	Priorytet: Średni
•	Opis: System musi prezentować kalendarz dostępności sprzętu (dzienny/tygodniowy) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Widok pokazuje zajęte i wolne przedziały.
________________________________________
4.4 Rezerwacje
FR-10 Utworzenie rezerwacji
•	Aktorzy: Użytkownik
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać rezerwację: wybór sprzętu (lub grupy), termin od–do (data i godziny), cel (np. zajęcia/wydarzenie/projekt) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Rezerwacja jest zapisana i widoczna w historii.
2.	Termin i cel rezerwacji są zapisane zgodnie z wyborem.
FR-11 Walidacja rezerwacji
•	Aktorzy: System
•	Priorytet: Wysoki
•	Opis: System musi walidować: brak konfliktu z istniejącymi rezerwacjami, dostępność liczby egzemplarzy, spełnienie reguł (np. max czas, minimalne wyprzedzenie) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Konflikt → rezerwacja nie zostaje zapisana, komunikat dla użytkownika.
2.	Brak egzemplarzy → rezerwacja nie zostaje zapisana, komunikat.
FR-12 Edycja i anulowanie
•	Aktorzy: Użytkownik
•	Priorytet: Średni
•	Opis: System musi umożliwiać edycję/anulowanie rezerwacji do określonego momentu (konfigurowalne) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Przed limitem – edycja/anulowanie działa.
2.	Po limicie – system blokuje operację i komunikuje powód.
FR-13 Rezerwacje cykliczne (opcjonalne)
•	Aktorzy: Użytkownik
•	Priorytet: Niski
•	Opis: System powinien umożliwiać rezerwacje cykliczne (np. co tydzień) jeśli wymagane 
•	Weryfikacja: test funkcjonalny (jeśli wdrożone)
________________________________________
4.5 Zatwierdzanie (workflow)
FR-14 Konfiguracja zatwierdzania
•	Aktorzy: Opiekun/technik, Administrator (utrzymaniowo)
•	Priorytet: Średni
•	Opis: System musi pozwalać zdefiniować, czy dany typ sprzętu wymaga zatwierdzenia 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Dla sprzętu „wymaga zatwierdzenia” rezerwacja otrzymuje status oczekujący.
2.	Dla sprzętu „bez zatwierdzenia” rezerwacja przechodzi dalej automatycznie.
FR-15 Zatwierdzenie i odrzucenie
•	Aktorzy: Opiekun/technik
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać: zatwierdzenie, odrzucenie z podaniem powodu oraz automatyczne zatwierdzanie w prostych przypadkach 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Odrzucenie wymaga podania powodu.
2.	Zatwierdzenie zmienia status i jest widoczne dla użytkownika.
FR-16 Statusy rezerwacji
•	Aktorzy: System
•	Priorytet: Wysoki
•	Opis: System musi obsługiwać statusy: oczekująca, zatwierdzona, odrzucona, zakończona 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Status jest widoczny na szczegółach rezerwacji i w historii.
________________________________________
4.6 Powiadomienia
FR-17 Powiadomienia e-mail
•	Aktorzy: System
•	Priorytet: Średni
•	Opis: System musi wysyłać powiadomienia e-mail o: utworzeniu rezerwacji, zmianie statusu, zbliżającym się terminie odbioru/zwrotu oraz przekroczeniu terminu zwrotu 
•	Weryfikacja: test integracyjny
•	Kryteria akceptacji:
1.	Użytkownik otrzymuje e-mail po utworzeniu rezerwacji.
2.	Użytkownik otrzymuje e-mail po zmianie statusu.
FR-18 Szablony i częstotliwość (opcjonalne)
•	Aktorzy: Administrator (utrzymaniowo)
•	Priorytet: Niski
•	Opis: System powinien umożliwiać konfigurację szablonów i częstotliwości przypomnień 
•	Weryfikacja: test funkcjonalny (jeśli wdrożone)
________________________________________
4.7 Wydanie i zwrot sprzętu
FR-19 Potwierdzenie wydania
•	Aktorzy: Opiekun/technik / pracownik punktu wydania
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać potwierdzenie wydania sprzętu (np. akcja „Wydano”) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	System zapisuje datę i osobę potwierdzającą wydanie.
2.	Rezerwacja zmienia stan na odpowiadający „w trakcie realizacji”.
FR-20 Potwierdzenie zwrotu
•	Aktorzy: Opiekun/technik / pracownik punktu wydania
•	Priorytet: Wysoki
•	Opis: System musi umożliwiać potwierdzenie zwrotu sprzętu
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	System zapisuje datę zwrotu.
2.	Rezerwacja otrzymuje status „zakończona”.
FR-21 Uwagi przy zwrocie (opcjonalne)
•	Aktorzy: Opiekun/technik
•	Priorytet: Niski
•	Opis: System powinien umożliwiać dodanie uwag o stanie sprzętu przy zwrocie (uszkodzenia, brak akcesoriów, uwagi) 
•	Weryfikacja: test funkcjonalny 
________________________________________
4.8 Raportowanie
FR-22 Raporty
•	Aktorzy: Opiekun/technik, Administrator (wgląd)
•	Priorytet: Średni
•	Opis: System musi umożliwiać generowanie raportów (najczęściej rezerwowany sprzęt, obciążenie okresów, historia użytkownika, historia usterek) 
•	Weryfikacja: test funkcjonalny
•	Kryteria akceptacji:
1.	Raport generuje dane dla wybranego okresu.
FR-23 Eksport (opcjonalne)
•	Aktorzy: Opiekun/technik, Administrator
•	Priorytet: Niski
•	Opis: System powinien umożliwiać eksport danych do CSV/XLSX 

•	Weryfikacja: test funkcjonalny (jeśli wdrożone)
________________________________________
5. Wymagania niefunkcjonalne (NFR)
Konwencja: NFR-xx + metryka i weryfikacja.
5.1 Użyteczność i UI/UX
NFR-01 Responsywność
•	Metryka: poprawne działanie na desktop/tablet/telefon
•	Weryfikacja: testy UI
•	Treść: Interfejs musi być responsywny i dostępny w przeglądarce 

NFR-02 Intuicyjność
•	Metryka: użytkownik wykonuje rezerwację bez instrukcji w rozsądnym czasie
•	Weryfikacja: testy użyteczności / inspekcja
•	Treść: System powinien być intuicyjny, komunikaty błędów jasne 

NFR-03 Spójna terminologia
•	Metryka: jednolite nazwy w UI i dokumentacji
•	Weryfikacja: inspekcja
•	Treść: Terminologia zgodna z uczelnią („rezerwacja”, „wypożyczenie”, „magazyn”) 

________________________________________
5.2 Wydajność
NFR-04 Czas odpowiedzi
•	Metryka: 2–3 s dla typowych operacji
•	Weryfikacja: testy wydajności
•	Treść: Typowe operacje nie powinny przekraczać 2–3 sekund przy typowym obciążeniu 

NFR-05 Równoczesna praca
•	Metryka: ≥200 równoczesnych użytkowników
•	Weryfikacja: test obciążeniowy
•	Treść: System ma obsługiwać co najmniej 200 użytkowników równocześnie 
 ________________________________________
5.3 Niezawodność i kopie zapasowe
NFR-06 Backup
•	Metryka: codzienna kopia + możliwość odtworzenia
•	Weryfikacja: test odtwarzania
•	Treść: System musi posiadać mechanizmy backupu i odtwarzania danych 
NFR-07 Trwałość danych rezerwacji
•	Metryka: brak utraty danych po odtworzeniu z kopii
•	Weryfikacja: testy awaryjne
•	Treść: Awaria nie może powodować utraty zarejestrowanych rezerwacji po przywróceniu 
________________________________________
5.4 Bezpieczeństwo
NFR-08 HTTPS/TLS
•	Metryka: brak dostępu po HTTP / wymuszenie HTTPS
•	Weryfikacja: inspekcja konfiguracji
•	Treść: System musi zapewniać szyfrowanie transmisji (HTTPS/TLS) 

NFR-09 Uwierzytelnienie dostępu
•	Metryka: brak dostępu do danych i rezerwacji bez logowania
•	Weryfikacja: test funkcjonalny
•	Treść: System musi wymagać uwierzytelnienia przed dostępem do funkcji rezerwacji i danych użytkowników 

NFR-10 Ograniczenie funkcji administracyjnych
•	Metryka: funkcje admin niewidoczne dla innych ról
•	Weryfikacja: test funkcjonalny
•	Treść: Dostęp do funkcji administracyjnych musi być ograniczony do wybranych ról 

NFR-11 Logi audytowe
•	Metryka: rejestrowanie logowań i zmian w rezerwacjach/sprzęcie
•	Weryfikacja: inspekcja + testy
•	Treść: System musi prowadzić logi operacji istotnych: logowania, CRUD rezerwacji, modyfikacje sprzętu 

NFR-12 RODO
•	Metryka: minimalizacja danych, możliwość korekty danych
•	Weryfikacja: inspekcja
•	Treść: System musi spełniać wymagania ochrony danych osobowych (RODO) 

________________________________________
5.5 Skalowalność i utrzymywalność
NFR-13 Skalowalność
•	Metryka: możliwość zwiększenia zasobów bez zmian w logice aplikacji
•	Weryfikacja: inspekcja architektury
•	Treść: Architektura powinna umożliwiać skalowanie serwera i bazy danych 

NFR-14 Modułowość i rozwój
•	Metryka: kod udokumentowany, modułowa struktura
•	Weryfikacja: inspekcja repozytorium
•	Treść: Kod powinien być udokumentowany, system modułowy, gotowy do rozwoju 

________________________________________
5.6 Zgodność i język
NFR-15 Przeglądarki
•	Metryka: Chrome/Firefox/Edge/Safari
•	Weryfikacja: testy kompatybilności
•	Treść: System musi działać na popularnych przeglądarkach 

NFR-16 Lokalizacja
•	Metryka: PL jako minimum + możliwość dodania EN
•	Weryfikacja: inspekcja
•	Treść: System musi obsługiwać język polski i umożliwiać łatwe dodanie kolejnych języków 

________________________________________
6. Interfejsy i architektura 
6.1 Interfejs użytkownika
Responsywny frontend w przeglądarce (HTML5/CSS3/JS) 

6.2 API (wewnętrzne)
REST API dla operacji na sprzęcie i rezerwacjach; autoryzacja sesyjna / tokenowa (do ustalenia) 

Przykład endpointu: GET /api/equipment/{id} zwraca szczegóły sprzętu wraz z dostępnością 
________________________________________
7. Model danych (opis tekstowy)
Encje:
•	User (id, email, phone, role, active)
•	Equipment (id, name, category, location, description, qty_total, qty_available, status, manager_user_id)
•	Reservation (id, user_id, equip_id, start, end, qty, status, purpose, created_at)
•	Maintenance/History (id, equip_id, date, type, notes, reported_by)
•	NotificationTemplate (id, name, subject, body)
•	AuditLog (id, user_id, action, target_type, target_id, timestamp, details)
Zgodne z częścią „Model danych / ERD” w Twoim dokumencie 

________________________________________
8. Organizacja projektu
8.1 Zespół i role
Role oraz odpowiedzialności zgodne z dokumentem: kierownik, analityk, programista, tester, dokumentalista 
8.2 Narzędzia
Jira, GitHub, Miro, Visual Studio, MySQL, MS Word, Discord 
8.3 Harmonogram
Etapy Tydzień 1–10: analiza wymagań, UML, konfiguracja, implementacja prototypu, testy, dokumentacja końcowa i prezentacja 
8.4 Analiza ryzyka
Ryzyka i działania: opóźnienia, brak doświadczenia w PHP/JS, integracja front-back, utrata danych, dokumentacja 
________________________________________
9. Kryteria sukcesu i akceptacja
9.1 Kryteria sukcesu
•	wymagania funkcjonalne zaimplementowane i przetestowane,
•	system działa w przeglądarce bez krytycznych błędów,
•	użytkownik tworzy rezerwację i widzi historię,
•	dokumentacja zawiera modele UML/ERD,
•	projekt ukończony i zaakceptowany 

