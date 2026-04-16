# Dotace Monitor

Next.js aplikace připravená pro GitHub + Vercel.

## Co umí

- 1× denně spustí scan přes Vercel Cron
- monitoruje nové dotační výzvy z OPST a IROP
- ukládá nalezené záznamy do Postgres databáze
- URL používá jako unikátní identifikátor
- po nalezení nových výzev pošle e-mailový přehled
- v UI ukáže zelený badge `+X právě přidáno`
- nově přidané řádky zvýrazní zeleně na 30 sekund nebo do dalšího refreshu
- umí filtrovat přes `KEYWORD_FILTER`

## Stack

- Next.js App Router
- Vercel Cron Jobs
- Vercel Postgres přes `@vercel/postgres`
- Resend pro odesílání e-mailů
- Cheerio pro scraping

## Lokální start

```bash
npm install
cp .env.example .env.local
npm run db:init
npm run dev
```

## Environment variables

- `POSTGRES_URL`
- `CRON_SECRET`
- `RESEND_API_KEY`
- `NOTIFY_EMAIL`
- `FROM_EMAIL`
- `KEYWORD_FILTER`
- `SCAN_OPST`
- `SCAN_IROP`

## Cron

V `vercel.json` je nastavený denní běh:

```json
{
  "crons": [
    {
      "path": "/api/cron/scan",
      "schedule": "0 7 * * *"
    }
  ]
}
```

Na Vercelu se cron zapisuje do `vercel.json` a endpoint je volaný HTTP GET requestem. Pokud nastavíš `CRON_SECRET`, Vercel pošle jeho hodnotu automaticky v `Authorization` headeru.

## Deploy na Vercel

1. Nahraj repo na GitHub.
2. Importuj repo do Vercelu.
3. Připoj Postgres databázi.
4. Nastav environment variables.
5. Deployni production.
6. Ověř cron a případně spusť ručně `/api/cron/scan`.

## Poznámka k Facebooku

Facebook monitoring v této verzi není aktivní. Architektura je připravená na doplnění dalšího provideru, ale stabilní veřejný scraping Facebooku bez API a loginu nebývá spolehlivý.
