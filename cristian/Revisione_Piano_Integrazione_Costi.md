# Revisione e Ottimizzazione dei Costi del Piano di Integrazione

**Versione:** 2.0 (Ottimizzata)
**Classificazione:** Riservato — Uso Interno 

**Strategia di Revisione:** Ottimizzazione del Footprint di Licensing basata sulla profilazione reale delle risorse (IT Cost Optimization).

---

## Indice

1. Executive Summary
2. Nuova Distribuzione del Personale
3. Strategia di Ottimizzazione del Licensing (SaaS)
4. Analisi Comparativa dei Costi Ricorrenti Annuali
5. Analisi Comparativa dei Costi Una Tantum (Setup Anno 1)
6. Proiezione del Total Cost of Ownership (TCO) a 3 Anni
7. Assunzioni Amministrative e Governance

---

## 1. Executive Summary

A seguito dell'analisi della proposta iniziale emessa da *MOSAIC Group* (Versione 1.0), l'area Amministrazione e Controllo di Gestione ha identificato significative opportunità di efficientamento economico. La versione precedente applicava una metrica di licenziamento "flat" (lineare) sull'intero organico di 50 utenti.

Introducendo una **profilazione granulare delle mansioni** basata sulla reale operatività aziendale post-fusione, siamo in grado di ridurre drasticamente sia l'esborso iniziale (*Una Tantum*) sia la spesa corrente (*Ricorrente Annuale*), migliorando il margine operativo senza compromettere l'efficacia tecnologica della convergenza IT.

---

## 2. Nuova Distribuzione del Personale

Il calcolo economico aggiornato si basa sulla reale allocazione delle risorse umane all'interno di **TP Group srl** (50 dipendenti totali):

* **80% Sviluppatori / Tecnici (40 Risorse):** Personale proveniente prevalentemente da *Pro Studio srl*. Operano su Bitbucket, piattaforme cloud di sviluppo e terminali Linux. Non partecipano alle attività commerciali (CRM) né amministrative.
* **20% Personale CRM, Amministrazione e Contabilità (10 Risorse):** Personale "Core" deputato alla gestione del business, fatturazione, contabilità fiscale e relazioni commerciali.

---

## 3. Strategia di Ottimizzazione del Licensing (SaaS)

### 3.1 Profilazione Odoo SaaS

* **Approccio Precedente:** Licenze generiche per circa 25 utenti senza distinzione di ruolo.
* **Nuovo Approccio:** Solo le 10 risorse amministrative/CRM necessitano della suite completa *Odoo Enterprise* (Contabilità, CRM, Fatturazione). Per i 40 sviluppatori, l'inserimento dei fogli ore (*Timesheet*) viene gestito tramite moduli economici dedicati o configurazioni d'accesso limitate, azzerando la necessità di licenze commerciali complete per l'80% dell'azienda.

### 3.2 Profilazione Google Workspace

* **Approccio Precedente:** 50 licenze uniformi su piano *Business Standard* (~€10/utente/mese).
* **Nuovo Approccio:** * **40 Utenti Sviluppatori:** Profilati su piano *Business Starter* (€5.75/utente/mese), in quanto il loro storage principale risiede nei repository di codice (Bitbucket).
  * **10 Utenti Amministrativi/Commerciali:** Mantengono il piano *Business Standard* (€10.35/utente/mese) per la gestione di grandi volumi di documenti, contratti e funzionalità avanzate di riunione.

### 3.3 Infrastruttura AWS e Dismissione Software Legacy

Il costo dell'infrastruttura AWS (€12.000/anno) include il mantenimento temporaneo del software contabile legacy *Profis* (Thema Consulting). La Direzione Amministrativa stabilisce il completamento della migrazione dei dati storici entro il termine dell'Anno 1, permettendo lo spegnimento programmato dell'istanza EC2 Windows associata e un conseguente abbattimento stimato del 50% della quota AWS a partire dall'Anno 2.

---

## 4. Analisi Comparativa dei Costi Ricorrenti Annuali

La tabella seguente evidenzia il risparmio ottenuto applicando il licensing profilato rispetto all'offerta iniziale:

| Voce di Spesa                   | Costo Annuale Iniziale (v1.0) | Nuovo Costo Ottimizzato (v2.0) |     Risparmio Annuale     | Note di Ottimizzazione                                 |
| :------------------------------ | :---------------------------: | :----------------------------: | :-----------------------: | :----------------------------------------------------- |
| **Google Workspace**      |            €6.000            |            €4.002            |     **€1.998**     | 40 utenti Business Starter + 10 Business Standard      |
| **Odoo SaaS**             |           €12.000           |            €6.000            |     **€6.000**     | Riduzione utenti Core CRM/Contabilità a 10 risorse    |
| **Bitbucket SaaS**        |            €1.200            |            €1.200            |            €0            | Invariato (destinato ai 40 sviluppatori)               |
| **Infrastruttura AWS**    |           €12.000           |            €12.000            |            €0            | €12k stabili per Anno 1; riduzione a €6k dall'Anno 2 |
| **Supporto e Formazione** |            €4.000            |            €4.000            |            €0            | Invariato per canoni di manutenzione                   |
| **TOTALE ANNUALE**        |      **€35.200**      |       **€27.202**       | **€7.998 (22.7%)** | **Risparmio netto ricorrente consolidato**       |

