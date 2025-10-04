# Piano di Lavoro Tecnico (con Testing): Piattaforma di Orientamento Professionale

## 1. Introduzione
Questo documento scompone l'implementazione definita nel `design.md` in task tecnici, includendo la scrittura di test di unità e end-to-end, con un focus sull'ecosistema Next.js e Firebase.

## 2. Task 0: Setup del Progetto e Dipendenze
* **T00**: Inizializzazione del progetto con Next.js (`create-next-app`).
* **T01**: Installazione delle dipendenze di produzione: `firebase`, `@mui/material`, `@emotion/react`, `@emotion/styled`.
* **T02**: Installazione delle dipendenze di sviluppo (testing): `vitest`, `@vitejs/plugin-react`, `jsdom`, `@testing-library/react`, `@playwright/test`.
* **T03**: Configurazione dell'ambiente: linter (ESLint), formatter (Prettier), setup delle variabili d'ambiente (`.env.local`) per la configurazione di Firebase.
* **T04**: Creazione della struttura di directory di base (`/src/app`, `/src/components`, `/src/lib`, `/tests` per Playwright).

## 3. Suddivisione dei Task di Sviluppo e Testing

| ID    | Funzionalità/Componente        | Descrizione Tecnica                                                                      | File da Creare/Modificare                                       | Dipendenze (ID Task) |
|:------|:-------------------------------|:-----------------------------------------------------------------------------------------|:----------------------------------------------------------------|:---------------------|
| **T05** | **Setup Firebase** | Creare il file di configurazione del client Firebase e le utility per l'accesso a Firestore e Auth. | `src/lib/firebase.ts`                                           | T01, T03             |
| **T06** | **Setup Autenticazione** | Configurare Firebase Authentication e creare le Route Handlers per login/logout se necessario. | `src/app/api/auth/[...nextauth]/route.ts` (se si usa NextAuth), o utility custom in `src/lib/auth.ts` | T05                  |
| **T07** | **Componente UI: Auth Buttons** | Creare i componenti client per i bottoni di Login (con Google) e Logout.                 | `src/components/auth-buttons.tsx`                               | T06                  |
| **T08** | **TEST DI UNITÀ: Auth Buttons** | Scrivere test di unità (Vitest) per verificare il rendering e l'interazione dei bottoni. | `src/components/auth-buttons.test.tsx`                          | T07                  |
| **T09** | **Layout Applicazione** | Creare il layout principale (`RootLayout`) con header, footer e integrazione del ThemeProvider di MUI. | `src/app/layout.tsx`                                            | T07                  |
| **T10** | **Pagina: Homepage** | Sviluppare la landing page statica (SSG) con il motore di ricerca.                       | `src/app/page.tsx`, `src/components/search-bar.tsx`             | T09                  |
| **T11** | **Pagina: Lista Professionisti** | Creare la pagina che mostra la lista dei professionisti, usando ISR per dati aggiornati. | `src/app/professionisti/page.tsx`, `src/components/professional-card.tsx` | T05                  |
| **T12** | **Pagina: Dettaglio Professionista** | Creare la pagina del profilo pubblico di un professionista (ISR).                        | `src/app/professionisti/[id]/page.tsx`                          | T05                  |
| **T13** | **TEST DI UNITÀ: ProfessionalCard** | Testare il componente `ProfessionalCard` per assicurarsi che renderizzi correttamente i dati. | `src/components/professional-card.test.tsx`                     | T11                  |
| **T14** | **TEST E2E: Flusso Autenticazione** | Scrivere un test Playwright che simula il login di un utente tramite il provider Google. | `tests/auth.spec.ts`                                            | T07, T09             |
| **T15** | **TEST E2E: Ricerca e Visualizzazione** | Test Playwright: l'utente cerca una categoria, naviga alla lista, clicca su un profilo e ne verifica il contenuto. | `tests/search.spec.ts`                                          | T10, T11, T12        |
