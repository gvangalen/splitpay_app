# 🚀 PROJECT: SplitPay App (EUR + Bitcoin)

## 🎯 Doel
We bouwen een app zoals Tikkie, maar met twee betaalopties:

1. EUR betalingen via Stripe  
2. Bitcoin betalingen via River (Lightning)

De gebruiker kan bij elk betaalverzoek kiezen:
👉 betalen in euro of in bitcoin

---

## 🧠 Concept

De app is een hybride payment layer:

- User maakt verzoek (€10)
- Betaler kiest:
  - 💳 Betaal met euro (Stripe)
  - ⚡ Betaal met Bitcoin (Lightning via River)

De app handelt beide flows correct af en toont één status.

---

## ⚙️ Tech stack

### Frontend
- Next.js (App Router)
- TailwindCSS
- PWA (mobile-first)

### Backend
- FastAPI
- PostgreSQL
- Redis (later voor realtime)

---

## 🧱 Folder structuur

### Backend
plaintext id="hyb001" /backend   main.py    /api     auth_api.py     request_api.py     payment_api.py    /services     user_service.py     request_service.py     stripe_service.py     river_service.py     payment_orchestrator.py    /models     user.py     payment_request.py     transaction.py    /webhooks     stripe_webhook.py     river_webhook.py    /core     config.py     database.py 

---

## 🧠 Core services

### payment_orchestrator.py (BELANGRIJKSTE)
Deze service bepaalt:

- Welke payment flow wordt gebruikt
- Hoe status wordt verwerkt
- Eén uniforme response naar frontend

---

## 💳 Stripe flow (EUR)

### stripe_service.py

Functies:
- create_checkout_session(amount_eur)
- handle_webhook()

Flow:
1. User kiest EUR
2. Backend maakt Stripe checkout session
3. User betaalt via iDEAL / card
4. Stripe stuurt webhook
5. Status → paid

---

## ⚡ River flow (Bitcoin)

### river_service.py

Functies:
- create_lightning_invoice(amount_sats)
- handle_webhook()

Flow:
1. User kiest Bitcoin
2. Backend maakt Lightning invoice via River
3. QR wordt getoond
4. Betaling wordt gedaan
5. River webhook → paid

---

## 🔄 Unified payment flow

1. User maakt request (€10)
2. Backend slaat request op

3. Betaler opent link:
   - kiest:
     - EUR → Stripe
     - BTC → River

4. Backend start juiste flow

5. Webhook update:
   - payment_request.status = paid

---

## 🗄️ Database schema

### payment_requests
- id
- user_id
- amount_eur
- amount_sats (optioneel)
- description
- status (pending, paid, expired)
- created_at

---

### transactions
- id
- request_id
- payment_method (stripe, bitcoin)
- amount
- currency (EUR, BTC)
- external_id (stripe_session_id / lightning_invoice)
- status
- created_at

---

## 🎨 Frontend flow

### Create request
- bedrag invoeren (€)
- omschrijving

---

### Payment screen

Toon:

- Bedrag (€10)
- Keuze:
  - 💳 Pay with Euro
  - ⚡ Pay with Bitcoin

---

### Bitcoin flow
- toon QR
- toon sats bedrag

---

### Euro flow
- redirect naar Stripe checkout

---

### Status
- pending
- paid ✅

---

## 🚫 MVP beperkingen

- Geen eigen wallet
- Geen custody
- Geen conversie engine (nog niet)
- Stripe en River handelen betaling af

---

## 🚀 Eerste taken

1. Backend structuur opzetten
2. Database models maken
3. payment_request API bouwen
4. Stripe test integratie
5. River test integratie
6. Webhooks verwerken
7. Frontend keuze flow bouwen

---

## 🔥 Belangrijk

- Backend moet beide payment flows ondersteunen
- Eén request = meerdere betaalopties
- Status moet altijd synchroon zijn

---

## 🎯 MVP doel

Werkende app waarin:
- je een verzoek maakt (€10)
- iemand kiest:
  - euro of bitcoin
- betaling wordt verwerkt
- status wordt correct geüpdatet

---

## 💡 Focus

Simpel houden  
Geen extra features  
Werkende payment flow = succes