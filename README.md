# Romer - Społeczność absolwentów i uczniów

Nieoficjalna platforma dla społeczności związanej z I Liceum Ogólnokształcącego im. Eugeniusza Romera w Rabce-Zdroju, gdzie użytkownicy mogą rejestrować się, logować i publikować swoje historie ze zdjęciami.

## Funkcje

- Rejestracja i logowanie użytkowników
- Publikowanie historii z formatowaniem tekstu (pogrubienie, kursywa, listy itp.)
- Możliwość dodawania zdjęć do historii
- Wyświetlanie wszystkich historii
- Profil użytkownika z listą jego opublikowanych historii

## Technologie

- HTML, CSS, JavaScript
- Firebase (uwierzytelnianie, baza danych Firestore, przechowywanie plików)
- GitHub Pages (hosting)

## Konfiguracja i wdrożenie

### 1. Klonowanie repozytorium

```bash
git clone https://github.com/twoj-username/romer.git
cd romer
```

### 2. Konfiguracja Firebase

1. Utwórz nowy projekt na [Firebase Console](https://console.firebase.google.com/)
2. Aktywuj w projekcie:
   - Authentication (z metodą Email/Password)
   - Firestore Database
   - Storage

3. Znajdź ustawienia projektu i skopiuj dane konfiguracyjne Firebase

4. Edytuj plik `index.html` i zastąp obiekt `firebaseConfig` swoimi danymi:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

5. Skonfiguruj reguły bezpieczeństwa w Firebase:

Firestore:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /stories/{storyId} {
      allow read: if true;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && request.resource.data.authorId == request.auth.uid;
    }
  }
}
```

Storage:
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{userId}/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### 3. Wdrożenie na GitHub Pages

1. Utwórz nowe repozytorium na GitHubie
2. Prześlij pliki do repozytorium:

```bash
git add .
git commit -m "Pierwsza wersja strony Romer"
git remote add origin https://github.com/twoj-username/romer.git
git push -u origin main
```

3. W ustawieniach repozytorium włącz GitHub Pages:
   - Przejdź do zakładki "Settings"
   - Znajdź sekcję "Pages"
   - Wybierz gałąź (branch) "main" i folder "/" (root)
   - Kliknij "Save"

4. Po kilku minutach strona będzie dostępna pod adresem: `https://twoj-username.github.io/romer/`

## Struktura Projektu

- `index.html` - Główny plik HTML zawierający całą strukturę strony
- `README.md` - Ten plik z instrukcjami

## Uwagi dodatkowe

- Ta strona nie jest oficjalną stroną internetową I LO im. E. Romera w Rabce-Zdroju
- Projekt wykorzystuje Firebase w darmowym planie, który ma pewne ograniczenia dotyczące liczby operacji
- W przypadku większego ruchu na stronie, warto rozważyć upgrade do płatnego planu Firebase

## Licencja

MIT