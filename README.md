# Instrukcja instalacji skryptu Ceneo
## Podstawowa wersja skryptu
W podstawowej wersji skryptu należy skopiować i uzupełnić poniższy skrypt.
```
<script>(function(w,d,s,i,dl){w._ceneo = w._ceneo || function () {
w._ceneo.e = w._ceneo.e || []; w._ceneo.e.push(arguments); };
w._ceneo.e = w._ceneo.e || [];dl=dl===undefined?"dataLayer":dl;
const f = d.getElementsByTagName(s)[0], j = d.createElement(s); j.defer = true; j.src = "https://ssl.ceneo.pl/shops" + "/ct.js?accountGuid=" + i + "&t=" + Date.now() + (dl ? "&dl=" + dl : ''); f.parentNode.insertBefore(j, f);
})(window, document, "script", "GUID");</script>
```
Tutaj należy jedynie zmienić napis GUID na Twój indywidualny numer GUID. Tak uzupełniony skrypt należy umieścić pomiędzy tagami `<HEAD>` i `</HEAD>` na każdej stronie sklepu.<br><br>
Dodatkowo, aby zbierać informacje o zamówieniach na **stronie potwierdzenia zamówienia** należy umieścić pomiędzy tagami `<BODY>` i `</BODY>` następujący skrypt.
```
<script>
    _ceneo('transaction', {
       client_email: 'PRZYKLAD@PRZYKLAD.pl',
       order_id: '1PRZYKLAD',
       shop_products: [
       {
            id: '60724',
            price: 123.33,
            quantity: 1,
            currency: 'PLN'
       },
       {
            id: '60726',
            price: 126,
            quantity: 2,
            currency: 'PLN'
       }],
       work_days_to_send_questionnaire: 3,
       amount: 375.33
    });
</script>
```
<br>
W skrypcie tym należy uzupełnić parametry. Szczegóły dotyczące parametrów:

- client_email: e-mail klienta, składającego zamówienie w sklepie. Powinien być zawarty w pojedynczym apostrofie, a cały wiersz zakończony przecinkiem, jak na przykładzie powyżej. Adres jest niezbędnym elementem, dzięki niemu wyślemy klientowi ankietę, zbierającą informacje o przeprowadzonej transakcji.
- order_id: numer zamówienia klienta. Podajemy go również w apostrofach (tak jak jest to przedstawione na przykładzie powyżej), a cały wiersz powinien być zakończony przecinkiem. Numerem zamówienia może być dowolny ciąg cyfr i liter o max. długości 32 znaków.
- shop_products: jest to tablica zakupionych produktów. Każdy produkt ma swoje odzwierciedlenie poprzez następujące parametry:
  - id – sklepowy identyfikator produktu. Powinien być zawarty w pojedynczym apostrofie, a cały wiersz zakończony przecinkiem, jak na przykładzie powyżej.
  - price – cena za sztukę zakupionego produktu. Powinna to być liczba, gdzie separatorem między liczbą całkowitą, a ułamkiem (czyli w przypadku Polski między złotówką, a groszem) jest kropka. W tym parametrze nie używamy apostrofów, a cały wiersz kończymy przecinkiem.
  - quantity – liczba sztuk zakupionego produktu. W tym parametrze nie używamy apostrofów, a cały wiersz kończymy przecinkiem.
  - currency – waluta, w której wyrażona została cena produktu. Powinna być zawarta w pojedynczym apostrofie, mieć 3 znaki (standard ISO-4217), a cały wiersz zakończony przecinkiem.
