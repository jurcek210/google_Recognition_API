# Google_Recognition_API

## Zakaj ta API
**Google Activity Recognition API** omogoča aplikacijam zaznavanje in razvrščanje uporabnikove fizične aktivnosti, kot so hoja, tek, kolesarjenje, stanje na mestu ali vožnja z avtomobilom, brez potrebe po neposredni uporabi GPS-a. S tem se bistveno zmanjša poraba baterije, kar je še posebej pomembno pri dolgotrajnem delovanju aplikacij v ozadju.

API je del Google Play services in deluje tako, da zbira ter analizira podatke senzorjev mobilne naprave, predvsem pospeškomera in žiroskopa. Na podlagi teh podatkov uporablja Googlove algoritme strojnega učenja za prepoznavanje vzorcev gibanja in določanje verjetnosti posamezne aktivnosti.

Razvijalcem API omogoča, da aplikacije prilagodijo vedenju uporabnika v realnem času, na primer samodejno beleženje telesne aktivnosti, prilagajanje nastavitev aplikacije glede na gibanje ali izboljšanje uporabniške izkušnje brez dodatnega ročnega vnosa.


API je primeren za:
- aplikacije za spremljanje telesne aktivnosti,
- zdravstvene in fitnes aplikacije,
- aplikacije, ki se prilagajajo uporabnikovemu gibanju.

---

## Prednosti
- **Nizka poraba energije**: API ne uporablja GPS-a, temveč senzorje naprave, kar bistveno zmanjša porabo baterije in omogoča dolgotrajno delovanje aplikacij v ozadju.  
- **Samodejno zaznavanje aktivnosti**: uporabniku ni treba ročno izbirati ali potrjevati aktivnosti, saj API samodejno zazna trenutno gibanje uporabnika.  
- **Enostavna integracija**: API je del Google Play services, kar omogoča hitro in enostavno vključitev v Android aplikacije brez zapletene konfiguracije.  
- **Delovanje v ozadju**: aplikacije lahko prejemajo podatke o aktivnosti tudi takrat, ko niso aktivno uporabljene, kar izboljša uporabniško izkušnjo.  
- **Visoka zanesljivost**: API uporablja Googlove modele strojnega učenja, ki se stalno izboljšujejo na podlagi velike količine podatkov, kar zagotavlja dobro natančnost zaznavanja.

---

## Slabosti
- **Omejena platformna podpora**: API je namenjen predvsem Android napravam, kar pomeni, da ga ni mogoče neposredno uporabljati v aplikacijah za druge platforme (npr. iOS).  
- **Omejena natančnost lokacije**: ker API ne uporablja GPS-a, ne omogoča natančnega določanja lokacije, temveč le prepoznavanje vrste aktivnosti.  
- **Odvisnost od Google ekosistema**: uporaba API-ja zahteva Google Play services, kar pomeni odvisnost od Googlovih storitev in njihovih posodobitev.  
- **Omejene možnosti prilagajanja**: razvijalci ne morejo trenirati ali prilagajati lastnih modelov za zaznavanje aktivnosti.  
- **Možne napačne zaznave**: pri zelo podobnih aktivnostih, kot sta stanje na mestu in počasna hoja, lahko pride do manj natančnih rezultatov.


---

## Licenca
Google Activity Recognition API je na voljo v skladu s pogoji uporabe **Google APIs Terms of Service**.  
Uporaba API-ja je praviloma brezplačna, vendar zahteva:
- Google račun,
- uporabo Google Play services,
- upoštevanje pravil glede zasebnosti uporabnikov.

---

## Ocenitev števila uporabnikov
Google Activity Recognition API je del osnovnih storitev Android ekosistema in se uporablja v zelo velikem številu aplikacij, predvsem na področju zdravja, fitnesa in pametnih storitev. Ker je vključen v Google Play services, deluje na milijonih Android naprav po vsem svetu. Zaradi razširjenosti Android operacijskega sistema lahko ocenimo, da API dnevno uporablja zelo veliko število končnih uporabnikov, pogosto ne da bi se ti tega sploh zavedali.

---

