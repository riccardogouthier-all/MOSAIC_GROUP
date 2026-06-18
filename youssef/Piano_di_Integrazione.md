# Offerta Tecnica ed Economica: Integrazione IT Themea Consulting × Pro Studio

**Destinatario:** Comitato Direttivo per l'Integrazione M&A
**Redatto da:** MOSAIC Group — Team di Integrazione IT
**Versione:** 1.0 (Finale)
**Data:** 17 giugno 2026
**Classificazione:** Riservato — Uso Interno

**Strategia selezionata:** Integrazione "Best-of-Breed" SaaS supportata da AWS

---

## Indice

1. Presentazione dell'Azienda e del Team
2. Introduzione e Ambito dell'Offerta
3. Panoramica dell'Architettura di Destinazione
4. Selezione delle Piattaforme — Confronti Oggettivi
5. Architettura di Identità e Single Sign-On
6. Selezione e Migrazione Servizio per Servizio
7. Descrizione Hardware
8. Attività Richieste
9. Mappatura dei Servizi ai Criteri Richiesti
10. Piano di Implementazione (Cronoprogramma, Gantt, RASCI)
11. Gestione dei Rischi
12. Criteri di Successo (KPI)
13. Backup e Disaster Recovery
14. Offerta Economica (Costi)
15. Documentazione
16. Termini e Condizioni
17. Conclusione

---

## 1. Presentazione dell'Azienda e del Team

MOSAIC Group è una società di consulenza specializzata in integrazione IT e cloud, focalizzata sul consolidamento tecnologico post-fusione per piccole e medie imprese di servizi professionali. Il nostro lavoro si concentra sull'unificazione di infrastrutture IT frammentate in ambienti coerenti e a bassa manutenzione, costruiti su piattaforme cloud consolidate.

Questa offerta è erogata da un team di integrazione dedicato, costituito per la fusione tra Themea Consulting e Pro Studio:

| Ruolo | Responsabilità nel progetto |
| --- | --- |
| Project Lead / Architetto Enterprise | Progettazione complessiva della soluzione, allineamento degli stakeholder, approvazione finale |
| Ingegnere Cloud e Identità | AWS IAM Identity Center, federazione SSO, provisioning SCIM |
| Ingegnere Migrazione Dati | Pipeline ETL (AWS Glue, DMS, DataSync), validazione dati |
| Specialista ERP / Odoo | Configurazione Odoo, mappatura dati contabilità e CRM |
| Ingegnere Collaborazione | Tenant Google Workspace, migrazione posta e file |
| Responsabile QA e Documentazione | Test funzionali, accettazione utente, documentazione di consegna |

Il team opera come un'unica unità di delivery, riportando al Comitato Direttivo per l'Integrazione M&A durante l'intero incarico di 13 settimane.

---

## 2. Introduzione e Ambito dell'Offerta

Questo documento costituisce l'offerta tecnica ed economica per il consolidamento dei servizi IT di Themea Consulting e Pro Studio in un unico ambiente operativo unificato a seguito della loro fusione. Definisce l'architettura di destinazione consigliata, la piattaforma selezionata per ciascun servizio, l'approccio di migrazione, l'infrastruttura di supporto, il piano di implementazione e i relativi costi e condizioni commerciali.

### Incluso nell'ambito
- Consolidamento delle dieci categorie di servizi comuni a entrambe le aziende (posta, produttività, conferenze, archiviazione file, identità, controllo versioni, contabilità, CRM, rilevamento ore e documentazione).
- Selezione di un'unica piattaforma di destinazione per servizio, basata su confronto oggettivo.
- Identità centralizzata e single sign-on (SSO) su tutte le piattaforme.
- Migrazione dei dati dai sistemi dismessi verso le piattaforme selezionate.
- Infrastruttura AWS di supporto per i carichi di lavoro legacy e self-hosted.
- Cronoprogramma di implementazione, gestione dei rischi, collaudo e documentazione.

### Escluso dall'ambito
- Acquisto di dispositivi per utenti finali (laptop, periferiche) — vedere Descrizione Hardware.
- Provisioning di circuiti di rete/WAN presso le sedi aziendali.
- Reingegnerizzazione dei processi aziendali oltre a quanto richiesto dalla migrazione.
- Sviluppo software personalizzato non correlato all'integrazione.

### 2.1 Perché un Modello di Deployment SaaS-First

Prima di selezionare le singole piattaforme, è stato valutato il modello di deployment stesso. Quattro modelli sono stati considerati alla luce delle realtà di questa fusione: un organico combinato ridotto (~100 utenti), due aziende di servizi professionali il cui vantaggio competitivo è la consegna al cliente piuttosto che la gestione dell'infrastruttura, e la necessità di unificare rapidamente con capitale iniziale limitato.

