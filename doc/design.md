# Documento di Progettazione: Piattaforma di Orientamento Professionale

## 1. Introduzione
* **Scopo del Documento**: Questo documento descrive l'architettura dell'applicazione web "Piattaforma di Orientamento Professionale", basata su un approccio a framework integrato.
* **Riferimento ai Business Needs**: La soluzione è progettata per soddisfare i requisiti delineati nel file `Business_Needs.md`.

## 2. Riepilogo dei Vincoli e Scelte Tecnologiche
* **Framework Full-Stack**: Next.js 14 con App Router
* **Database & ORM**: Firestore (interfacciato tramite Firebase Client SDK)
* **Piattaforma di Deployment**: Google Cloud Platform (Cloud Run)
* **Autenticazione**: Firebase Authentication
* **Requisiti Chiave**: 
  - Tempi di caricamento delle pagine minimi (Core Web Vitals ottimizzati).
  - Piattaforma affidabile e sicura per la selezione di professionisti.
* **Integrazioni Esterne**: Nessuna integrazione di terze parti richiesta per la versione iniziale.

## 3. Architettura dell'Applicazione
* **Paradigma Architetturale**: Applicazione web monolitica basata su framework full-stack con rendering ibrido.
* **Strategia di Rendering**: Si utilizzerà un approccio ibrido per massimizzare le performance e la SEO:
  - **Static Site Generation (SSG)**: Per le pagine di marketing, landing page, e contenuti informativi che non cambiano frequentemente.
  - **Server-Side Rendering (SSR)**: Per le pagine il cui contenuto è altamente dinamico e specifico per l'utente, come la dashboard dell'imprenditore.
  - **Incremental Static Regeneration (ISR)**: Per le liste di professionisti e i loro profili pubblici, per garantire che i dati siano recenti senza sacrificare la velocità di un sito statico.
* **Struttura delle Route**: Il routing seguirà la convenzione basata su file system di Next.js (App Router). Le route principali saranno:
  - `/`: Landing page e motore di ricerca principale.
  - `/professionisti`: Lista dei professionisti filtrabile.
  - `/professionisti/[id]`: Profilo pubblico di un singolo professionista.
  - `/auth/`: Pagine per login, registrazione e recupero password.
  - `/dashboard`: Area riservata per l'imprenditore per gestire i contatti.
* **Gestione della Logica di Business**: La logica di business e l'accesso ai dati saranno incapsulati nelle **API Routes** di Next.js e nelle **Route Handlers**. Questo approccio centralizza le interazioni con Firestore e mantiene i componenti frontend focalizzati sulla UI.
* **Struttura dei Componenti**: Si adotterà una filosofia a componenti atomici.
  - **Server Components**: Utilizzati per la maggior parte dei componenti che mostrano dati ma non richiedono interattività (es. visualizzazione di un profilo, liste). Questo riduce il JavaScript inviato al client.
  - **Client Components**: Utilizzati solo quando necessario per l'interattività dell'interfaccia utente (es. moduli di ricerca, pulsanti, menu a tendina).

## 4. Scelte Tecnologiche Dettagliate
| Categoria             | Tecnologia Scelta                    | Motivazione                                                     |
|:----------------------|:-------------------------------------|:----------------------------------------------------------------|
| Framework Principale  | Next.js (React)                      | Ecosistema maturo, flessibilità nel rendering (SSG/SSR/ISR), ottima SEO e performance. |
| UI e Stile            | Material UI (MUI)                    | Libreria di componenti completa e matura, design system coerente, buona accessibilità. |
| Database              | Firestore (Google Cloud)             | Database NoSQL serverless, si integra nativamente con Firebase Auth, scalabilità automatica. |
| Data Access Layer     | Firebase Client SDK                  | SDK ufficiale, garantisce un'integrazione semplice e diretta con Firestore e Firebase Auth. |
| Autenticazione        | Firebase Authentication              | Soluzione gestita, sicura, con supporto multi-provider (Google, email/password) e integrata con Firestore. |
| Deployment            | Google Cloud Run                     | Piattaforma serverless per container, si integra perfettamente con l'ecosistema Google Cloud, scaling automatico. |

## 5. Progettazione dei Dati
* **Modello Dati Logico (Firestore Collections)**:
  - **`users`**: Contiene i dati degli "Aspiranti Imprenditori".
    - `uid` (ID da Firebase Auth)
    - `email`
    - `displayName`
  - **`professionals`**: Contiene i profili dei professionisti.
    - `uid` (ID da Firebase Auth)
    - `displayName`
    - `category` (es. "Sviluppo Web", "Marketing Digitale")
    - `bio`
    - `portfolio` (sub-collection o array di oggetti)
    - `averageRating`
  - **`reviews`**: Valutazioni lasciate dagli imprenditori.
    - `professionalId`
    - `userId`
    - `rating` (1-5)
    - `comment`
    - `createdAt`
  - **`categories`**: Elenco delle categorie professionali per la ricerca (BN01).
    - `name`
    - `description`

## 6. Considerazioni Chiave
* **Performance**: L'uso strategico di SSG/ISR e dei Server Components minimizzerà il JavaScript lato client e i tempi di caricamento. Le immagini saranno ottimizzate tramite il componente `next/image`.
* **Sicurezza**: Le interazioni con Firestore saranno protette tramite le **Security Rules** di Firestore, garantendo che gli utenti possano leggere/scrivere solo i dati a cui sono autorizzati. Le chiavi segrete saranno gestite tramite variabili d'ambiente.
* **Scalabilità**: L'architettura è interamente basata su servizi serverless (Cloud Run, Firestore, Firebase Auth) che scalano automaticamente in base al traffico, garantendo la gestione della crescita degli utenti.

## 7. Fasi di Sviluppo Proposte (Roadmap)
* **Fase 1 (MVP)**: 
  - Setup del progetto Next.js con Firebase.
  - Implementazione dell'autenticazione per utenti e professionisti.
  - Creazione e visualizzazione dei profili dei professionisti (senza portfolio dettagliato).
  - Sistema di ricerca base per categorie.
  - Pagina statica di contatto.
* **Fase 2**:
  - Implementazione del sistema di recensioni.
  - Creazione di una dashboard utente per gestire i contatti.
  - Implementazione di un sistema di "primo contatto" (es. modulo di contatto sul profilo del professionista).
  - Aggiunta del portfolio dettagliato ai profili.
