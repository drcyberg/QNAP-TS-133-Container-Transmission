## QNAP TS-133 NAS - Konténer kezelés - Transmission
---

Nem rég vettem egy olcsó de jó minőségű **[QNAP TS-133](https://www.qnap.com/hu-hu/product/ts-133)** NAS eszközt, amit fotók archiválására (**Qimage**), filmek és sorozatok nézéshez (**Plex**), valamint torrent fájlmegosztásra szeretném használni. Sajnos mint ahogy több fórumon is írják és ahogy Én is tapasztaltam, a **Download Station** program nem bizonyul megbízhatónak, visszatöltési problémák vannak.
Szerencsére a QNAP OS (**QTS**) támogatja a konténerek futtatását a **Container Station** program segítségével, így könnyedén letudjuk váltani a **[Transmission](https://hub.docker.com/r/linuxserver/transmission)** alkalmazásra.
Ez a leírás segítséget fog nyújtani abban, hogy hogyan tudjuk könnyedén beállítani és mik azok a lépések, amikkel betudjuk könnyedén konfigurálni.

---
## Előkészület
---

- Töltsük le és telepítsük az alábbi alkalmazásokat az **App Centeren** belül:
- [x] **Container Station**
- [x] **Text Editor**

![](/img/1.jpg)

---
## Konténer és a Transmission alkalmazás beállítása
---

- Nyissuk meg a **Container Station** alkalmazást, majd hozzuk létre a Transmission konténert a konténerek menüpont alatt.

![](/img/2.jpg)

Ekkor egy varázsló fog minket végigvezetni és a következő paramétereket kell megadni:
- **Nyilvántartás:** Docker Hub
- **Kép:** linuxserver/transmission
(Ha konkrét verziót szeretnénk kiválasztani akkor az így tehető meg: linuxserver/transmission:4.0.6)
- [x] Próbálja meg lehívni a képet a nyilvántartásból a konténer létrehozása előtt

![](/img/3.jpg)

- [x] Az újraindítási házirend csak hiba esetén induljon újra maximum 3x
- Majd kattintsunk a speciális beállítások menüpontra

![](/img/4.jpg)

- **Hálózati mód:** Bridge
(NAT beállítás mellett további routing beállítás szükséges)
- A hálózati beállításoknál ha nincs DHCP szerver üzemeltetve a hálózatunkban akkor állítsuk be a statikus IP-cím használata alatti részt.
- **DNS-szerverbeállítások konfigurálása:** Opcionális

![](/img/5.jpg)

- A `/config` tárhely beállításánál fontos, hogy elsőként **RW** módként adjuk meg az elérési útvonalat, mert a Transmission alkalmazás számára szükséges beállításokat és könyvtárakat ő maga fogja elsőként létrehozni.
- A kijelölt útvonalakat csak a HDD lemezen szereplő könyvtárakból lehet kiválasztani.
- A szimbolikus hivatkozások működnek ha esetleg USB meghajtó lenne a célterület.

![](/img/6.jpg)

- Miután sikeresen létrejött a Transmission konténer, utána a `/config` tárterületet **RO** módként át kell állítani, mert mindig a saját beállításit veszi alapul ezzel felülírva a saját beállításainkat. Ezt jobban szeretem Én menedzselni. Módosítani a konténer újbóli létrehozásával lehetséges. 

![](/img/7.jpg)

- Most már készen áll a Transmission alkalmazás a használatra, ami a **TCP/9091** porton web böngészőből meghívva érhető el vagy a **[Transmission Remote GUI](https://github.com/transmission-remote-gui/transgui)** program segítségével. Az utólagos konfiguráció beállításokat mindig a konténer leállításával lehetséges, máskülönben nem veszi figyelembe  (`/config/settings/settings.json`).

![](/img/8.jpg)

- A `/config/settings/settings.json` fájl szerkesztése közvetlenül a **Text Editor** alkalmazással lehetséges.
- **Paraméterek:** [settings.json](https://trac.transmissionbt.com/wiki/MovedToGitHub/EditConfigFiles)

![](/img/9.jpg)

![](/img/10.jpg)

![](/img/11.jpg)

---
## Tipp
---

**Jelszó:**
- A jelszavak feldolgozása mindig SHA1 kriptográfiával történik. A jelszót plain text formában (`"Jelszo"`) és közvetlen SHA1 formában (`"{01baa84e8e80cb590b41765389e2f2c1a4c176cf"`) is meg lehet adni a kapcsos zárójel után a `/config/settings/settings.json` konfigurációs fájlban.
- Online Tools: [LINK](https://timestampgenerator.com/generate-hash/sha1)