| Modello di deployment | Valutazione per questa fusione |
| --- | --- |
| **SaaS (selezionato)** | Il fornitore gestisce la piattaforma; nessun server da aggiornare. Unificazione più rapida, costo per utente prevedibile, operazioni interne minime. La soluzione migliore per una piccola azienda di servizi. |
| Self-hosted (on-premise / VM) | Massimo controllo ma elevato carico operativo — esattamente il problema di Themea che si vuole eliminare. Scartato. |
| IaaS (lift-and-shift su AWS EC2) | Sposta i server nel cloud ma mantiene patching del sistema operativo, backup e scalabilità come compiti interni. Utile solo per le poche app legacy senza equivalente SaaS. |
| Ibrido | Adottato in forma limitata: SaaS per i servizi principali, una piccola infrastruttura AWS per le app legacy (Profis, MediaWiki) e l'identità. |

Il risultato è un modello SaaS-first con un livello AWS volutamente ridotto per il brokeraggio dell'identità e per i pochi carichi di lavoro privi di un equivalente SaaS adeguato.

---

## 3. Panoramica dell'Architettura di Destinazione

L'ambiente di destinazione standardizza entrambe le aziende su due ecosistemi SaaS maturi — Google Workspace per la collaborazione e Odoo SaaS per le operazioni aziendali — con AWS IAM Identity Center che funge da broker di identità centrale e una piccola infrastruttura AWS che ospita i restanti carichi di lavoro legacy e self-hosted.

```
                  +-------------------------------------------+
                  |   AWS IAM Identity Center (broker SSO)    |
                  |   Un accesso -> ogni applicazione         |
                  +---+-------------------+---------------+---+
                      | SAML 2.0          | OpenID        | SAML/OAuth
                      v                   v               v
          +-----------------+   +-----------------+   +-----------------+
          | Google Workspace|   |    Odoo SaaS    |   |  Ospitato AWS   |
          | - Posta/Calend. |   | - CRM           |   | - Profis (EC2)  |
          | - Docs/Drive    |   | - Contabilità   |   | - MediaWiki     |
          | - Meet/Chat     |   | - Timesheet     |   |   (EC2 + RDS)   |
          +-----------------+   +-----------------+   +-----------------+
                      ^                   ^
                      | Provisioning automatico SCIM (utenti + gruppi)
                  +---+-------------------+---+
                  |   AWS IAM Identity Center |
                  +---------------------------+
```

L'autenticazione passa attraverso un unico broker, mentre ogni piattaforma continua a fornire la propria esperienza nativa. I nuovi dipendenti vengono provisionati automaticamente in ogni sistema da un'unica directory.

---

## 4. Selezione delle Piattaforme — Confronti Oggettivi

Ogni piattaforma è stata selezionata rispetto alle alternative di mercato, non solo rispetto allo strumento interno dismesso. Criteri di valutazione: costo, sforzo di integrazione, scalabilità, oneri di amministrazione e modello di licenza.

### 4.1 Suite di Collaborazione — Google Workspace vs Microsoft 365

| Criterio | Google Workspace (selezionato) | Microsoft 365 |
| --- | --- | --- |
| Costo (per utente/mese) | ~€10 (Business Standard) | ~€11–12 (Business Standard) |
| Sforzo di integrazione | Già attivo in Pro Studio; SSO nativo | Implementazione totalmente nuova per entrambe |
| Collaborazione in tempo reale | Tra i migliori, nativa nel browser | Solida, ma orientata al desktop |
| Amministrazione | Console unica; basso onere | Capace ma più complessa (Entra, Exchange, SharePoint) |
| Modello di licenza | SaaS per utente, semplice | SaaS per utente con più SKU/add-on |

**Motivazione:** Pro Studio utilizza già Google Workspace in modo produttivo. Selezionarlo significa che metà dell'azienda unificata non cambia nulla, mentre il personale Themea passa dai frammentati Zimbra/MS365/LibreOffice a un'unica suite moderna.

### 4.2 Operazioni Aziendali — Odoo vs Salesforce + ERP separato

| Criterio | Odoo SaaS (selezionato) | Salesforce + ERP separato |
| --- | --- | --- |
| Copertura funzionale | CRM + Contabilità + Timesheet in un'unica suite | Solo CRM; ERP/contabilità richiede un secondo prodotto |
| Costo | ~€25–40 per utente/mese, un solo fornitore | Salesforce €75+/utente più licenze ERP |
| Sforzo di integrazione | Già attivo in Pro Studio; modello dati unico | Due sistemi da integrare e riconciliare |
| Amministrazione | Una piattaforma, un contratto di supporto | Due piattaforme, due competenze amministrative |
| Localizzazione fiscale italiana | Integrata (l10n_it) | Dipende dall'ERP scelto |

**Motivazione:** Odoo unifica CRM, contabilità e timesheet in un unico modello dati già in produzione presso Pro Studio. Uno stack Salesforce-più-ERP costerebbe di più e reintrodurrebbe il problema di integrazione che la fusione mira a eliminare.

### 4.3 Controllo Versioni — Bitbucket vs GitHub / GitLab

| Criterio | Bitbucket (selezionato) | GitHub | GitLab |
| --- | --- | --- | --- |
| Costo (per utente/mese) | ~€5 (Standard) | ~€4 (Team) | ~€25 (Premium) |
| Utilizzo attuale | Attivo in Pro Studio | Non utilizzato | Non utilizzato |
| CI/CD integrato | Pipeline incluse | Actions incluse | CI/CD potente |
| Sforzo di migrazione | Solo mirroring dei repo GiTea | Nuova implementazione | Nuova implementazione |