---

## 5. Analisi Comparativa dei Costi Una Tantum (Setup Anno 1)

Interventi di riduzione sui costi di implementazione legati a formazione focalizzata e razionalizzazione dei processi di migrazione dati:

| Voce di Setup               | Costo Iniziale (v1.0) | Nuovo Costo Ottimizzato (v2.0) |   Risparmio Una Tantum   | Razionale della Modifica                                          |
| :-------------------------- | :-------------------: | :----------------------------: | :-----------------------: | :---------------------------------------------------------------- |
| Configurazione AWS          |        €8.000        |            €8.000            |            €0            | Setup base infrastruttura idoneo                                  |
| Ingegneria Migrazione Dati  |       €18.000       |            €12.000            |     **€6.000**     | Migrazione parziale tramite export nativi per sistemi open-source |
| Setup Google Workspace      |        €6.000        |            €6.000            |            €0            | Invariato per configurazione tenant                               |
| Configurazione Odoo         |        €9.000        |            €9.000            |            €0            | Configurazione moduli finanziari e fiscali italiani               |
| Identità e SSO (SCIM)      |        €5.000        |            €5.000            |            €0            | Invariato (fondamentale per sicurezza ISO 27001)                  |
| Licenze Premium SSO         |        €2.000        |            €2.000            |            €0            | Invariato                                                         |
| Servizi Professionali       |        €7.000        |            €7.000            |            €0            | Invariato                                                         |
| Formazione Utenti           |        €3.000        |            €1.500            |     **€1.500**     | Escluso il personale tecnico (già autonomo su Google/Bitbucket)  |
| Funzionamento in Parallelo  |        €3.000        |            €3.000            |            €0            | Necessario per garantire continuità di business                  |
| Contingenza (10%)           |        €6.100        |            €5.350            |      **€750**      | Ricalcolata sul nuovo subtotale                                   |
| **TOTALE UNA TANTUM** |  **€67.100**  |       **€58.850**       | **€8.250 (12.3%)** | **Abbattimento dell'investimento iniziale**                 |

---

## 6. Proiezione del Total Cost of Ownership (TCO) a 3 Anni

Il risparmio combinato (maggiorato dal calo del costo AWS a partire dall'Anno 2 grazie allo spegnimento del sistema legacy *Profis*) genera una riduzione drastica del costo totale di proprietà.

```
  Costo Totale Triennale (TCO)
  v1.0 (Iniziale)   [████████████████████████████████████████] €172.700
  v2.0 (Ottimizzato)[████████████████████████████] €125.254
  
  RISPARMIO TRIPLICE ANNO: €47.446 (Taglio del 27.5% sul budget IT globale)
```

### Dettaglio Flussi di Cassa

* **Anno 1 (Setup + Ricorrente):** €58.850 + €27.202 = **€86.052** *(Risparmio di €16.248 rispetto alla v1.0)*
* **Anno 2 (Solo Ricorrente):** **€21.202** *(Ulteriore risparmio di €6.000 dovuto al decommissioning di Profis su AWS)*
* **Anno 3 (Solo Ricorrente):** **€21.202**
* **TCO COMPLESSIVO TRIENNALE:** **€125.254** (contro i €172.700 iniziali).

---

## 7. Assunzioni Amministrative e Governance

Al fine di garantire la corretta applicazione di questo piano finanziario e l'allineamento con i requisiti **ISO/IEC 27001**, si stabiliscono le seguenti assunzioni:

1. **Segregazione dei Ruoli (SoD):** Alle 40 risorse identificate come "Sviluppatori" verranno revocati i diritti di accesso in scrittura e lettura ai moduli finanziari e commerciali di Odoo, riducendo l'esposizione al rischio di data-breach e limitando l'uso delle licenze d'uso ai soli scopi di rilevamento ore (Timesheet).
2. **Audit Periodico:** La Direzione Amministrativa effettuerà una revisione trimestrale degli account attivi in *AWS IAM Identity Center* per assicurare che il provisioning automatico (SCIM) mantenga aggiornati i profili di licenza Google Starter/Standard senza derive di costo.
3. **Pianificazione Ammortamenti:** I costi una tantum ottimizzati (€58.850) saranno capitalizzati e ammortizzati in quote costanti nell'arco del triennio di riferimento del progetto.

