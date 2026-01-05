# Google_Recognition_API

## Zakaj ta API
**Google Activity Recognition API** omogoča aplikacijam zaznavanje uporabnikove fizične aktivnosti (npr. hoja, tek, kolesarjenje, vožnja z avtom) brez neposredne uporabe GPS-a. API je del **:contentReference[oaicite:0]{index=0}** in uporablja podatke senzorjev mobilne naprave, kot so pospeškomer in žiroskop.

API je primeren za:
- aplikacije za spremljanje telesne aktivnosti,
- zdravstvene in fitnes aplikacije,
- pametne mestne rešitve,
- aplikacije, ki se prilagajajo uporabnikovemu gibanju.

---

## Prednosti
- Nizka poraba energije v primerjavi z uporabo GPS-a  
- Samodejno zaznavanje aktivnosti brez uporabniškega vnosa  
- Enostavna integracija v Android aplikacije  
- Delovanje v ozadju  
- Visoka zanesljivost zaradi Googlovih modelov strojnega učenja  

---

## Slabosti
- API je na voljo predvsem na Android platformi  
- Manj natančen pri določanju lokacije kot GPS  
- Odvisnost od Google ekosistema  
- Omejene možnosti prilagajanja (ni učenja lastnih modelov)  
- Možne napake pri zelo podobnih aktivnostih (npr. stanje in počasna hoja)

---

## Licenca
Google Activity Recognition API je na voljo v skladu s pogoji uporabe **Google APIs Terms of Service**.  
Uporaba API-ja je praviloma brezplačna, vendar zahteva:
- Google račun,
- uporabo Google Play services,
- upoštevanje pravil glede zasebnosti uporabnikov.

---

## Ocenitev števila uporabnikov ter časovne in prostorske zahtevnosti
- **Število uporabnikov**: API se uporablja v velikem številu Android aplikacij in deluje na milijonih naprav po svetu.  
- **Časovna zahtevnost**: zaznavanje aktivnosti poteka skoraj v realnem času z majhnimi časovnimi zamiki.  
- **Prostorska zahtevnost**: nizka, saj se obdelava podatkov izvaja lokalno na napravi in ne zahteva večjih količin pomnilnika.

---

## Vzdrževanje
Vzdrževanje API-ja izvaja Google:
- redne posodobitve in izboljšave natančnosti,
- posodobitve modelov strojnega učenja,
- razvijalec mora skrbeti za posodobitve aplikacije in združljivost z novejšimi verzijami Androida.