**Motivazione:** Bitbucket è già in uso presso Pro Studio e copre le esigenze del team con pipeline integrate. Migrare solo i repository GiTea di Themea è molto meno dirompente che standardizzare tutti su una nuova piattaforma.

### 4.4 Provider di Identità — AWS IAM Identity Center vs Entra ID vs Google Cloud Identity

| Criterio | AWS IAM Identity Center (selezionato) | Microsoft Entra ID | Google Cloud Identity |
| --- | --- | --- | --- |
| Broker neutrale per Google + Odoo + AWS | Sì — hub indipendente dal fornitore | Orientato a Microsoft | Orientato a Google |
| Costo | Nessun costo per utente per SSO ad app SAML | Livelli premium per SSO avanzato | Per utente oltre il livello gratuito |
| Accesso alle risorse AWS | Nativo (app legacy su EC2) | Integrazione aggiuntiva | Integrazione aggiuntiva |
| Evita il vendor lock-in | Basato su standard SAML/OIDC/SCIM | Tende verso lo stack MS | Tende verso lo stack Google |

**Motivazione** spiegata in dettaglio nella Sezione 5.

---

## 5. Architettura di Identità e Single Sign-On

L'identità è il fondamento dell'integrazione: è ciò che fa sì che due aziende separate si comportino come una sola. AWS IAM Identity Center è selezionato come broker di identità centrale, anziché utilizzare Google Workspace o Microsoft Entra ID direttamente come provider principale.

### 5.1 Perché un Broker di Identità Dedicato

Utilizzare Google Workspace come unico provider di identità legherebbe ogni accesso — incluso quello a Odoo e alle app legacy ospitate su AWS — a un singolo fornitore applicativo. La stessa preoccupazione vale per Entra ID. Un broker dedicato e basato su standard mantiene l'identità indipendente da qualsiasi suite di produttività, il che è importante perché:

- Odoo e le app legacy ospitate su AWS non sono prodotti Google; instradare la loro autenticazione tramite Google sarebbe una dipendenza scomoda.
- Il broker parla SAML 2.0, OpenID Connect e SCIM — standard aperti — quindi qualsiasi futura sostituzione di piattaforma (ad es. sostituire uno strumento SaaS) non richiede di riprogettare l'identità.
- AWS IAM Identity Center governa nativamente l'accesso all'account AWS che ospita Profis e MediaWiki, cosa che un IdP di suite di produttività non fa.
- Un'unica directory diventa l'unico punto per l'onboarding, l'offboarding e l'audit degli utenti su tutte le piattaforme, riducendo gli oneri di amministrazione a lungo termine.

### 5.2 Flusso di Autenticazione

```
  Utente             AWS IAM Identity Center            Applicazione
   |                          |                          |
   |  1. Apre app / portale   |                          |
   |------------------------> |                          |
   |  2. Autenticazione (pwd  |                          |
   |     + MFA)               |                          |
   |------------------------> |                          |
   |                          | 3. Emette asserzione     |
   |                          |    SAML / OIDC           |
   |                          |------------------------> |
   |                          |                          | 4. Concede
   |                          |                          |    la sessione
   |  5. Accesso effettuato, nessun secondo prompt       |
   | <---------------------------------------------------|
```

L'utente si autentica una sola volta. Google Workspace riceve un'asserzione SAML 2.0; Odoo riceve un token OpenID Connect; le risorse AWS sono accessibili tramite la stessa directory. L'autenticazione a più fattori è applicata una sola volta, centralmente.

### 5.3 Provisioning (SCIM)

Gli account utente vengono creati e disattivati automaticamente tramite SCIM (System for Cross-domain Identity Management). Quando una persona viene aggiunta a un gruppo in AWS IAM Identity Center, SCIM crea l'account in Google Workspace e Odoo con i ruoli corretti; quando esce, lo stesso meccanismo disabilita l'accesso ovunque.

| Evento | Risultato automatico tramite SCIM |
| --- | --- |
| Nuovo assunto aggiunto al gruppo "Vendite" | Account Google creato; utente CRM Odoo creato; accesso al drive condiviso Vendite concesso |
| Cambio di ruolo | Appartenenza al gruppo aggiornata → ruoli applicativi aggiornati automaticamente |
| Offboarding | Una singola disattivazione disabilita l'accesso a Google, Odoo e AWS; dati conservati per audit |

Questo è il fulcro dell'argomentazione sulla "riduzione degli oneri di gestione a lungo termine": l'identità viene amministrata una sola volta, non separatamente in ogni piattaforma.

---

## 6. Selezione e Migrazione Servizio per Servizio

Per ciascuna delle dieci categorie di servizi, la tabella mostra i sistemi dismessi, la piattaforma selezionata, la motivazione e l'approccio di migrazione.

### 6.1 Posta e Calendario

