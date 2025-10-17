# Demo 
Demo cvičení - place pro dev hrátky
---
---
---
# 🧑‍🏫 Lekce: Od Z-tabulky po OData službu

## Cíl

Naučit se vytvořit vlastní databázovou tabulku.

Postavit nad ní report, který umožňuje čtení, vytvoření, změnu i smazání záznamu.

Stejnou logiku implementovat do OData služby (CRUD) v SEGW.

---
## 1️⃣ Vytvoření Z-tabulky (SE11)

### Postup:

Vytvoř novou tabulku, např. ZDEMO_COURSE.

Nastav Delivery class A a povol Display/Maintenance Allowed.

Přidej pole:

Klient MANDT (key)

ID kurzu COURSE_ID (key)

Název TITLE

Stav STATUS (např. A=Active, I=Inactive)

Datum vytvoření CREATED_AT

Datum změny CHANGED_AT

Ulož a aktivuj.

(Volitelné) Vytvoř doménu pro STATUS s pevnými hodnotami.

Vygeneruj lock object (např. EZDEMO_COURSE) pro zamykání záznamů.

---
## 2️⃣ Report s výběrovou obrazovkou a CRUD funkcionalitou

### Cíl:
Report bude obsahovat výběrovou obrazovku, která umožní:

filtrovat data (READ),

vytvořit záznam (CREATE),

upravit existující (UPDATE),

smazat (DELETE).

### Kroky:

Vytvoř report (SE38 nebo ADT).

Definuj selekční obrazovku:

výběrové možnosti (SELECT-OPTIONS) pro čtení,

parametry pro CRUD operace (např. ID, název, status),

parametr pro určení operace (R, C, U, D).

Přidej pomocné formuláře:

získání timestampu,

zamknutí / odemknutí záznamu.

Implementuj logiku:

READ – výběr dat z tabulky podle filtrů a zobrazení v ALV,

CREATE – kontrola existence, vložení nového záznamu,

UPDATE – zamknutí, načtení, změna, uvolnění zámku,

DELETE – zamknutí, smazání, uvolnění zámku.

Přidej obsluhu událostí:

AT SELECTION-SCREEN – validace vstupů,

START-OF-SELECTION – volání správné formy dle typu operace.

Nakonec zobraz data pomocí ALV (např. třída CL_SALV_TABLE).

---
## 3️⃣ OData služba přes SEGW

### Cíl:
Zpřístupnit stejnou tabulku jako OData službu s CRUD operacemi.

### Postup:

Vytvoř nový projekt v SEGW, např. ZDEMO_COURSE_SRV.

Přidej Entity Type Course s vlastnostmi odpovídajícími polím tabulky.

Vytvoř Entity Set Courses.

Vygeneruj runtime objekty (MPC/DPC třídy).

V DPC_EXT implementuj CRUD metody:

GET_ENTITYSET – čtení všech záznamů s možností filtrace,

GET_ENTITY – čtení konkrétního záznamu,

CREATE_ENTITY – vytvoření nového,

UPDATE_ENTITY – aktualizace,

DELETE_ENTITY – odstranění.

Aktivuj službu v /IWFND/MAINT_SERVICE.

Otestuj CRUD operace pomocí SAP Gateway Clientu nebo Postmanu.

---
## 4️⃣ Best Practices

Používej novou Open SQL syntaxi (@ před proměnnými).

Vkládej COMMIT WORK pouze při úspěšném DML.

Pro změny a mazání vždy zamykej záznamy (ENQUEUE_...).

Přidej AUTHORITY-CHECK před CREATE/UPDATE/DELETE.

Používej datové typy jako TIMESTAMPL (UTC timestamp).

Logiku přesuň do tříd a report nech jen jako UI vrstvu.

V OData používej správné výjimky (/IWBEP/CX_MGW_BUSI_EXCEPTION apod.).

---
## 5️⃣ Rozšíření (volitelné)

Přidej search help pro COURSE_ID.

Implementuj $count, $orderby, $top, $skip v OData.

Použij ETag pro optimistické zamykání.

Vytvoř Fiori/UI5 aplikaci nad vytvořenou službou.

---
## 6️⃣ Shrnutí
Krok	Cíl	Výsledek
1	Z-tabulka v SE11	Základní datový model
2	Report s CRUD logikou	Funkční CRUD přes GUI
3	SEGW projekt	OData API s CRUD metodami
4	Best practices	Správné zamykání, validace, autorizace
5	Rozšíření	Moderní UI (Fiori, RAP, CDS)

---
---
---

# 🧠 ABAP Tahák – CRUD, Selection Screen, ALV, OData

---
## 📋 Základní koncepty
Téma	Popis
Report	Spustitelný program (SE38/ADT), základní forma aplikace v ABAPu.
FORM	Procedurální blok (subroutine), volán pomocí PERFORM.
SE11	Nástroj pro práci s tabulkami, datovými typy, strukturami a pohledy.
SEGW	SAP Gateway Builder – tvorba OData služeb.
Lock Object (ENQUEUE/DEQUEUE)	Mechanismus zamykání záznamů na úrovni aplikační logiky.

---
## 🧩 Výběrová obrazovka (Selection Screen)
Příkaz / prvek	Význam
PARAMETERS	Definuje jednoduchý vstupní parametr.
SELECT-OPTIONS	Umožňuje zadat rozsah hodnot (od-do, seznam).
PARAMETERS ... RADIOBUTTON GROUP	Definuje přepínače (radio buttony).
AT SELECTION-SCREEN	Událost pro validaci vstupů.
START-OF-SELECTION	Spouští hlavní logiku programu.

---
## Příklad struktury událostí:

