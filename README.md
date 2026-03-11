# nevera_sudstvo
Nevera_Sudstvo — QBCore Pravosudni Sistem
# ⚖️ nevera_sudstvo — QBCore Department of Justice

> Kompletan pravosudni sistem za FiveM QBCore servere  
> Hard RP | Krivična i civila procedura | 35 pravnih formi | React NUI

<p align="center">
  <img src="https://i.imgur.com/24Wk4O5.png" alt="Nevera Army Banner">
</p>

---

## 📋 Sadržaj

- [Pregled](#pregled)
- [Zahtjevi](#zahtjevi)
- [Instalacija](#instalacija)
- [Konfiguracija](#konfiguracija)
- [Struktura projekta](#struktura-projekta)
- [Uloge i permisije](#uloge-i-permisije)
- [Komande](#komande)
- [NUI Build](#nui-build)
- [Baza podataka](#baza-podataka)
- [Integracije](#integracije)

---

## Pregled

`nevera_sudstvo` je potpuni DOJ (Department of Justice) resource koji replicira stvarni pravni sistem unutar FiveM okruženja. Pokriva:

- **Krivičnu proceduru** — od zločina do zatvaranja slučaja
- **Civilnu proceduru** — od tužbe do presude
- **35 digitalnih pravnih formi** identičnih originalnim PDF formama
- **7 sudskih uloga** s različitim permisijama
- **Porota, ročišta, jamčevina, kazne, nalozi za pretres i hapšenje**
- **React NUI panel** — moderni sudski interface unutar igre
- **Revizijski log** — svaka akcija se bilježi

---

## Zahtjevi

| Dependency | Verzija |
|---|---|
| [qb-core](https://github.com/qbcore-framework/qb-core) | latest |
| [oxmysql](https://github.com/overextended/oxmysql) | latest |
| [ox_lib](https://github.com/overextended/ox_lib) | latest |
| [ox_target](https://github.com/overextended/ox_target) | latest |
| [ox_inventory](https://github.com/overextended/ox_inventory) | latest |
| [peleg-billing](https://github.com/PelegCast/peleg-billing) | latest |
| [17mov_Phone](https://github.com/17mov/phone) | latest |
| Node.js | v18+ |

---

## Instalacija

### 1. Kloniraj repozitorij

```bash
git clone https://github.com/tvoj-username/nevera_sudstvo.git
cd nevera_sudstvo
```

### 2. Importaj bazu podataka

```bash
# Kroz HeidiSQL, phpMyAdmin ili direktno:
mysql -u root -p ime_baze < nevera_sudstvo.sql
```

### 3. Build NUI (React panel)

```bash
cd web
npm install
npm run build
cd ..
```

### 4. Dodaj u server.cfg

```
ensure nevera_sudstvo
```

### 5. Restart servera

```
refresh
restart nevera_sudstvo
```

---

## Konfiguracija

### config.lua — osnovne postavke

```lua
Config.Debug         = false        -- debug mod (ispisuje logove)
Config.MaxSlucajeva  = 50           -- maks. aktivnih slučajeva
Config.BailTimeout   = 72           -- sati do isteka jamčevine
Config.NalogTimeout  = 48           -- sati do isteka naloga
Config.MinPorota     = 6            -- minimalno porotnika
Config.MaxPorota     = 12           -- maksimalno porotnika
```

### config_fines.lua — kazne

```lua
Config.Kazne = {
    ubojstvo          = 500000,
    pokusaj_ubojstva  = 250000,
    napad             = 50000,
    -- ...
}
```

---

## Struktura projekta

```
nevera_sudstvo/
├── fxmanifest.lua
├── config.lua
├── config_fines.lua
├── nevera_sudstvo.sql
│
├── shared/
│   ├── enums.lua               # sve enum vrijednosti
│   └── utils.lua               # dijeljeni helperi
│
├── server/
│   ├── main.lua
│   ├── sv_roles.lua            # upravljanje ulogama
│   ├── sv_cases.lua            # slučajevi
│   ├── sv_forms.lua            # pravne forme
│   ├── sv_warrants.lua         # nalozi
│   ├── sv_bail.lua             # jamčevina
│   ├── sv_fines.lua            # kazne
│   ├── sv_hearings.lua         # ročišta
│   ├── sv_jury.lua             # porota
│   ├── sv_timers.lua           # automatski timeri
│   ├── sv_notifications.lua    # SMS/obavijesti
│   └── sv_utils.lua            # server helperi
│
├── client/
│   ├── main.lua
│   ├── cl_keys.lua             # key bindovi
│   ├── cl_target.lua           # ox_target zone
│   ├── cl_menus.lua            # ox_lib meniji
│   ├── cl_nui.lua              # NUI komunikacija
│   └── cl_phone_app.lua        # phone integracija
│
└── web/                        # React NUI panel
    ├── package.json
    ├── vite.config.js
    ├── index.html
    └── src/
        ├── main.jsx
        ├── App.jsx
        ├── hooks/
        │   ├── useNUI.js
        │   └── useDebounce.js
        ├── store/
        │   └── appStore.js
        ├── utils/
        │   ├── helpers.js
        │   └── nui.js
        ├── styles/
        │   ├── global.css
        │   └── components.css
        ├── components/
        │   ├── Layout/
        │   │   ├── Sidebar.jsx
        │   │   ├── Header.jsx
        │   │   └── TabBar.jsx
        │   ├── Dashboard/
        │   │   └── Dashboard.jsx
        │   ├── Cases/
        │   │   ├── CaseList.jsx
        │   │   ├── CaseDetail.jsx
        │   │   ├── CaseTimeline.jsx
        │   │   └── NewCaseModal.jsx
        │   ├── Forms/
        │   │   ├── FormList.jsx
        │   │   ├── FormFill.jsx
        │   │   ├── FormViewer.jsx
        │   │   └── templates/      # 35 formi
        │   ├── Warrants/
        │   │   ├── WarrantList.jsx
        │   │   └── WarrantDetail.jsx
        │   ├── Hearings/
        │   │   ├── HearingCalendar.jsx
        │   │   └── HearingDetail.jsx
        │   ├── Jury/
        │   │   ├── JuryPool.jsx
        │   │   └── JurySelection.jsx
        │   ├── Fines/
        │   │   ├── FineCalculator.jsx
        │   │   └── FineHistory.jsx
        │   └── Admin/
        │       ├── RoleManager.jsx
        │       └── AuditLog.jsx
        └── pages/
            ├── DashboardPage.jsx
            ├── CasesPage.jsx
            ├── FormsPage.jsx
            ├── WarrantsPage.jsx
            ├── HearingsPage.jsx
            ├── FinesPage.jsx
            └── AdminPage.jsx
```

---

## Uloge i permisije

| Uloga | Opis | Permisije |
|---|---|---|
| `vrhovni_sud` | Vrhovni sudac | Sve — admin, uloge, sve forme |
| `sudac` | Sudac | Slučajevi, nalozi, ročišta, porota, kazne |
| `tuzilac` | Tužilac | Slučajevi, nalozi, forme, kazne |
| `branilac` | Advokat (privatni) | Slučajevi, forme, ročišta |
| `javni_branilac` | Javni branilac | Slučajevi, forme, ročišta |
| `policija` | Policija | Nalozi, bail, pregled slučajeva |
| `gradjanin` | Građanin | Pregled vlastitih slučajeva i formi |

### Postavljanje uloge (server konzola)

```
# Nema direktne komande — uloge se postavljaju kroz Admin panel u NUI
# ili direktno u bazi:
UPDATE doj_uloge SET uloga = 'sudac' WHERE citizenid = 'char1:abc123';
```

---

## Komande

### Chat komande (ingame)

| Komanda | Opis | Permisija |
|---|---|---|
| `/sudstvo` | Otvori NUI panel | Sve uloge |
| `/slucaj [id]` | Brzi pregled slučaja | Sve uloge |
| `/nalog [id]` | Provjeri nalog | Policija+ |
| `/bail [citizenid]` | Provjeri bail status | Policija+ |

### Server konzola

```bash
# Restart resourcea nakon izmjena
restart nevera_sudstvo

# Debug mod
setr nevera_sudstvo:debug true

# Refresh fajlova
refresh
```

### Git workflow

```bash
# Kloniraj
git clone https://github.com/tvoj-username/nevera_sudstvo.git

# Provjeri status
git status

# Dodaj izmjene
git add .

# Commit
git commit -m "opis izmjene"

# Push
git push origin main

# Pull (novi pull na serveru)
git pull origin main

# Nakon pull-a uvijek rebuild NUI ako su web fajlovi izmijenjeni
cd web && npm run build
```

---

## NUI Build

```bash
# Instaliraj dependencies (samo prvi put)
cd web
npm install

# Build za produkciju
npm run build

# Dev server (lokalno testiranje u browseru)
npm run dev
```

> ⚠️ Nakon svake izmjene u `web/src/` moraš pokrenuti `npm run build` i restartat resource.

---

## Baza podataka

Sve tablice se kreiraju automatski iz `nevera_sudstvo.sql`.

| Tablica | Opis |
|---|---|
| `doj_slucajevi` | Svi sudski slučajevi |
| `doj_forme` | Podnesene pravne forme |
| `doj_nalozi` | Nalozi za hapšenje i pretres |
| `doj_bail` | Jamčevine |
| `doj_kazne` | Izrečene kazne |
| `doj_rocista` | Zakazana ročišta |
| `doj_porota` | Porotnici po slučaju |
| `doj_uloge` | Uloge igrača |
| `doj_revizija` | Revizijski log svih akcija |
| `doj_dogadjaji` | Timeline događaja po slučaju |

---

## Integracije

### peleg-billing
Koristi se za automatsko naplaćivanje kazni direktno s bankovnog računa igrača.

### 17mov_Phone
Igrači primaju SMS obavijesti o:
- Novim slučajevima
- Promjenama statusa forme
- Zakazanim ročištima
- Izdanim nalozima

### ox_target
Interakcija s objektima u sudnici (sudački pult, svjedočka klupa, arhiva).

### ox_lib
- Kontekst meniji za brze akcije
- Progress bar za procesiranje dokumenata
- Notifikacije

---

## .gitignore

```
node_modules/
web/dist/
*.log
```

---

## Licenca

Privatni resource — Nevera Hard RP Server  

### Licencia / License
For the exclusive use of Nevera-HARD-Rp./ (For Nevera-HARD-Rp use only.)
