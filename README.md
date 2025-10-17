# Demo 
Demo cvičení - place pro dev hrátky

# 🧑‍🏫 Lekce: Od Z-tabulky po OData službu
Cíl

Naučit se vytvořit vlastní databázovou tabulku.

Postavit nad ní report, který umožňuje čtení, vytvoření, změnu i smazání záznamu.

Stejnou logiku implementovat do OData služby (CRUD) v SEGW.

1️⃣ Vytvoření Z-tabulky (SE11)

Postup:

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

2️⃣ Report s výběrovou obrazovkou a CRUD funkcionalitou

Cíl:
Report bude obsahovat výběrovou obrazovku, která umožní:

filtrovat data (READ),

vytvořit záznam (CREATE),

upravit existující (UPDATE),

smazat (DELETE).

Kroky:

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

3️⃣ OData služba přes SEGW

Cíl:
Zpřístupnit stejnou tabulku jako OData službu s CRUD operacemi.

Postup:

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

4️⃣ Best Practices

Používej novou Open SQL syntaxi (@ před proměnnými).

Vkládej COMMIT WORK pouze při úspěšném DML.

Pro změny a mazání vždy zamykej záznamy (ENQUEUE_...).

Přidej AUTHORITY-CHECK před CREATE/UPDATE/DELETE.

Používej datové typy jako TIMESTAMPL (UTC timestamp).

Logiku přesuň do tříd a report nech jen jako UI vrstvu.

V OData používej správné výjimky (/IWBEP/CX_MGW_BUSI_EXCEPTION apod.).

5️⃣ Rozšíření (volitelné)

Přidej search help pro COURSE_ID.

Implementuj $count, $orderby, $top, $skip v OData.

Použij ETag pro optimistické zamykání.

Vytvoř Fiori/UI5 aplikaci nad vytvořenou službou.

6️⃣ Shrnutí
Krok	Cíl	Výsledek
1	Z-tabulka v SE11	Základní datový model
2	Report s CRUD logikou	Funkční CRUD přes GUI
3	SEGW projekt	OData API s CRUD metodami
4	Best practices	Správné zamykání, validace, autorizace
5	Rozšíření	Moderní UI (Fiori, RAP, CDS)