INITIALIZATION.
AT SELECTION-SCREEN.
START-OF-SELECTION.
END-OF-SELECTION.

---
## 🧮 Práce s databází (Open SQL)
Příkaz	Význam
SELECT ... INTO TABLE	Načtení více řádků.
SELECT SINGLE ... INTO	Načtení jednoho řádku.
INSERT INTO ... FROM	Vložení nového záznamu.
UPDATE ... FROM	Aktualizace existujícího záznamu.
DELETE FROM ... WHERE	Smazání záznamu.
MODIFY ... FROM	Vložení nebo update podle klíče.
COMMIT WORK	Potvrzení transakce.
ROLLBACK WORK	Zrušení transakce.

---
## Tipy:

Používej moderní syntaxi s @ (host proměnné).

Pro typovanou konstrukci použij VALUE #( ) nebo VALUE tablename( ).

FOR ALL ENTRIES použij pro výběr podle více hodnot.

---
## 🔒 Zamykání objektů (Lock Objects)
Element	Popis
SE11 → Lock Object	Definice zamykacího objektu (např. EZDEMO_COURSE).
ENQUEUE_<objekt>	Uzamkne záznam.
DEQUEUE_<objekt>	Odemkne záznam.
SM12	Správa zámků (přehled aktivních locků).

---
## Doporučení:

Zamykat před UPDATE/DELETE.

Po operaci záznam vždy odemknout.

V OData zvaž optimistické zamykání (ETag).

---
## 🧾 ALV (ABAP List Viewer)
Cíl	Postup
Zobrazení dat v tabulce	Použij třídu CL_SALV_TABLE.
Tvorba instance	CL_SALV_TABLE=>FACTORY( ).
Zobrazení výsledku	Metoda DISPLAY( ).
Přidání funkcí (export, layout)	Přes objekty lo_alv->get_functions( ).

---
### Výhody ALV:

Automatické třídění, filtrování, export.

Umožňuje rychle zobrazit data z interní tabulky.

---
## ⚙️ Eventy v reportu
Událost	Účel
INITIALIZATION	Nastavení výchozích hodnot.
AT SELECTION-SCREEN	Kontrola vstupů.
START-OF-SELECTION	Spuštění hlavní logiky.
END-OF-SELECTION	Výpis výsledků.
TOP-OF-PAGE	Hlavička při tisku nebo výpisu.
AT LINE-SELECTION	Reakce na dvojklik v seznamu.
##🧍‍♂️ AUTHORITY-CHECK
Příkaz	Význam
AUTHORITY-CHECK OBJECT 'S_TABU_NAM' ID 'TABLE' FIELD 'ZDEMO_COURSE'	Ověří oprávnění k tabulce.
SY-SUBRC = 0	Oprávnění existuje.
SY-SUBRC <> 0	Přístup zamítnut.


### Tipy:

Autorizaci kontroluj před CRUD operacemi.

Můžeš použít i vlastní autorizační objekty (definované v SU21).

---
## 🌐 SEGW (SAP Gateway Builder)
Krok	Popis
1. Vytvoř projekt	Např. ZDEMO_COURSE_SRV.
2. Definuj Entity Type	Odpovídá tabulce (např. Course).
3. Definuj Entity Set	Kolekce entit (např. Courses).
4. Generuj runtime objekty	Vytvoří MPC a DPC třídy.
5. Implementuj CRUD v DPC_EXT	GET_ENTITYSET, GET_ENTITY, CREATE_ENTITY, UPDATE_ENTITY, DELETE_ENTITY.
6. Aktivuj službu v /IWFND/MAINT_SERVICE	Umožní volání OData API.
7. Testuj v Gateway Clientu	/IWFND/GW_CLIENT nebo Postman.

---
## 🧱 EDM (Entity Data Model)
Termín	Význam
Entity Type	Struktura dat (např. kurz).
Entity Set	Kolekce entit stejného typu.
Property	Jednotlivé pole (atribut).
Key Property	Klíčová vlastnost entity.
Association / Navigation Property	Vztah mezi entitami.

---
## 🌍 OData CRUD operace
HTTP metoda	Popis	Odpovídající ABAP metoda
GET	Čtení dat	GET_ENTITYSET, GET_ENTITY
POST	Vytvoření nového záznamu	CREATE_ENTITY
PATCH / PUT	Aktualizace existujícího záznamu	UPDATE_ENTITY
DELETE	Smazání záznamu	DELETE_ENTITY

---
## Doporučení:

Používej /IWBEP/CX_MGW_BUSI_EXCEPTION pro business chyby.

Měj správně vyplněné er_entity v odpovědi.

Validuj vstupy (ID, formát, autoritu).

---
## 🧠 Bonus: Debugging & Testing
Nástroj	Použití
/IWFND/GW_CLIENT	Testování OData služeb.
SE80 / ADT Debugger	Ladění ABAP logiky.
SM12	Kontrola zámků.
ST05 / SAT	Analýza SQL výkonu.
ABAP Unit (SE80)	Testování business logiky.

---
## 🧾 Shrnutí
Oblast	Hlavní příkazy / nástroje
Selection Screen	PARAMETERS, SELECT-OPTIONS, AT SELECTION-SCREEN
Databáze	SELECT, INSERT, UPDATE, DELETE, COMMIT WORK
Zamykání	ENQUEUE_..., DEQUEUE_..., SM12
ALV	CL_SALV_TABLE=>FACTORY(), DISPLAY()
Autorizace	AUTHORITY-CHECK
SEGW / OData	CRUD metody v DPC_EXT
Eventy	START-OF-SELECTION, END-OF-SELECTION, AT LINE-SELECTION