**Selezionato:** Google Workspace

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| Zimbra (VM self-hosted) | Google Workspace | **Google Workspace** |

**Perché:** Pro Studio lo utilizza già; collaborazione superiore e zero manutenzione server rispetto allo Zimbra self-hosted.
**Migrazione:** Esportare le caselle Zimbra via IMAP, importare tramite il servizio di migrazione dati di Google, far funzionare entrambi i sistemi in parallelo per due settimane, quindi effettuare il cutover dei record MX.
**Durata indicativa:** 4 settimane

### 6.2 Office e Produttività

**Selezionato:** Google Workspace (Docs/Sheets/Slides)

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| MS365 / LibreOffice | Google Workspace | **Google Workspace** |

**Perché:** Elimina un mix frammentato; collaborazione in tempo reale nel browser e un unico archivio documenti.
**Migrazione:** Convertire in massa i documenti legacy in formati Google; organizzare in drive condivisi per team; conservare gli originali come archivio di sola lettura per sei mesi.
**Durata indicativa:** 3 settimane

### 6.3 Videoconferenza e Chat

**Selezionato:** Google Meet + Google Chat

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| MS Teams | Google Meet | **Google Meet + Google Chat** |

**Perché:** Consolida su un unico strumento già in uso; integrato con la suite di posta scelta.
**Migrazione:** Disabilitare le licenze Teams; esportare e archiviare la cronologia chat; formare gli utenti su Meet/Chat.
**Durata indicativa:** 2 settimane

### 6.4 Archiviazione File

**Selezionato:** Google Drive

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| NextCloud (VM self-hosted) | Google Drive | **Google Drive** |

**Perché:** Elimina un server self-hosted; affidabile, ottimizzato per mobile, incluso in Workspace.
**Migrazione:** Usare AWS DataSync per spostare i dati NextCloud in un bucket di staging S3, quindi importare in Google Drive via API; mappare le cartelle ai team tramite i gruppi di identità.
**Durata indicativa:** 3 settimane

### 6.5 CRM

**Selezionato:** Odoo SaaS

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| SugarCRM (VM self-hosted) | Odoo SaaS | **Odoo SaaS** |

**Perché:** Pro Studio utilizza già Odoo; il CRM è nativamente collegato a contabilità e timesheet in un unico modello dati.
**Migrazione:** AWS Glue ETL estrae i record SugarCRM, li mappa su partner/lead/opportunità Odoo, valida e carica tramite l'API Odoo.
**Durata indicativa:** 3 settimane di progettazione + 2 settimane di esecuzione

### 6.6 Contabilità e Finanza

**Selezionato:** Odoo SaaS (Modulo Contabilità)

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| Profis (Server Windows legacy) | Odoo SaaS | **Odoo SaaS** |

**Perché:** Interfaccia moderna, localizzazione fiscale italiana aggiornata, integrata con la fatturazione guidata dal CRM.
**Migrazione:** AWS DMS migra il piano dei conti e le fatture in Odoo; Profis mantenuto in sola lettura su EC2 per due settimane come fallback. Vedere la nota nella Sezione 7.
**Durata indicativa:** 3 settimane di progettazione + 1 settimana di esecuzione

### 6.7 Rilevamento Ore

**Selezionato:** Odoo SaaS (Modulo Timesheet)

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| Kimai (VM self-hosted) | Odoo SaaS | **Odoo SaaS** |

**Perché:** Collegamento nativo ai progetti e fatturazione automatica; nessuno strumento separato da mantenere.
**Migrazione:** Esportare le registrazioni Kimai, trasformarle e caricarle in massa in Odoo Timesheet; riconciliare le ore totali rispetto alla fonte.
**Durata indicativa:** 2 settimane

### 6.8 Controllo Versioni

**Selezionato:** Bitbucket SaaS

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| GiTea (self-hosted) | Bitbucket (SaaS) | **Bitbucket SaaS** |

**Perché:** Già in uso presso Pro Studio; pipeline CI/CD integrate; solo i repo Themea devono essere spostati.
**Migrazione:** Mirror-push dei repository GiTea su Bitbucket; riconfigurare i webhook CI/CD; verificare le build.
**Durata indicativa:** 2 settimane

### 6.9 Documentazione / Knowledge Base

**Selezionato:** MediaWiki su AWS (EC2 + RDS)

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| MediaWiki (VM self-hosted) | MediaWiki (self-hosted su GCP) | **MediaWiki su AWS** |

**Perché:** Entrambe le aziende usano già MediaWiki, quindi nessuna riformazione; mantenuto su una piccola infrastruttura AWS gestita a basso costo, con l'opzione di passare a un wiki SaaS in seguito.
**Migrazione:** Esportare entrambi i database MySQL, unirli in un'unica istanza Amazon RDS, ospitare il front-end su una piccola istanza EC2 e riconciliare pagine e link duplicati.
**Durata indicativa:** 3 settimane

### 6.10 Gestione di Identità e Accessi

**Selezionato:** AWS IAM Identity Center

| Themea (attuale) | Pro Studio (attuale) | Selezionato |
| --- | --- | --- |
| Active Directory su Samba4 (VM) | Google Authenticator | **AWS IAM Identity Center** |

