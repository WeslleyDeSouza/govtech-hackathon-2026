# Freddy's Companion / Digital Companion for Unemployment

**Freddy's Companion** is a GovTech Hackathon 2026 prototype: a plain-language companion that
guides a person through the Swiss unemployment journey - from the first letter from the RAV
to the first payout.

🔗 **GovTech page:** https://www.bk.admin.ch/de/govtechhackathon26

🔗 **Project page:** https://govtech.digisus-lab.ch/project/36

---

> [!NOTE]
> This is an **MVP built for demo purposes only** - a clickable prototype to illustrate the
> flow and user experience, not a production-ready application.

![Banner](Grafik-Hackathon-2026-blau-weisser-hintergrund.png)

----

### User Story – Onboarding

The linear flow a new user walks through, from launch to a personal plan.

```mermaid
journey
  title Onboarding: from first launch to personal plan
  section Welcome
    Open app, see splash & trust badge: 0: User
  section Language
    Choose UI language (DE/UK/AR/PT/TI/EN): 0: User
  section Situation
    Describe situation (lost job / got notice / at risk / other): 0: User
  section Apply for benefits
    Pick unemployment fund (Arbeitslosenkasse): 0: User
  section RAV verification
    Confirm RAV registration via AGOV login: 0: User
    Fetch RAV/Job-Room data securely (no re-typing): 0: System
  section Plan generation
    Analyse language, situation & deadlines: 0: System
    Set reminders, build personal plan: 0: System
  section Home
    Land on dashboard with next step & timeline: 0: System
```

----

### User Story – Ongoing tasks (Home dashboard)

After onboarding the user lives on the Home screen, which separates **one-time** from
**monthly** obligations and surfaces blockers.

```mermaid
journey
  title Ongoing: managing the claim from Home
  section One-time steps
    RAV registration (done): 0: User
    AGOV account created (done): 0: User
    Submit benefit application (Antrag): 0: User
    Wait for employer certificate (blocked on ex-employer): 0: System
  section Application (Antrag)
    View pre-filled application: 0: User
    Edit via form wizard (Formular 10000d): 0: User
    Remind ex-employer for the missing part: 0: User
  section Monthly duties
    File AVP-Meldung (Angaben der versicherten Person): 0: User
    Log Arbeitsbemühungen (job-search efforts): 0: User
  section Support
    Open emergency / help mode anytime: 0: User
```

----

### Screen flow

These situations and their paths are designed for the demo. The branching shown here illustrates the intended logic — a production implementation would differ, with real eligibility rules and support routes defined together with the responsible authorities.

```mermaid
flowchart TD
  V0["Splash / Welcome"]:::on --> V1["Language select"]:::on
  V1 --> V3["Situation"]:::on

  V3 --> S1["Lost job · already unemployed"]:::opt
  V3 --> S2["Got notice · still working"]:::opt
  V3 --> S3["At risk · only a suspicion"]:::opt
  V3 --> S4["Something else · re-entry / exhausted"]:::opt

  %% full application path (needs a fund)
  S1 --> V12["Choose Arbeitslosenkasse"]:::on
  S2 --> V12
  V12 --> V13["RAV verification (AGOV login)"]:::on
  V13 --> V10["Plan generation"]:::on

  %% support paths (no fund needed)
  S3 -. "no application yet" .-> SUP["Support & info · early RAV registration"]:::alt
  S4 -. "special case" .-> SUP2["Support & referral · e.g. Sozialdienst"]:::alt
  SUP --> V10
  SUP2 --> V10

  V10 --> V11["Home dashboard"]:::home

  V11 --> A["Antrag (view / edit · 10000d)"]:::sub
  V11 --> B["Arbeitsbemühungen"]:::sub
  V11 --> P["AVP-Meldung"]:::sub
  V11 --> E["Emergency / help sheet"]:::sub
  A -. "remind employer" .-> AG["Arbeitgeber-Status"]:::wait

  classDef on fill:#EEEDFE,stroke:#534AB7,color:#26215C
  classDef opt fill:#F7F6FD,stroke:#9990D8,color:#26215C
  classDef alt fill:#E6F1FB,stroke:#185FA5,color:#0C447C
  classDef home fill:#EAF3DE,stroke:#3B6D11,color:#173404
  classDef sub fill:#ffffff,stroke:#888780,color:#2c2c2a
  classDef wait fill:#FAEEDA,stroke:#BA7517,color:#633806
```

> Demo persona: **Freddy Fremd**, Personennummer `12345678`, **RAV Bern** — the ex-employer's
> certificate is overdue, so the application is blocked until it arrives.
