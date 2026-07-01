# 📊 Piano di Ammortamento e Revisione Economica dell'Integrazione IT

**Destinatario:** Comitato Direttivo TP Group srl / Direzione Finanziaria  
**Redatto da:** Area Amministrazione & Controllo di Gestione (MOSAIC Group)  
**Versione:** 2.0 (Ottimizzata FinOps & Compliance)  
**Data:** 1 luglio 2026  
**Classificazione:** Riservato — Uso Interno  

---

## 🏢 1. Scenario Macroeconomico Modificato

Come da ultime direttive della governance aziendale, l'analisi comparativa dei costi totali di proprietà (TCO) a lungo termine per le due soluzioni macroscopiche a confronto è stata aggiornata ufficialmente come segue:

* **Soluzione di Calcolo On-Premise / Hosting:** **€ 580.000** (Costi rigidi di capitale, manutenzione e adeguamento locali saturati)
* **Soluzione di Calcolo On-Cloud:** **€ 525.000** (Modello flessibile basato su SaaS e infrastruttura AWS ottimizzata)

---

## 👥 2. Profilazione Organica del Personale (Drivers di Costo)

L'efficientamento economico della versione 2.0 si basa sulla reale allocazione delle risorse umane all'interno di **TP Group srl** (50 dipendenti totali):

* **80% Personale Tecnico / Sviluppatori (40 Risorse):** Operano prevalentemente sui flussi di codice (Bitbucket), terminali Linux e sistemi Cloud di sviluppo. Non necessitano dell'accesso a funzioni commerciali o ERP fiscali.
* **20% Personale Amministrativo / Contabile (10 Risorse):** Risorse core deputate alla gestione del business, fatturazione attiva/passiva, relazioni commerciali e contabilità generale.

---

## ⚙️ 3. Analisi Critica del Piano dei Costi (`costi.txt`)

Il file `costi.txt` è stato esaminato dal Controllo di Gestione. Di seguito si analizza la sua aderenza alle normative, alle richieste del cliente e ai vincoli di efficienza.

### 📜 3.1 Adempimento dei Vincoli e delle Normative
* **Compliance Finanziaria (Profis):** **Sì.** Il piano include l'obbligo di mantenere attivo il software legacy *Profis* per 1 anno. Questa scelta tutela legalmente l'azienda, garantendo l'accessibilità immediata ai dati fiscali storici per l'ispezione tributaria e l'audit durante il primo anno di transizione.
* **Conformità ISO 27001 (Backup):** **Sì.** L'approccio ibrido (copia locale + bucket S3 cifrato e geograficamente ridondato a ~100€/anno) adempie pienamente ai requisiti di delocalizzazione dei dati e Disaster Recovery previsti dalla norma.
* **Richieste del Cliente (Copertura Servizi):** **Sì.** Tutti i servizi fondamentali richiesti (Posta, Office, Collaboration, Repository Codice, KB Interna, Identity Management) sono coperti ed economicamente quantificati.

### 📉 3.2 Correzione dei Costi Odoo e Ottimizzazione FinOps
Il file originario prevedeva solo 10 licenze Odoo. Questo è corretto **solo se si profila l'azienda**: le 10 licenze *Odoo Standard* (€1.428/anno) coprono interamente il **20% del personale amministrativo**. Per il restante **80% (i 40 sviluppatori)**, l'inserimento dei fogli ore (*Timesheet*) viene integrato a costo zero tramite moduli o flussi alternativi estranei alle costose licenze commerciali, salvaguardando il budget.

---

## 💰 4. Tabelle Comparative dei Costi (Prima vs Dopo)

### 4.1 Costi Ricorrenti Annuali (Licensing e Cloud)

| Servizio / Voce di Spesa | Costo Iniziale Stimato (v1.0) | Nuovo Costo Ottimizzato (v2.0) | Risparmio Annuale | Note di Ottimizzazione (v2.0) |
| :--- | :---: | :---: | :---: | :--- |
| **Google Workspace Standard** | € 8.160 | € 8.160 | € 0 | Fornitura completa per tutti i 50 dipendenti |
| **Odoo Standard** | € 3.570 (25 ut.) | € 1.428 | **€ 2.142** | Profilato solo sui 10 dipendenti Admin (20%) |
| **Bitbucket Standard** | € 1.920 | € 1.920 | € 0 | Destinato esclusivamente ai 40 sviluppatori (80%) |
| **MediaWiki (Knowledge Base)** | € 1.200 | € 0 (FREE) | **€ 1.200** | Hostata a costo zero su istanza GCP/AWS pre-esistente |
| **Backup Cifrato (Local + S3)** | € 400 | € 100 | **€ 300** | Ottimizzazione dello storage delocalizzato S3 |
| **TOTALE RICORRENTE** | **€ 15.250** | **€ 11.608** | **€ 3.642 (23.8%)** | **Spesa corrente annua stabilizzata** |

### 4.2 Costi Una Tantum (Setup e Migrazione Anno 1)

| Voce di Setup | Costo Iniziale (v1.0) | Nuovo Costo Ottimizzato (v2.0) | Risparmio Una Tantum | Razionale della Modifica |
| :--- | :---: | :---: | :---: | :--- |
| Setup AWS IAM Identity Center | € 2.500 | € 500 | **€ 2.000** | Configurazione snella su licenze core native FREE |
| Manodopera Migrazione App | € 4.000 | € 1.000 | **€ 3.000** | Trasferimento interno per Kimai, SugarCRM e Wiki |
| **TOTALE UNA TANTUM** | **€ 6.500** | **€ 1.500** | **€ 5.000 (76.9%)** | **Investimento iniziale minimizzato** |

---

## 📉 5. Strategia di Abbattimento dei Costi del Server Legacy (Profis)

Per evitare che l'istanza *Profis* (necessaria per un anno) pesi in modo lineare sulle finanze aziendali, vengono applicate le seguenti misure di controllo:

1.  **Automazione Oraria (AWS Instance Scheduler):** Il server rimarrà attivo solo dal lunedì al venerdì (08:00 - 18:00). Questo riduce l'allocazione oraria da 720 ore a sole **200 ore al mese**, abbattendo del **72%** la tariffa computazionale di AWS senza impattare sul lavoro del team contabile.
2.  **Transizione a Sola Lettura (Mese 6):** Al sesto mese il database viene bloccato in modalità *Read-Only*. Il personale effettuerà l'estrazione massiva di tutti i registri storici in PDF e CSV. Una volta completato il travaso su storage statico (Google Drive), il server EC2 potrà essere terminato anzitempo, dimezzando la durata effettiva della macchina.

---

## 🛡️ 6. Assunzioni e Governance di Sicurezza (ISO 27001)

* **Segregazione dei Ruoli (SoD):** L'accesso ai dati sensibili e contabili del server legacy e di Odoo viene ristretto esclusivamente ai 10 utenti amministrativi (20% dell'organico). Ai 40 sviluppatori viene inibita qualsiasi visibilità fiscale, riducendo la superficie di rischio interno (Insider Risk).
* **Tracciabilità Immutabile:** Ogni operazione di accensione, spegnimento e consultazione del server legacy viene registrata automaticamente tramite i log immutabili del Cloud, garantendo conformità totale in fase di Audit ISO 27001.
