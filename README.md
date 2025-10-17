# 🧩 Demo  
**Demo cvičení — prostor pro vývojářské hrátky**

---

# 🧑‍🏫 Lekce: Od Z-tabulky po OData službu

## 🎯 Cíl

- Naučit se vytvořit vlastní databázovou tabulku  
- Postavit nad ní report, který umožňuje čtení, vytvoření, změnu i smazání záznamu  
- Stejnou logiku implementovat do OData služby (CRUD) v SEGW  

---

## 1️⃣ Vytvoření Z-tabulky (SE11)

### Postup:

- Vytvoř novou tabulku, např. `ZDEMO_COURSE`
- Nastav:
  - **Delivery class:** `A`
  - **Display/Maintenance Allowed:** Ano
- Přidej pole:
  - `MANDT` – klient (key)
  - `COURSE_ID` – ID kurzu (key)
  - `TITLE` – název
  - `STATUS` – např. `A=Active`, `I=Inactive`
  - `CREATED_AT` – datum vytvoření
  - `CHANGED_AT` – datum změny
- Ulož a aktivuj tabulku  
- (Volitelné) Vytvoř doménu pro `STATUS` s pevnými hodnotami  
- Vygeneruj **lock object** (např. `EZDEMO_COURSE`) pro zamykání záznamů  

---

## 2️⃣ Report s výběrovou obrazovkou a CRUD funkcionalitou

### Cíl

Report bude obsahovat výběrovou obrazovku, která umožní:
- filtrovat data (READ)
- vytvořit záznam (CREATE)
- upravit existující (UPDATE)
- smazat (DELETE)

### Kroky

- Vytvoř nový report (SE38 nebo ADT)
- Definuj selekční obrazovku:
  - výběrové možnosti (`SELECT-OPTIONS`) pro čtení
  - parametry pro CRUD operace (např. ID, název, status)
  - parametr pro určení operace (`R`, `C`, `U`, `D`)
- Přidej pomocné formuláře:
  - získání timestampu
  - zamknutí / odemknutí záznamu
- Implementuj logiku:
  - **READ** – výběr dat z tabulky podle filtrů a zobrazení v ALV  
  - **CREATE** – kontrola existence, vložení nového záznamu  
  - **UPDATE** – zamknutí, načtení, změna, uvolnění zámku  
  - **DELETE** – zamknutí, smazání, uvolnění zámku
- Přidej obsluhu událostí:
  - `AT SELECTION-SCREEN` – validace vstupů  
  - `START-OF-SELECTION` – volání správné formy dle typu operace
- Nakonec zobraz data pomocí **ALV** (`CL_SALV_TABLE`)

---

## 3️⃣ OData služba přes SEGW

### Cíl
Zpřístupnit stejnou tabulku jako OData službu s CRUD operacemi.

### Postup

- Vytvoř nový projekt v **SEGW**, např. `ZDEMO_COURSE_SRV`
- Přidej **Entity Type** `Course` s vlastnostmi odpovídajícími polím tabulky
- Vytvoř **Entity Set** `Courses`
- Vygeneruj runtime objekty (MPC/DPC třídy)
- V DPC_EXT implementuj CRUD metody:
  - `GET_ENTITYSET` – čtení všech záznamů
  - `GET_ENTITY` – čtení konkrétního záznamu
  - `CREATE_ENTITY` – vytvoření nového záznamu
  - `UPDATE_ENTITY` – aktualizace záznamu
  - `DELETE_ENTITY` – odstranění záznamu
- Aktivuj službu v `/IWFND/MAINT_SERVICE`
- Otestuj CRUD operace pomocí **SAP Gateway Clientu** nebo **Postmanu**

---

## 4️⃣ Best Practices

- Používej novou Open SQL syntaxi (`@` před proměnnými)
- `COMMIT WORK` dávej jen po úspěšném DML
- Před změnou nebo smazáním záznamu vždy zamykej (`ENQUEUE_...`)
- Před `CREATE`, `UPDATE`, `DELETE` proveď `AUTHORITY-CHECK`
- Používej datové typy jako `TIMESTAMPL` (UTC timestamp)
- Logiku business vrstvy přesuň do tříd (oddělení UI a logiky)
- V OData metodách používej výjimky `/IWBEP/CX_MGW_BUSI_EXCEPTION`

---

## 5️⃣ Rozšíření (volitelné)

- Přidej **search help** pro `COURSE_ID`
- Implementuj `$count`, `$orderby`, `$top`, `$skip` v OData
- Použij **ETag** pro optimistické zamykání
- Vytvoř **Fiori/UI5 aplikaci** nad vytvořenou službou

---

## 6️⃣ Shrnutí

**Krok 1:** Z-tabulka v SE11 → základní datový model  
**Krok 2:** Report s CRUD logikou → funkční CRUD přes GUI  
**Krok 3:** SEGW projekt → OData API s CRUD metodami  
**Krok 4:** Best practices → správné zamykání, validace, autorizace  
**Krok 5:** Rozšíření → moderní UI (Fiori, RAP, CDS)

---

# 🧠 ABAP Tahák – CRUD, Selection Screen, ALV, OData

---

## 📋 Základní koncepty

- **Report:** Spustitelný program (SE38/ADT)
- **FORM:** Procedurální blok, volán pomocí `PERFORM`
- **SE11:** Datové typy, tabulky, pohledy
- **SEGW:** Gateway Builder – tvorba OData služeb
- **Lock Object:** Mechanismus zamykání záznamů (ENQUEUE/DEQUEUE)