**Perché:** Broker SSO neutrale tra Google, Odoo e AWS; MFA centralizzato; provisioning automatico (vedere Sezione 5).
**Migrazione:** Costruire la directory in AWS IAM Identity Center, federare Google (SAML) e Odoo (OIDC), abilitare SCIM, fare un pilota con un piccolo gruppo, quindi effettuare il cutover e dismettere Samba4.
**Durata indicativa:** 2 settimane

---

## 7. Descrizione Hardware

Poiché la soluzione selezionata è SaaS-first, richiede quasi nessun hardware dedicato. Le piattaforme girano sull'infrastruttura dei fornitori e la piccola infrastruttura AWS utilizza risorse virtuali on-demand anziché server fisici. Questa sezione documenta la posizione hardware, comprese esclusioni e note esplicite, come richiesto.

### 7.1 Risorse Infrastrutturali (virtuali, in leasing)

| Risorsa | Scopo | Tipo / Modello |
| --- | --- | --- |
| Istanza AWS EC2 (piccola) | Ospita l'applicazione legacy Profis durante la transizione e il front-end MediaWiki | Virtuale, on-demand (in leasing) |
| Istanza Amazon RDS | Database per il MediaWiki consolidato | Database virtuale gestito (in leasing) |
| Storage Amazon S3 | Area di staging per la migrazione file; backup | Object storage (a consumo) |
| AWS Glue / DMS / DataSync | Strumenti di migrazione temporanei (dismessi dopo il cutover) | Servizi gestiti (a consumo) |

### 7.2 Esclusioni
- Nessun server fisico viene acquistato o installato. Tutte le VM on-premise dismesse (Zimbra, NextCloud, SugarCRM, Kimai, Samba4, MediaWiki) vengono disattivate, non sostituite.
- I dispositivi per utenti finali (laptop, monitor, periferiche) non fanno parte di questa offerta; i dispositivi esistenti vengono riutilizzati.
- Le apparecchiature di rete d'ufficio (router, switch, Wi-Fi) sono escluse.
- Non è richiesto alcun spazio in co-location o rack in data center.

### 7.3 Note
- Tutto il calcolo e lo storage sono in leasing on-demand da AWS; non vi sono asset hardware di proprietà/capitalizzati.
- L'infrastruttura AWS è volutamente minima e si prevede si riduca ulteriormente una volta dismesso completamente Profis (obiettivo: Q2 2027).
- La capacità hardware scala automaticamente con l'utilizzo; non è necessario alcun dimensionamento manuale o ciclo di rinnovo.

---

## 8. Attività Richieste

Il lavoro di implementazione è organizzato nelle categorie di attività standard riportate di seguito.

### 8.1 Installazione Hardware

Minima, a causa del modello SaaS-first:
- Provisioning della piccola istanza AWS EC2 per Profis e il front-end MediaWiki.
- Provisioning dell'istanza di database Amazon RDS per MediaWiki.
- Provisioning dello storage di staging Amazon S3 per la migrazione.

Nessun hardware fisico viene installato in rack, cablato o configurato.

### 8.2 Installazione e Configurazione Software
- Creare e configurare il tenant unificato Google Workspace.
- Configurare l'ambiente Odoo SaaS (CRM, Contabilità con localizzazione italiana, Timesheet).
- Attivare AWS IAM Identity Center; configurare la federazione SAML verso Google, OIDC verso Odoo e il provisioning SCIM.
- Consolidare i repository Bitbucket e riconfigurare le pipeline CI/CD.
- Distribuire e unire le istanze MediaWiki su AWS.
- Configurare gli strumenti di migrazione (AWS Glue, DMS, DataSync).

### 8.3 Test Funzionali

Ogni servizio è validato prima e dopo il cutover:
- Test SSO: un utente pilota accede una sola volta e raggiunge le app Google, Odoo e AWS senza riautenticarsi.
- Test flusso di posta: consegna in entrata/uscita confermata dopo il cutover MX.
- Test integrità dati: conteggio dei record riconciliato tra fonte e destinazione (CRM, contabilità, timesheet) entro la tolleranza concordata.
- Test accesso file: i file migrati si aprono con i permessi corretti per team.
- Test di Accettazione Utente (UAT): utenti designati di entrambe le aziende approvano ciascun servizio migrato.

### 8.4 Esclusioni
- Formazione oltre le sessioni standard di onboarding per utenti finali (formazione personalizzata aggiuntiva quotabile separatamente).
- Migrazione di dati più vecchi della finestra di conservazione concordata, se non specificamente richiesto.
- Integrazioni con applicazioni di terze parti non elencate nell'ambito.

### 8.5 Note
- Tutte le migrazioni avvengono mantenendo il sistema di origine disponibile in sola lettura fino all'approvazione, consentendo il rollback.
- I cutover sono programmati al di fuori dell'orario lavorativo per minimizzare le interruzioni.

---

## 9. Mappatura dei Servizi ai Criteri Richiesti