- work_days_to_send_questionnaire: liczba dni roboczych do wysyłki ankiety, dla danego zamówienia. Parametr musi być liczbą całkowitą w zakresie od 0 do 21. W tym parametrze nie używamy apostrofów, a cały wiersz kończymy przecinkiem.
np. work_days_to_send_questionnaire = 0 lub work_days_to_send_questionnaire = 10 Ten parametr nie jest wymagany. Jeśli go nie podasz, system automatycznie uzupełni go na podstawie średniej wartości dla Twojej kategorii lub w przypadku jej braku domyślną wartością równą 3.
- amount: kwota zapłacona za całość zamówienia. Powinna to być liczba, gdzie separatorem między liczbą całkowitą, a ułamkiem (czyli w przypadku Polski między złotówką a groszem) jest kropka. W tym parametrze nie używamy apostrofów.
## Opcje zaawansowane – ręczne zarządzanie zgodą na śledzenie użytkownika
Skrypt Ceneo pozwala na wyłączenie śledzenia indywidualnego użytkownika dopóki nie zaakceptuje on zgody na posługiwanie się mechanizmami śledzącymi. Jeżeli tego nie zrobi, to wtedy zbierzemy jedynie anonimowe informacje o zakupie jako przejście użytkownika z portalu Ceneo.<br>
Aby uaktywnić ten tryb należy na początku umieścić skrypt główny pomiędzy tagami `<HEAD>`
i `</HEAD>` na każdej stronie sklepu, tak jak zostało to opisane na początku instrukcji.
```
<script>(function(w,d,s,i,dl){w._ceneo = w._ceneo || function () {
w._ceneo.e = w._ceneo.e || []; w._ceneo.e.push(arguments); };
w._ceneo.e = w._ceneo.e || [];dl=dl===undefined?"dataLayer":dl;
const f = d.getElementsByTagName(s)[0], j = d.createElement(s); j.defer = true; j.src = "https://ssl.ceneo.pl/shops" + "/ct.js?accountGuid=" + i + "&t=" + Date.now() + (dl ? "&dl=" + dl : ''); f.parentNode.insertBefore(j, f);
})(window, document, "script", "GUID");</script>
```
A zaraz pod nim należy umieścić skrypt uruchamiający ręczne zarządzanie zgodą:
```
<script>_ceneo('enableManualConsentMaode');</script>
```
Całość powinna wyglądać jak poniżej, oraz powinna znajdować się na każdej stronie sklepu pomiędzy tagami `<HEAD>` i `</HEAD>`:
```
<script>(function(w,d,s,i,dl){w._ceneo = w._ceneo || function () {
w._ceneo.e = w._ceneo.e || []; w._ceneo.e.push(arguments); };
w._ceneo.e = w._ceneo.e || [];dl=dl===undefined?"dataLayer":dl;
const f = d.getElementsByTagName(s)[0], j = d.createElement(s); j.defer = true; j.src = "https://ssl.ceneo.pl/shops" + "/ct.js?accountGuid=" + i + "&t=" + Date.now() + (dl ? "&dl=" + dl : ''); f.parentNode.insertBefore(j, f);
})(window, document, "script", "GUID");</script>
<script>_ceneo('enableManualConsentMode');</script>
```
Następnie gdy użytkownik wyrazi zgodę należy uruchomić następującą funkcję skryptu Ceneo:
```
<script>_ceneo('updateConsentState', {
    allow_tracking: true
});
</script>
```
Uwaga: powyższy skrypt ze zezwoleniem na śledzenie **musi** się również pojawiać na każdej stronie sklepu od momentu zezwolenia przez użytkownika do momentu wycofania tej zgody. Najlepiej aby pojawiał się on pod koniec sekcji HEAD strony (po skrypcie głównym, ale przed skryptem
z potwierdzeniem zamówienia umiejscowionym w sekcji BODY).
## Opcje zaawansowane – zmienna DataLayer
Skrypt Ceneo domyślne czyta również dane umieszczane w tablicy DataLayer wykorzystywanej przez analitykę Google. Udostępnia on także możliwość wyłączenia tego odczytywania lub wykorzystanie tablicy DataLayer o innej nazwie zmiennej. W tym celu należy wykorzystać dodatkowy parametr skryptu zaznaczony poniżej.
```
<script>(function(w,d,s,i,dl){w._ceneo = w._ceneo || function () {
w._ceneo.e = w._ceneo.e || []; w._ceneo.e.push(arguments); };
w._ceneo.e = w._ceneo.e || [];dl=dl===undefined?"dataLayer":dl;
const f = d.getElementsByTagName(s)[0], j = d.createElement(s); j.defer = true; j.src = "https://ssl.ceneo.pl/shops" + "/ct.js?accountGuid=" + i + "&t=" + Date.now() + (dl ? "&dl=" + dl : ''); f.parentNode.insertBefore(j, f);
})(window, document, "script", "GUID", "NAZWA_ZMIENNEJ_DATALAYER");</script>
```
Parametr NAZWA_ZMIENNEJ_DATALAYER może przybrać następujące wartości:
- "" (pusty napis) lub null wtedy czytanie informacji z DataLayer zostaje wyłączone
- Dowolny napis, np. „testLayer” wtedy skrypt zamiast sięgać do tablicy dataLayer będzie próbował wyciągać informacje z testLayer.


