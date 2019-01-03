[English](./README.md) | [繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md)

# API λίστα ελέγχου ασφαλείας
Λίστα με τα πιο σημαντικά μέτρα ασφαλείας στον σχεδιασμό, έλεγχο, και την έκδοση του API σας.


---

## Επικύρωση ασφαλείας (Authentication)
- [ ] Μη χρησιμοποιήτε `Basic Auth`. Χρησιμοποιήστε standard authentication (π.χ. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Μην προσπαθήσετε να επανεφεύρετε τον τροχό για `Authentication`, `token generation`, `password storage`. Χρησιμοποιήστε ήδη υπάρχων βιβλιοθήκες.
- [ ] Χρησιμοποιήστε `Max Retry` και jail features κατά τη σύνδεση (Login).
- [ ] Χρησιμοποιήστε κρυπτογράφηση (encryption) για όλα τα σημαντικά δεδομένα.

### JWT (JSON Web Token)
- [ ] Χρησιμοποιήστε τυχαίο περίπλοκο κλειδί (`JWT Secret`) για να γίνει αρκετά δύσκολο να αποκρυπτογραφηθεί με brute forcing.
- [ ] Μη χρησιμοποιήτε/αφαιρήτε τον αλγόριθμο απο το payload. Ο αλγόριθμος πρέπει να πραγματοποιήτε στο backend (`HS256` or `RS256`).
- [ ] Κάντε το token να λήγει (token expiration) (`TTL`, `RTTL`) όσο πιο σύντομα γίνεται.
- [ ] Μη καταχωρείτε ευαίσθητα δεδομένα στο JWT payload, μπορεί να αποκρυπτογραφηθεί εύκολα [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] Πάντα να επαληθεύετε το `redirect_uri` στο server-side και επιτρέπετε μόνο whitelisted URLs.
- [ ] Πάντα να προσπαθήτε να ανταλλάσετε auth code και όχι tokens (μην επιτρέπετε `response_type=token`).
- [ ] Χρησιμοποιήστε `state` παράμετρο με τυχαίο περίπλοκο κλειδί (hash) για να αποτρέψετε CSRF κατα τη διάρκεια της OAuth authentication διαδικασίας.
- [ ] Ορίστε το προεπιλεγμένο πεδίο (default scope), και επικυρώστε τις παραμέτρους πεδίου (scope parameters) για κάθε εφαρμογή.

## Πρόσβαση (Access)
- [ ] Περιορίστε τα αιτήματα (requests) (Throttling) για να αποφύγετε επιθέσεις DDoS / brute-force.
- [ ] Χρησιμοποιήστε HTTPS στο server side για να αποφύγετε επιθέσεις MITM (Man in the Middle Attack).
- [ ] Χρησιμοποιήστε `HSTS` κεφαλίδα (header) με SSL για να αποφύγετε SSL Strip επιθέσεις.

## Είσοδος δεδομένων (Input)
- [ ] Χρησιμοποιήστε την κατάλληλη HTTP μέθοδο σύμφωνα με τη λειτουργία που χρειάζεστε: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, και `DELETE (για διαγραφή αρχείου)`, και απαντήστε με `405 Method Not Allowed` εάν η ζητούμενη μέθοδος δεν είναι κατάλληλη για την αιτούμενη εφαρμογή.
- [ ] Επικυρώστε `content-type` στη ζητούμενη Accept κεφαλίδα (Content Negotiation) για να επιτρέψετε μόνο το format που υποστηρίζετε (π.χ. `application/xml`, `application/json`, κτλ.) και απαντήστε με `406 Not Acceptable` εάν δεν το υποστηρίζετε.
- [ ] Επικυρώστε `content-type` δεδομένα που στέλνετε, με τον ίδιο τρόπο όπως τα δέχεστε (π.χ. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, κτλ.).
- [ ] Επικυρώστε την οποιαδήποτε είσοδο δεδομένων απο τους χρήστες, για να αποφύγετε τα κοινά κενά ασφαλείας (π.χ. `XSS`, `SQL-Injection`, `Remote Code Execution`, κτλ.).
- [ ] Μη χρησιμοποιήτε ευαίσθητα δεδομένα (`credentials`, `Passwords`, `security tokens`, ή `API keys`) στο URL, αλλά χρησιμοποιήστε τη κοινή Authorization κεφαλίδα (standard Authorization header).
- [ ] Χρησιμοποιήστε API Gateway service για να ενεργοποιήσετε caching, Rate Limit policies (π.χ. `Quota`, `Spike Arrest`, ή `Concurrent Rate Limit`) και κάντε deploy APIs resources δυναμικά.

## Επεξεργασία (Processing)
- [ ] Ελέγξτε ότι όλα τα endpoints είναι προστατευμένα πίσω από επικύρωση ασφαλείας(authentication) για να αποφύγετε προβλήματα λανθασμένης επικύρωσης (broken authentication process).
- [ ] Μη χρησιμοποιήτε το ID των χρηστών. Χρησιμοποιήστε `/me/orders` αντί `/user/654321/orders`.
- [ ] Μη χρησιμοποιήτε την αυτόματη αύξηση των IDs. Χρησιμοποιήστε `UUID` αντι αυτου.
- [ ] Εάν επεργάζεστε XML αρχεία, σιγουρευτείτε ότι το entity parsing δεν είναι ενεργοποιημένο, για να αποφύγετε `XXE` (επίθεση XML external entity).
- [ ] Εάν επεργάζεστε XML αρχεία, σιγουρευτείτε ότι το entity expansion δεν είναι ενεργοποιημένο, για να αποφύγετε `Billion Laughs/XML bomb` δια μέσου exponential entity expansion επίθεσης.
- [ ] Χρησιμοποιήστε CDN για την φόρτωση αρχείων (file uploads).
- [ ] Εάν επεξεργάζεστε μεγάλο αριθμο δεδομένων, χρησιμοποιήστε Workers και Queues για να γίνετε η επεξεργασία στο background και να γίνεται η επιστροφή απάντησης πολύ πιο γρήγορα, αποφεύγοντας HTTP Blocking.
- [ ] Μην ξεχνάτε να απενεργοποιήσετε το DEBUG mode.

## Αποστολή/Επιστροφή δεδομένων (Output)
- [ ] Αποστέλετε `X-Content-Type-Options: nosniff` κεφαλίδα (header).
- [ ] Αποστέλετε `X-Frame-Options: deny` κεφαλίδα (header).
- [ ] Αποστέλετε `Content-Security-Policy: default-src 'none'` κεφαλίδα (header).
- [ ] Αφαιρέστε fingerprinting κεφαλίδεs (headers) - `X-Powered-By`, `Server`, `X-AspNet-Version`, κτλ.
- [ ] Εξαναγκάστε το `content-type` να υπάρχει στην απάντηση (response), εάν η απάντηση είναι `application/json` τότε η απάντηση `content-type` πρέπει να είναι `application/json`.
- [ ] Μην επιστρέφετε ευαίσθητα δεδομένα, όπως: `credentials`, `Passwords`, ή `security tokens`.
- [ ] Επιστρέψτε τον κατάλληλο κωδικό κατάστασης σύμφωνα με τη διαδικασία που ολοκληρώθηκε. (π.χ. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, κτλ.).

## CI & CD
- [ ] Ελέγξτε το σχεδιασμό και την κατάσταση της εφαρμογή σας με επαρκή κάλυψη τεστ Unit / integration.
- [ ] Χρησιμοποιήτε code review διαδικασίες και μη δέχεστε self-approval απο την ομάδα.
- [ ] Εξασφαλίστε ότι όλα τα στοιχέια των υπηρεσιών σας περνούν απο στατικό έλεγχο με AV software πριν τα αναρτήσετε στο production, συμπεριλαμβανομένου οποιασδήποτε εξωτερικής βιβλιοθήκης που μπορει να χρησιμοποιήτε.
- [ ] Σχεδιάστε rollback διαδικασίες για deployments.


---

## Δείτε επίσης:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Λίστα με χρήσιμες πληροφορίες για τον σχεδιασμό RESTful HTTP+JSON APIs.


---

# Συνεισφορά
Μη διστάσετε να συμβάλλετε με το να κάνετε forking αυτό το repository, κάνοντας αλλαγές και υποβάλλοντας pull requests. Για οποιεσδήποτε ερωτήσεις στείλτε μας ένα email στο `team@shieldfy.io`.