Ogni servizio è mappato rispetto ai criteri richiesti dal capitolato: modello di deployment, tipo di server, sistema operativo, open-source vs commerciale, di proprietà vs in leasing, modello di gestione e modello di prezzo.

| Servizio / Piattaforma | Deployment | Tipo server | SO | OSS / Commerciale | Proprietà / Leasing | Gestione | Prezzo |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Posta — Google Workspace | SaaS | Fornitore | n/d | Commerciale | Leasing | Esternalizzata | Per utente/mese |
| Produttività — Google Workspace | SaaS | Fornitore | n/d | Commerciale | Leasing | Esternalizzata | Per utente/mese |
| Conferenze — Google Meet/Chat | SaaS | Fornitore | n/d | Commerciale | Leasing | Esternalizzata | Incluso |
| Archiviazione — Google Drive | SaaS | Fornitore | n/d | Commerciale | Leasing | Esternalizzata | Incluso |
| CRM — Odoo SaaS | SaaS | Fornitore | n/d | Open-core | Leasing | Esternalizzata | Per utente/mese |
| Contabilità — Odoo SaaS | SaaS | Fornitore | n/d | Open-core | Leasing | Esternalizzata | Per utente/mese |
| Timesheet — Odoo SaaS | SaaS | Fornitore | n/d | Open-core | Leasing | Esternalizzata | Incluso |
| Controllo versioni — Bitbucket | SaaS | Fornitore | n/d | Commerciale | Leasing | Esternalizzata | Per utente/mese |
| Documentazione — MediaWiki | IaaS (AWS) | Virtuale (container/VM) | Linux | Open-source | Leasing | Interna (leggera) | A consumo |
| Legacy — Profis | IaaS (AWS) | VM virtuale | Windows | Commerciale | Leasing | Interna (leggera) | A consumo |
| Identità — AWS IAM Identity Center | SaaS (AWS) | Fornitore | n/d | Commerciale | Leasing | Interna (leggera) | Quasi gratuito |

L'infrastruttura è prevalentemente SaaS, in leasing e gestita dal fornitore, con una piccola componente AWS Linux/Windows sotto gestione interna leggera per i due carichi di lavoro privi di un equivalente SaaS adeguato.

---

## 10. Piano di Implementazione

L'implementazione si svolge in 13 settimane. Ogni servizio segue la stessa sequenza disciplinata: preparazione, migrazione pilota, test di accettazione utente, migrazione in produzione, con un percorso di rollback definito e una finestra di supporto post-migrazione.

### 10.1 Sequenza di Migrazione Standard (per servizio)

| Fase | Cosa accade |
| --- | --- |
| Preparazione | Inventario dei dati di origine, configurazione della destinazione, costruzione e prova a vuoto degli strumenti di migrazione. |
| Migrazione pilota | Migrare un piccolo sottoinsieme rappresentativo; validare integrità e accesso. |
| Test di Accettazione Utente | Utenti designati di entrambe le aziende confermano il funzionamento del servizio migrato. |
| Migrazione in produzione | Migrazione completa dei dati fuori orario; origine mantenuta in sola lettura. |
| Piano di rollback | In caso di fallimento della validazione, tornare all'origine in sola lettura; nessun dato viene eliminato fino all'approvazione. |
| Supporto post-migrazione | Finestra di hypercare con il team a disposizione per risolvere rapidamente i problemi. |

### 10.2 Cronoprogramma per Fasi

| Fase | Settimane | Attività principali |
| --- | --- | --- |
| Fase 1 — Fondamenta | 1–4 | Configurazione AWS IAM Identity Center; tenant Google e Odoo; inventario dati; progettazione pipeline |
| Fase 2 — Migrazione principale | 5–8 | Migrazioni di posta, archiviazione file, CRM, contabilità con pilota + UAT ciascuna |
| Fase 3 — Cutover identità | 9–10 | Test SSO/SCIM; dismissione di Samba4 e Google Authenticator; applicazione MFA |
| Fase 4 — Rimanenti + chiusura | 11–13 | Bitbucket, MediaWiki, contenimento legacy; UAT finale; documentazione; go-live |

### 10.3 Diagramma di Gantt

I blocchi ombreggiati (`█`) indicano le settimane in cui ogni flusso di lavoro è attivo nel cronoprogramma di 13 settimane.

```
Flusso di lavoro              | S1 S2 S3 S4 S5 S6 S7 S8 S9 S10 S11 S12 S13
-----------------------------|------------------------------------------
Fondamenta / Identità        | ██ ██ ██ ██
Migrazione posta             |             ██ ██
Migrazione archiviazione     |             ██ ██
Migrazione CRM               |                ██ ██
Migrazione contabilità       |                   ██ ██
Cutover identità (SSO/MFA)   |                         ██ ██
Controllo versioni (Bitbucket)|                           ██  ██
Consolidamento MediaWiki     |                            ██  ██  ██
Contenimento legacy (Profis) |                                ██  ██
UAT, documenti e go-live     |                                    ██  ██
```

Le fondamenta dell'identità vengono per prime; le migrazioni dati seguono in ondate parallele; il cutover dell'identità e la chiusura completano il programma.

