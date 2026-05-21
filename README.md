# Mein Projektbüro - Angebote & Rechnungen

![Mein Projektbüro - Dashboard](images/projektbeuro.png)

Eine moderne Web-Anwendung zur Verwaltung von Angeboten, Rechnungen, Kunden und Projekten. Entwickelt mit Next.js 16, React 19, TypeScript und Tailwind CSS.

## Was es ist

Ein Backoffice-Tool für Solo-Operatoren und kleine Web-Agenturen, das den kompletten Auftragsweg abdeckt — von der ersten Angebotsversion über Rechnungsstellung, Mahnwesen und Online-Zahlung bis zu DATEV- und XRechnung-Export. Spezialisiert auf den Web-Agentur-Workflow: Angebote werden nicht als Pauschalpreis gerechnet, sondern aus Paketen und einzelnen Komponenten (Header, Seiten, Formulare, CMS, SEO, Wartung) mit Min/Max-Komplexität kalkuliert. Generische Buchhaltungs-Tools wie Sevdesk oder Lexware decken das nicht ab.

## Status

- **Privates Projekt**, in aktiver produktiver Nutzung.
- **Test-Abdeckung:** 2.569 Unit-Tests (Vitest), 650+ E2E-Tests (Playwright, 21 Spec-Dateien).
- Funktional vollständig für Angebot → Rechnung → Zahlung → Buchhaltungs-Export.

## Architektur (Kurz)

Next.js 16 App Router als Monolith mit klarer Schichtentrennung:

- **`src/app/api/`** — REST-Endpoints pro Domäne (Angebote, Rechnungen, Kunden, Projekte, Zeiterfassung, Portal, Webhooks).
- **`src/lib/repositories/`** — Service-Layer als einziger Zugriff auf die Datenbank, hält API-Routes dünn.
- **`src/lib/payment/`** — Provider-Abstraktion für Stripe und PayPal (Webhook-Verarbeitung idempotent über externe Event-IDs).
- **`src/lib/datev/`, `src/lib/mahnung/`, `src/lib/webhook/`** — fachliche Module mit eigener Logik, isoliert testbar.
- **Prisma + PostgreSQL** als Datenschicht, Migrations versioniert.
- **Kundenportal** als separater öffentlicher Route-Tree (`src/app/portal/`) mit Token-basiertem Zugang, kein Login.
- **PDF/PPTX/XRechnung-Export** als reine Server-Funktionen (jsPDF, pdf-lib, PptxGenJS, ZUGFeRD 2.3 / PDF/A-3).

## Features

### Angebotserstellung
- **Paket-Auswahl**: Vordefinierte Pakete (Starter, Professionell, Business, Enterprise) als Ausgangspunkt
- **Komponenten-Baukasten**: Modulare Auswahl von Website-Komponenten (Header, Footer, Seiten, Formulare, etc.)
- **Komplexitätsstufen**: Min/Max-Kalkulation pro Komponente
- **CMS-Option**: Optionaler CMS-Aufschlag pro Komponente
- **SEO-Upgrades**: Basis, Lokal, Professional SEO-Pakete
- **Barrierefreiheit**: WCAG AA/AAA Konformität als Zusatzleistung
- **Wartungspakete**: Monatliche Wartungsoptionen
- **Konfigurierbare Angebotsphasen**: Mehrere Phasen-Templates (z.B. Websiteentwicklung, Beratung) mit prozentualer Zeitverteilung, Drag & Drop Reihenfolge

### Rechnungswesen
- **Rechnungen aus Angeboten**: Ein-Klick-Konvertierung von angenommenen Angeboten
- **Flexible Rechnungstypen**: Standard, Abschlag, Schlussrechnung
- **Teilzahlungen**: Unterstützung für Abschlagszahlungen und Ratenzahlung
- **Mahnwesen**: Automatische Mahnstufen mit konfigurierbaren Texten und Fristen
- **PDF-Export**: Professionelle Rechnungs-PDFs mit Briefkopf
- **DATEV-Export**: CSV- und ASCII-Export für Steuerberater
- **XRechnung**: E-Rechnung im XRechnung 3.0.2 Format (ZUGFeRD 2.3, PDF/A-3)

### Online-Zahlung
- **Stripe-Integration**: Kreditkarte und SEPA-Lastschrift
- **PayPal-Integration**: PayPal Checkout
- **Kundenportal**: Kunden können Rechnungen direkt online bezahlen
- **Automatische Verbuchung**: Zahlungen werden via Webhook erfasst
- **Idempotente Verarbeitung**: Schutz vor doppelten Buchungen

