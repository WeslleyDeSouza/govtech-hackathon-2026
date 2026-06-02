# Freddy's Companion / Digital Companion for Unemployment

**Freddy's Companion** is a GovTech Hackathon 2026 prototype: a plain-language companion that
guides a person through the Swiss unemployment journey - from the first letter from the RAV
to the first payout.

> [!NOTE]
> This is an **MVP built for demo purposes only** — a clickable prototype to illustrate the
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

`index.html` is a client-side single-page app: every screen is toggled by `goToView()` and
the `open*()` helpers. There is no router and no server.

```mermaid
flowchart TD
  V0["Splash / Welcome"]:::on --> V1["Language select"]:::on
  V1 --> V3["Situation"]:::on
  V3 --> V12["Choose Arbeitslosenkasse"]:::on
  V12 --> V13["RAV verification (AGOV login)"]:::on
  V13 --> V10["Plan generation (loading)"]:::on
  V10 --> V11["Home dashboard"]:::home

  %% alternate help paths
  V3 -.no RAV yet.-> V5["RAV help path"]:::alt
  V5 --> V6["AGOV status"]:::alt
  V6 --> V9["Letter scan & explain"]:::alt
  V9 --> V10

  %% sub-screens reachable from Home
  V11 --> A["Antrag (view / edit)"]:::sub
  V11 --> B["Arbeitsbemühungen"]:::sub
  V11 --> P["AVP-Meldung"]:::sub
  V11 --> E["Emergency / help sheet"]:::sub
  A -. "remind employer" .-> AG["Arbeitgeber-Status"]:::wait

  classDef on fill:#EEEDFE,stroke:#534AB7,color:#26215C
  classDef home fill:#EAF3DE,stroke:#3B6D11,color:#173404
  classDef alt fill:#E6F1FB,stroke:#185FA5,color:#0C447C
  classDef sub fill:#ffffff,stroke:#888780,color:#2c2c2a
  classDef wait fill:#FAEEDA,stroke:#BA7517,color:#633806
```

> Demo persona: **Freddy Fremd**, Personennummer `12345678`, **RAV Bern** — the ex-employer's
> certificate is overdue, so the application is blocked until it arrives.
