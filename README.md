# Shox Service CRM — Deploy Qo'llanma

## Loyiha tuzilmasi
```
shox-service/
├── index.html          ← Entry point
├── src/
│   ├── main.jsx        ← React root
│   └── App.jsx         ← Asosiy ilova
├── worker/
│   └── index.js        ← Cloudflare Worker (API proxy)
├── public/
│   └── icon.svg
├── package.json
├── vite.config.js
└── wrangler.toml
```

---

## QADAM 1 — GitHub repo yaratish

1. https://github.com ga kiring
2. "New repository" → nom: `shox-service`
3. Public yoki Private (istalgan)
4. "Create repository"

Keyin terminal da:
```bash
cd shox-service
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/SIZNING_USERNAME/shox-service.git
git push -u origin main
```

---

## QADAM 2 — Cloudflare Worker deploy

1. https://cloudflare.com ga kiring (bepul akkaunt)
2. Dashboard → Workers & Pages → Create → "Hello World" Worker
3. Worker nomini `shox-service-api` qiling
4. Kodni `worker/index.js` dan nusxalab yapishtiringm "Deploy"

**API Key qo'shish:**
- Worker sahifasi → Settings → Variables and Secrets
- "+ Add variable": `ANTHROPIC_API_KEY` = sizning kalitingiz
- "Save"

Worker URL ni eslab qoling:
`https://shox-service-api.YOUR_SUBDOMAIN.workers.dev`

---

## QADAM 3 — Cloudflare Pages deploy

1. Cloudflare Dashboard → Workers & Pages → Create → Pages
2. "Connect to Git" → GitHub → `shox-service` repo tanlang
3. Build sozlamalari:
   - **Framework preset:** Vite
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
4. Environment variables qo'shing:
   - `VITE_PROXY_URL` = `https://shox-service-api.YOUR_SUBDOMAIN.workers.dev`
5. "Save and Deploy"

Bir necha daqiqada saytingiz tayyor!
URL: `https://shox-service.pages.dev`

---

## QADAM 4 — Lokal test (ixtiyoriy)

```bash
# Dependencylarni o'rnatish
npm install

# .env.local fayl yarating
echo "VITE_PROXY_URL=https://shox-service-api.YOUR_SUBDOMAIN.workers.dev" > .env.local

# Dev server ishga tushiring
npm run dev
```

---

## Keyingi yangilanishlar

Har yangi o'zgarishda faqat:
```bash
git add .
git commit -m "O'zgarish tavsifi"
git push
```
Cloudflare Pages avtomatik build va deploy qiladi (1-2 daqiqa).

---

## Muhim eslatmalar

- `ANTHROPIC_API_KEY` ni hech qachon GitHub ga push qilmang
- `.env.local` `.gitignore` da allaqachon bor
- Worker bepul: kuniga 100,000 so'rov
- Pages bepul: cheksiz deploy