### Kundenportal
- **Sichere Zugangslinks**: Token-basierter Zugang ohne Login
- **Rechnungsansicht**: Kunden können ihre Rechnungen einsehen
- **Online-Zahlung**: Direkte Bezahlung per Stripe oder PayPal
- **PDF-Download**: Rechnungen als PDF herunterladen

### Kundenverwaltung
- **Kundenstamm**: Verwaltung von Firmendaten und Ansprechpartnern
- **Adressverwaltung**: Rechnungs- und Lieferadressen
- **Kundenhistorie**: Übersicht aller Angebote und Rechnungen pro Kunde
- **Schnellanlage per Modal**: Neuen Kunden direkt aus der Kunden-Liste erstellen

### Projektverwaltung
- **Projektübersicht**: Alle Projekte auf einen Blick
- **Projektstatus**: Tracking von Projektfortschritt
- **Zeiterfassung**: Erfassung von Arbeitszeiten pro Projekt mit Timer
- **Schnellanlage per Modal**: Neues Projekt direkt aus der Projekt-Liste erstellen
- **Statistik**: Angebots- und Rechnungsauswertungen

### Kalkulation
- **Stundensatz-basiert**: Konfigurierbare interne Stundensätze
- **Puffer-Option**: Interner Aufschlag (nicht sichtbar für Kunden)
- **Flexible Zahlungsbedingungen**: Vorkasse, 50/50, Ratenzahlung mit Skonto/Aufschlag
- **Automatische USt.-Berechnung**: Netto/Brutto-Preise

### Export-Optionen
- **PDF-Export**: Professionelle PDFs für Angebote und Rechnungen
- **PowerPoint-Export**: Präsentation für Kundengespräche
- **DATEV-Export**: CSV- und ASCII-Format für Buchhaltung
- **XRechnung**: E-Rechnung für öffentliche Auftraggeber
- **Sevdesk-Integration**: Direkter Export nach Sevdesk

### Integrationen
- **Sevdesk**: Buchhaltungssoftware-Anbindung
- **Stripe**: Online-Zahlungen (Karte, SEPA)
- **PayPal**: Online-Zahlungen
- **Webhooks**: Benachrichtigungen bei Statusänderungen

## Tech-Stack

- **Framework**: Next.js 16 (App Router)
- **Sprache**: TypeScript
- **Styling**: Tailwind CSS + shadcn/ui
- **Datenbank**: PostgreSQL mit Prisma ORM
- **PDF-Generierung**: jsPDF + pdf-lib
- **PowerPoint**: PptxGenJS
- **Zahlungen**: Stripe, PayPal SDK
- **Testing**: Vitest (Unit), Playwright (E2E)
- **Icons**: Lucide React

## Installation

```bash
# Dependencies installieren
npm install

# Umgebungsvariablen konfigurieren
cp .env.example .env

# Datenbank initialisieren
npx prisma migrate dev

# Entwicklungsserver starten
npm run dev
```