### 10.4 Matrice di Responsabilità RASCI

R = Responsabile, A = Accountable (Garante), S = Supportivo, C = Consultato, I = Informato.
Ruoli: PL Project Lead, CE Ingegnere Cloud/Identità, DE Ingegnere Dati, OE Specialista Odoo, GE Ingegnere Collaborazione, QA Responsabile QA/Documentazione.

| Attività | PL | CE | DE | OE | GE | QA |
| --- | --- | --- | --- | --- | --- | --- |
| Configurazione Identità / SSO | A | R | C | I | I | S |
| Migrazione posta e file | A | S | R | I | R | C |
| Migrazione CRM e contabilità | A | I | R | R | I | C |
| Migrazione timesheet | A | I | R | R | I | C |
| Consolidamento controllo versioni | A | C | R | I | I | S |
| Consolidamento MediaWiki | A | R | S | I | I | C |
| Test funzionali e UAT | A | S | S | S | S | R |
| Documentazione e consegna | A | C | C | C | C | R |

---

## 11. Gestione dei Rischi

I principali rischi di migrazione e le relative mitigazioni:

| Rischio | Mitigazione |
| --- | --- |
| Perdita di dati durante la migrazione | Sistemi di origine mantenuti in sola lettura fino all'approvazione; migrazioni ripetibili; backup in stage su S3 prima del cutover. |
| Interruzione del servizio | Cutover programmati fuori orario; funzionamento in parallelo affinché gli utenti continuino a lavorare durante la transizione. |
| Errore di autenticazione al cutover | Pilota SSO con un piccolo gruppo prima; Samba4 mantenuto come fallback per due settimane; abilitazione graduale dell'MFA. |
| Resistenza degli utenti / bassa adozione | Il personale Pro Studio non vede alcun cambiamento; il personale Themea passa a strumenti chiaramente migliori; sessioni di onboarding e guide rapide. |
| Problemi di integrazione tra piattaforme | SCIM e SSO validati in un tenant di test prima della produzione; riconciliazione del conteggio record dopo ogni migrazione. |
| Lacune di conformità fiscale italiana (Profis → Odoo) | Localizzazione italiana di Odoo configurata e verificata; Profis mantenuto in sola lettura come riferimento fino all'approvazione contabile. |

---

## 12. Criteri di Successo (KPI)

Obiettivi misurabili utilizzati per confermare il successo dell'implementazione:

| KPI | Obiettivo |
| --- | --- |
| Tempo di fermo pianificato per servizio al cutover | < 2 ore |
| Completezza della migrazione utenti | 100% degli utenti attivi |
| Disponibilità del servizio dopo il go-live | ≥ 99,9% |
| Incidenti di sicurezza critici durante l'implementazione | Zero |
| Accuratezza migrazione dati (riconciliazione record) | ≥ 99,5% |
| Soddisfazione utenti dopo l'implementazione (sondaggio) | ≥ 95% positivo |
| Copertura single sign-on | 100% delle applicazioni in ambito |

---

## 13. Backup e Disaster Recovery

Le responsabilità di backup e ripristino sono condivise tra i fornitori SaaS e la piccola infrastruttura AWS.

| Parametro | Piattaforme SaaS (Google, Odoo, Bitbucket) | Ospitato su AWS (MediaWiki, Profis) |
| --- | --- | --- |
| Recovery Time Objective (RTO) | Secondo SLA del fornitore (tipicamente ore) | < 4 ore |
| Recovery Point Objective (RPO) | Quasi continuo (gestito dal fornitore) | ≤ 24 ore (snapshot giornalieri) |
| Conservazione backup | Default del fornitore + archivi esportati | 30 giorni (AWS Backup) |
| Ridondanza | Multi-regione del fornitore | Multi-AZ nella regione AWS Milano |
| Test di ripristino | Test di ripristino annuale | Test di ripristino semestrale |

---

## 14. Offerta Economica (Costi)

I costi sono suddivisi in implementazione una tantum (compresi i costi nascosti spesso omessi dalle stime SaaS) e costi ricorrenti annuali di abbonamento/infrastruttura. Le cifre sono stime di pianificazione indicative.

### 14.1 Costi di Implementazione Una Tantum

| Voce | Costo stimato |
| --- | --- |
| Configurazione infrastruttura AWS (EC2, RDS, S3) | €10.000 |
| Strumenti e ingegneria migrazione dati (Glue, DMS, DataSync) | €25.000 |
| Configurazione tenant Google Workspace e migrazione posta/file | €8.000 |
| Configurazione Odoo (CRM, Contabilità, Timesheet) | €12.000 |
| Identità: configurazione federazione SSO e provisioning SCIM | €6.000 |
| Funzionalità SSO/directory premium (se necessarie) | €3.000 |
| Servizi professionali / consulenza | €10.000 |
| Formazione utenti e sessioni di onboarding | €5.000 |
| Funzionamento temporaneo in parallelo durante la migrazione | €4.000 |
| **Subtotale** | **€83.000** |
| Contingenza (10%) | €8.300 |
| **Totale una tantum (setup Anno 1)** | **€91.300** |

