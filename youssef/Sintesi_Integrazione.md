# Integrazione IT Themea × Pro Studio — Sintesi

**Strategia:** SaaS "Best-of-Breed" + AWS come backbone di identità

---

## In breve

| | |
| --- | --- |
| **Tempi** | 6 mesi (go-live dicembre 2026) |
| **Costo Anno 1** | ~€102K (setup + abbonamenti) |
| **Costo annuo** | ~€35K |
| **Personale IT** | 1 persona |
| **Utenti** | ~50 |

**Idea di base:** adottiamo gli strumenti che Pro Studio usa già (Google Workspace + Odoo), spostiamo i dati di Themea su di essi, e usiamo AWS solo per il login unico (SSO) e le poche app legacy. Pro Studio non cambia nulla; Themea passa a strumenti migliori.

---

## Architettura

```
        AWS IAM Identity Center  ← un solo login per tutto
                 │
     ┌───────────┼───────────┐
     ▼           ▼           ▼
  Google      Odoo SaaS    AWS
  Workspace   (CRM,        (Profis,
  (posta,     contabilità, MediaWiki)
  file,       timesheet)
  Meet)
```

---

## Cosa scegliamo per ogni servizio (e perché)

**Posta / Calendario → Google Workspace**
Themea usa Zimbra self-hosted (server da mantenere). Pro Studio è già su Google. Scegliamo Google: zero manutenzione server, condivisione e calendario migliori, metà azienda non cambia nulla.

**Documenti / File → Google Workspace + Drive**
Themea ha un mix frammentato (MS365 + LibreOffice + NextCloud). Google offre collaborazione in tempo reale nel browser, cronologia versioni illimitata e un unico archivio file affidabile, senza server da gestire.

**Videoconferenza / Chat → Google Meet + Chat**
Themea usa Teams, Pro Studio usa Meet. Consolidiamo su Meet/Chat: un solo strumento, integrato con la posta, più semplice da usare.

**CRM → Odoo SaaS**
Themea usa SugarCRM (interfaccia datata, scollegata dalla contabilità). Pro Studio è già su Odoo. Odoo collega nativamente CRM, contabilità e fatturazione in un unico sistema.

**Contabilità → Odoo SaaS**
Themea usa Profis (legacy su server Windows). Odoo è moderno, ha la localizzazione fiscale italiana aggiornata e si integra con la fatturazione del CRM.

**Rilevamento ore → Odoo SaaS**
Themea usa Kimai (strumento separato). In Odoo le ore sono collegate ai progetti e alla fatturazione automatica — nessuno strumento extra da mantenere.

**Controllo versioni → Bitbucket**
Themea usa GiTea self-hosted. Pro Studio è già su Bitbucket, che ha pipeline CI/CD integrate. Spostiamo solo i repo di Themea.

**Documentazione → MediaWiki su AWS**
Entrambe le aziende usano già MediaWiki: nessuna riformazione. Lo manteniamo su una piccola infrastruttura AWS a basso costo, unendo i due wiki in uno.

**Identità / Login → AWS IAM Identity Center**
Themea usa Active Directory (Samba4), Pro Studio Google Authenticator. AWS IAM Identity Center fa da "collante": un solo login (con MFA) per Google, Odoo e AWS, indipendente da un singolo fornitore.

### Tabella riassuntiva

| Servizio | Scelto | Motivo principale |
| --- | --- | --- |
| Posta / Calendario | Google Workspace | Già in Pro Studio, niente server da mantenere |
| Documenti / File | Google Workspace + Drive | Collaborazione in tempo reale, archivio unico |
| Videoconf. / Chat | Google Meet + Chat | Un solo strumento, più semplice di Teams |
| CRM | Odoo SaaS | Già in Pro Studio, collegato alla contabilità |
| Contabilità | Odoo SaaS | Moderno, localizzazione fiscale italiana |
| Rilevamento ore | Odoo SaaS | Collegato ai progetti, fatturazione automatica |
| Controllo versioni | Bitbucket | Già in Pro Studio, CI/CD integrato |
| Documentazione | MediaWiki su AWS | Entrambi lo usano, nessuna riformazione |
| Identità / Login | AWS IAM Identity Center | Login unico per Google, Odoo e AWS |

---

## Tempistica (13 settimane)

```
Settimane 1-4   → Setup (identità, Google, Odoo)
Settimane 5-8   → Migrazione dati (posta, file, CRM, contabilità)
Settimane 9-10  → Cutover identità (login unico attivo)
Settimane 11-13 → Sistemi rimanenti, pulizia, go-live
```

---

## Costi (≈50 utenti)

| | |
| --- | --- |
| **Setup una tantum** | ~€67K |
| **Costo ricorrente** | ~€35K/anno |
| **Anno 1 (setup + 1° anno)** | ~€102K |
| **Totale 3 anni** | ~€173K |

---

## Perché questo piano

- **Veloce** — operativi in 6 mesi
- **Economico** — niente sviluppo custom, abbonamenti prevedibili
- **Semplice** — i fornitori gestiscono la manutenzione, non noi
- **Pochi cambiamenti** — metà azienda (Pro Studio) non tocca nulla
- **Sicuro** — login unico con MFA su tutto

---

*Versione di sintesi per discussione interna. Il documento completo (offerta tecnica ed economica dettagliata) è disponibile separatamente.*