---

## 🧩 Výběrová obrazovka (Selection Screen)

- `PARAMETERS` – jednoduchý vstupní parametr  
- `SELECT-OPTIONS` – rozsah hodnot (od–do, seznam)  
- `RADIOBUTTON GROUP` – přepínače  
- `AT SELECTION-SCREEN` – validace vstupů  
- `START-OF-SELECTION` – spuštění hlavní logiky  

**Typická struktura událostí:**
```
INITIALIZATION.
AT SELECTION-SCREEN.
START-OF-SELECTION.
END-OF-SELECTION.
```

---

## 🧮 Práce s databází (Open SQL)

- `SELECT ... INTO TABLE` – čtení více řádků  
- `SELECT SINGLE ... INTO` – čtení jednoho řádku  
- `INSERT INTO ... FROM` – vložení  
- `UPDATE ... FROM` – aktualizace  
- `DELETE FROM ... WHERE` – smazání  
- `COMMIT WORK` / `ROLLBACK WORK` – potvrzení nebo zrušení transakce  

**Tipy:**
- Používej `@` pro host proměnné  
- `VALUE #( )` pro konstrukci struktury  
- `FOR ALL ENTRIES` pro hromadné dotazy  

---

## 🔒 Zamykání objektů (Lock Objects)

- Vytvoř lock objekt v SE11 (např. `EZDEMO_COURSE`)  
- Použij:
  - `ENQUEUE_<objekt>` – uzamkne záznam  
  - `DEQUEUE_<objekt>` – odemkne záznam  
- Sleduj zámky v **SM12**

**Doporučení:**
- Zamykat před `UPDATE` nebo `DELETE`  
- Po operaci vždy záznam odemknout  
- V OData použij **ETag** (optimistické zamykání)

---

## 🧾 ALV (ABAP List Viewer)

- Zobrazení dat: `CL_SALV_TABLE=>FACTORY()`  
- Výpis dat: `DISPLAY()`  
- Volitelné funkce: `lo_alv->get_functions()`  

**Výhody:**
- Automatické třídění, filtrování, export  
- Rychlé zobrazení tabulkových dat  

---

## ⚙️ Eventy v reportu

- `INITIALIZATION` – výchozí hodnoty  
- `AT SELECTION-SCREEN` – kontrola vstupů  
- `START-OF-SELECTION` – hlavní logika  
- `END-OF-SELECTION` – výstup dat  
- `TOP-OF-PAGE` – hlavička tisku  
- `AT LINE-SELECTION` – reakce na klik  

---

## 🧍‍♂️ AUTHORITY-CHECK

- `AUTHORITY-CHECK OBJECT 'S_TABU_NAM' ID 'TABLE' FIELD 'ZDEMO_COURSE'`
  - `SY-SUBRC = 0` → oprávnění existuje  
  - `SY-SUBRC <> 0` → přístup zamítnut  

**Tipy:**
- Kontroluj přístup před CRUD operacemi  
- Vlastní autorizační objekty lze vytvořit v SU21  

---

## 🌐 SEGW (SAP Gateway Builder)

1. Vytvoř projekt (např. `ZDEMO_COURSE_SRV`)  
2. Definuj **Entity Type** (např. `Course`)  
3. Definuj **Entity Set** (`Courses`)  
4. Generuj runtime objekty (MPC/DPC)  
5. Implementuj CRUD v DPC_EXT (`GET`, `POST`, `PATCH`, `DELETE`)  
6. Aktivuj službu v `/IWFND/MAINT_SERVICE`  
7. Testuj pomocí `/IWFND/GW_CLIENT` nebo Postman  

---

## 🧱 EDM (Entity Data Model)

- **Entity Type** – struktura dat (např. kurz)  
- **Entity Set** – kolekce entit stejného typu  
- **Property** – atribut  
- **Key Property** – klíč entity  
- **Association** – vztah mezi entitami  

---

## 🌍 OData CRUD operace

- **GET** → čtení dat → `GET_ENTITYSET`, `GET_ENTITY`  
- **POST** → vytvoření záznamu → `CREATE_ENTITY`  
- **PATCH/PUT** → aktualizace → `UPDATE_ENTITY`  
- **DELETE** → smazání záznamu → `DELETE_ENTITY`  

**Doporučení:**
- Používej `/IWBEP/CX_MGW_BUSI_EXCEPTION` pro business chyby  
- Vyplňuj `er_entity` při návratu  
- Validuj vstupní data (ID, formát, autoritu)  

---

## 🧠 Bonus: Debugging & Testing

- `/IWFND/GW_CLIENT` – testování OData služeb  
- **SE80 / ADT Debugger** – ladění logiky  
- **SM12** – kontrola zámků  
- **ST05 / SAT** – výkon SQL  
- **ABAP Unit** – testování logiky  

---

## 🧾 Shrnutí

- **Selection Screen:** `PARAMETERS`, `SELECT-OPTIONS`, `AT SELECTION-SCREEN`  
- **Databáze:** `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `COMMIT WORK`  
- **Zamykání:** `ENQUEUE_...`, `DEQUEUE_...`, `SM12`  
- **ALV:** `CL_SALV_TABLE=>FACTORY()`, `DISPLAY()`  
- **Autorizace:** `AUTHORITY-CHECK`  
- **SEGW / OData:** CRUD metody v DPC_EXT  
- **Eventy:** `START-OF-SELECTION`, `END-OF-SELECTION`, `AT LINE-SELECTION`  