### 14.2 Costi Ricorrenti Annuali

| Voce | Costo annuale |
| --- | --- |
| Google Workspace (≈100 utenti) | €12.000 |
| Odoo SaaS (CRM/Contabilità/Timesheet) | €24.000 |
| Bitbucket SaaS (≈20 sviluppatori) | €2.400 |
| Infrastruttura AWS (EC2, RDS, storage, identità) | €12.000 |
| Supporto fornitore e formazione periodica | €5.000 |
| **Totale ricorrente (annuo)** | **€55.400** |

### 14.3 Costo Totale di Proprietà su Tre Anni

| Periodo | Costo |
| --- | --- |
| Anno 1 (setup €91.300 + ricorrente €55.400) | €146.700 |
| Anno 2 (ricorrente) | €55.400 |
| Anno 3 (ricorrente) | €55.400 |
| **Totale triennale** | **€257.500** |

L'Anno 1 combina il setup una tantum con il primo anno di costi ricorrenti. Gli Anni 2 e 3 aggiungono ciascuno un anno intero di costi ricorrenti, dando un totale triennale corretto di circa €257.500.

---

## 15. Documentazione

La seguente documentazione viene prodotta e consegnata come parte dell'incarico:

- Documento di architettura della soluzione (questa offerta più i diagrammi as-built aggiornati al go-live).
- Guida di configurazione di identità e SSO (impostazioni di federazione, mappature SCIM, policy MFA).
- Runbook di migrazione per servizio, comprese le procedure di rollback.
- Report di riconciliazione dati per ciascun servizio migrato (conteggio record, risultati di validazione).
- Guida amministratore per l'infrastruttura AWS residua (EC2, RDS, backup).
- Guide rapide per utenti finali su Google Workspace e Odoo.
- Procedure di backup e disaster recovery con il calendario dei test.
- Report finale di chiusura del progetto che mappa il lavoro consegnato rispetto ai KPI della Sezione 12.

---

## 16. Termini e Condizioni

### 16.1 Validità
Questa offerta è valida per 60 giorni dalla data riportata in copertina. Le stime dei costi sono indicative e soggette a conferma del numero finale di utenti e dei listini SaaS al momento del contratto.

### 16.2 Modello di prezzo
L'implementazione è quotata come incarico a perimetro fisso. I costi ricorrenti delle piattaforme sono fatturati dai rispettivi fornitori su base di abbonamento per utente; l'infrastruttura AWS è fatturata a consumo. I costi sono al netto dell'IVA.

### 16.3 Presupposti
- Entrambe le aziende forniscono accesso tempestivo ai sistemi di origine, le credenziali amministrative (inserite dal proprio personale, non condivise in chiaro) e gli utenti UAT designati.
- I dispositivi per utenti finali e le reti d'ufficio esistenti sono funzionanti e riutilizzati.
- I volumi di dati rientrano negli intervalli usati per la stima; volumi sensibilmente maggiori possono influire su tempi e costi.

### 16.4 Responsabilità
Il cliente è responsabile dell'approvazione aziendale a ciascun gate UAT e dell'approvazione delle finestre di cutover. MOSAIC Group è responsabile della consegna delle attività in ambito, della documentazione e dei KPI concordati.

### 16.5 Controllo delle modifiche
Il lavoro al di fuori dell'ambito definito (Sezione 2) è gestito tramite una richiesta di modifica scritta con una propria stima e non è incluso nel prezzo quotato.

### 16.6 Riservatezza
Tutte le informazioni scambiate durante l'incarico sono trattate come riservate e utilizzate esclusivamente ai fini di questa integrazione.

---

## 17. Conclusione

La soluzione proposta standardizza l'azienda unificata su due ecosistemi SaaS collaudati — Google Workspace e Odoo — con AWS IAM Identity Center come broker di identità neutrale rispetto ai fornitori e una infrastruttura AWS volutamente ridotta per i pochi carichi di lavoro privi di equivalente SaaS. In questo modo offre una serie di risultati chiari:

- **Complessità operativa ridotta** — dieci sistemi frammentati e self-hosted si riducono a due piattaforme gestite più un livello AWS minimo.
- **Costi inferiori a lungo termine** — abbonamenti per utente prevedibili sostituiscono il costo nascosto del lavoro di patching e manutenzione dei singoli server.
- **Sicurezza migliorata** — autenticazione centralizzata, autenticazione a più fattori obbligatoria e audit unificato su ogni applicazione.
- **Amministrazione più semplice** — un'unica directory governa onboarding, offboarding e accessi per tutte le piattaforme tramite provisioning automatico.
- **Scalabilità futura** — la capacità SaaS cresce automaticamente con l'organico, e il livello di identità basato su standard rende a basso rischio qualsiasi futura modifica di piattaforma.

Il risultato è un unico ambiente di lavoro digitale unificato che consente a entrambe le aziende di concentrarsi sulla consegna al cliente piuttosto che sulla gestione dell'infrastruttura, realizzabile entro il programma di 13 settimane definito in questa offerta.