Die App läuft unter [http://localhost:3000](http://localhost:3000).

## Umgebungsvariablen

```env
# Datenbank
DATABASE_URL="postgresql://user:password@localhost:5432/angebote"

# App URL (für Kundenportal-Links)
NEXT_PUBLIC_APP_URL="https://deine-app.de"

# Stripe (Online-Zahlung)
STRIPE_SECRET_KEY=sk_live_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...

# PayPal (Online-Zahlung)
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
NEXT_PUBLIC_PAYPAL_CLIENT_ID=...
PAYPAL_WEBHOOK_ID=...

# Sevdesk (optional)
SEVDESK_API_TOKEN=...

# Webhooks (optional)
WEBHOOK_URL=...
WEBHOOK_SECRET=...
```

## Konfiguration

### Einstellungen (in der App)
- **Allgemein**: Stundensatz, USt.-Satz, Puffer
- **Firmendaten**: Name, Adresse, Kontakt, Bankverbindung, Steuernummern
- **PDF-Vorlage**: Template, Logo, Sprache
- **Angebotsphasen**: Konfigurierbare Phasen-Templates mit Drag & Drop
- **Nummernkreise**: Angebots- und Rechnungsnummern-Format
- **Mahnwesen**: Mahnstufen, Fristen, Texte
- **Integrationen**: Sevdesk, Stripe, PayPal, Webhooks

### Stripe-Integration

1. Stripe-Account erstellen unter [stripe.com](https://stripe.com)
2. API-Keys aus dem Dashboard kopieren
3. Webhook-Endpoint einrichten: `https://deine-app.de/api/webhooks/stripe`
4. Events abonnieren: `checkout.session.completed`
5. Keys in `.env` eintragen

### PayPal-Integration

1. PayPal Developer Account unter [developer.paypal.com](https://developer.paypal.com)
2. App erstellen und Client ID/Secret kopieren
3. Webhook einrichten: `https://deine-app.de/api/webhooks/paypal`
4. Events: `CHECKOUT.ORDER.APPROVED`, `PAYMENT.CAPTURE.COMPLETED`
5. Keys in `.env` eintragen

### Sevdesk-Integration

1. Sevdesk API-Token in den Einstellungen hinterlegen
2. Token unter: Sevdesk → Einstellungen → Benutzerverwaltung → API-Token
3. Bei Export werden Kunde und Dokument automatisch angelegt

## Projektstruktur

```
src/
├── app/                    # Next.js App Router
│   ├── api/               # API Routes
│   │   ├── angebote/      # Angebote CRUD + PDF + Duplizieren
│   │   ├── rechnungen/    # Rechnungen CRUD + PDF + ZUGFeRD + Mahnung
│   │   ├── kunden/        # Kunden CRUD
│   │   ├── projekte/      # Projekte CRUD + Aufgaben + Abrechnung
│   │   ├── zeiterfassung/ # Timer + Zeiteinträge
│   │   ├── einstellungen/ # Stundensatz, Angebotsphasen, Nummernkreise
│   │   ├── portal/        # Kundenportal API + Online-Zahlung
│   │   ├── webhooks/      # Webhook-Verwaltung + Stripe/PayPal Handler
│   │   └── ...            # Weitere (Statistik, Suche, Banking, etc.)
│   ├── portal/            # Kundenportal (öffentlich)
│   ├── rechnungen/        # Rechnungsverwaltung
│   ├── kunden/            # Kundenverwaltung
│   ├── projekte/          # Projektverwaltung
│   ├── zeiterfassung/     # Zeiterfassung (Timer, Wochenübersicht)
│   ├── statistik/         # Angebots- und Rechnungsauswertung
│   └── einstellungen/     # App-Einstellungen
├── components/
│   ├── ui/                # Custom shadcn-style Komponenten
│   ├── builder/           # Angebots-Builder (Pakete, Komponenten, Zusammenfassung)
│   ├── einstellungen/     # Einstellungen-Komponenten (Phasen-Templates, etc.)
│   ├── kunden/            # Kunden-Komponenten (Formular, Modal)
│   ├── projekte/          # Projekt-Komponenten (Formular, Modal)
│   ├── portal/            # Portal-Komponenten
│   └── ...
├── lib/
│   ├── db/               # Prisma Client
│   ├── repositories/     # Service-Layer (Einstellungen, etc.)
│   ├── payment/          # Stripe/PayPal Clients
│   ├── portal/           # Portal-Service
│   ├── mahnung/          # Mahnwesen
│   ├── datev/            # DATEV-Export
│   ├── webhook/          # Webhook-Service
│   └── ...
└── types/                # TypeScript-Typen
```

## Scripts

```bash
npm run dev       # Entwicklungsserver
npm run build     # Produktions-Build
npm run start     # Produktionsserver
npm run lint      # ESLint
npm run test      # Unit-Tests (Vitest)
npm run test:e2e  # E2E-Tests (Playwright)
```

## Testing

```bash
# Alle Unit-Tests
npm test

# Unit-Tests im Watch-Mode
npm run test:watch

# E2E-Tests
npm run test:e2e

# E2E-Tests mit UI
npm run test:e2e:ui
```

**Test-Abdeckung:**
- 2569 Unit-Tests
- 650+ E2E-Tests (21 Spec-Dateien)

## Webhooks

Die App unterstützt ausgehende Webhooks für Statusänderungen:

| Event | Beschreibung |
|-------|--------------|
| `angebot.erstellt` | Neues Angebot erstellt |
| `angebot.versendet` | Angebot wurde versendet |
| `angebot.angenommen` | Angebot wurde angenommen |
| `angebot.abgelehnt` | Angebot wurde abgelehnt |
| `rechnung.erstellt` | Neue Rechnung erstellt |
| `rechnung.versendet` | Rechnung wurde versendet |
| `rechnung.bezahlt` | Rechnung wurde bezahlt |
| `mahnung.versendet` | Mahnung wurde versendet |

## Roadmap

Siehe [ROADMAP.md](./ROADMAP.md) für:
- Implementierte Features
- Geplante Erweiterungen
- Technische Verbesserungen

### Nächste geplante Features
- [ ] **Mobile Companion App** (iOS & Android) - Zeiterfassung, Push-Notifications, Quick Actions
- [ ] Online-Signatur (digitale Unterschrift)
- [ ] CRM-Anbindung (HubSpot, Pipedrive)
- [ ] KI-Features (Texterstellung, Preisempfehlungen)

## Lizenz

Privates Projekt.