## Časovna zahtevnost
Zaznavanje aktivnosti poteka skoraj v realnem času. API periodično obdeluje podatke senzorjev in v kratkih časovnih intervalih posodablja zaznano aktivnost. Časovna zahtevnost je nizka, saj so algoritmi optimizirani za delovanje na mobilnih napravah in ne povzročajo opaznih zakasnitev pri delovanju aplikacije.

---

## Prostorska zahtevnost
Prostorska zahtevnost API-ja je nizka, ker se večina obdelave izvaja lokalno na uporabnikovi napravi. API ne zahteva shranjevanja velikih količin podatkov, temveč uporablja kratkoročne senzorčne podatke za analizo gibanja. Zaradi tega poraba pomnilnika ostaja majhna, kar je primerno tudi za naprave z omejenimi strojni viri.


---

## Vzdrževanje
Vzdrževanje Google Activity Recognition API-ja v celoti izvaja Google, kar razvijalcem bistveno zmanjša potrebo po lastnem vzdrževanju kompleksnih sistemov za zaznavanje aktivnosti. Google redno zagotavlja:
- **posodobitve API-ja**, ki izboljšujejo stabilnost in delovanje,
- **izboljšave natančnosti zaznavanja**, ki temeljijo na novih podatkih in izboljšanih algoritmih,
- **posodobitve modelov strojnega učenja**, brez potrebe po spremembah na strani razvijalca.

---

## Primer uporabe

Za uporabo Google Activity Recognition API so ključni naslednji koraki: pridobitev dovoljenj, inicializacija odjemalca, zahteva za posodobitve in prejemanje ter obdelava rezultatov.

---

### 1. Dovoljenja
Za dostop do podatkov o telesni aktivnosti mora aplikacija imeti ustrezna dovoljenja. Od Android 10 (API 29) dalje mora uporabnik to dovoljenje potrditi tudi med izvajanjem aplikacije (runtime permission).

Primer nastavitve dovoljenj v datoteki `AndroidManifest.xml`:

```xml
<manifest ...>
    <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
</manifest>
```

Brez teh dovoljenj aplikacija ne more dostopati do podatkov senzorjev in zaznavati uporabnikove aktivnosti.

### 2. Inicializacija odjemalca in zahteva za posodobitve
V naslednjem koraku inicializiramo odjemalca za zaznavanje aktivnosti in sistemu sporočimo, kako pogosto želimo prejemati posodobitve.

Primer v razredu ActivityRecognitionManager.kt:

```kotlin
// Inicializacija odjemalca
private val client = ActivityRecognition.getClient(context)

fun startTracking(
    onSuccess: () -> Unit,
    onFailure: (Exception) -> Unit
) {
    client.requestActivityUpdates(
        3000L, // Interval zaznavanja v milisekundah
        pendingIntent // BroadcastReceiver za prejem rezultatov
    ).addOnSuccessListener {
        onSuccess()
    }.addOnFailureListener {
        onFailure(it)
    }
}
```

S tem se aplikacija naroči na redne posodobitve, sistem pa ob zaznani spremembi aktivnosti sproži dogodek.

### 3. Prejemanje in obdelava rezultatov

Ko sistem zazna novo aktivnost, sproži BroadcastReceiver, kjer aplikacija obdela prejete podatke.

Primer razreda ActivityReceiver.kt:


```kotlin
override fun onReceive(context: Context, intent: Intent) {
    if (ActivityRecognitionResult.hasResult(intent)) {
        val result = ActivityRecognitionResult.extractResult(intent)!!

        // Najverjetnejša zaznana aktivnost
        val activity = result.mostProbableActivity

        // Izpis v Logcat
        Log.d(
            "ActivityRecognition",
            "Detected: ${getActivityString(activity.type)} - ${activity.confidence}%"
        )
    }
}
```
API ne vrne le ene same aktivnosti, temveč seznam možnih aktivnosti skupaj z ocenami zaupanja (confidence). Aplikacija običajno uporabi tisto z najvišjo verjetnostjo.

### 4. Primer uporabe (Use Case)
<img width="935" height="351" alt="image" src="https://github.com/user-attachments/assets/054bc496-7918-4f9d-a73f-4364b1d8b7ca" />







