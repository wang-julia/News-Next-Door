# News Next Door

**News Next Door tells you, in plain words or audio, what’s happening on your block in the language you want.**

New York City never stops talking. It is one of the most linguistically diverse places on earth: 3.1 million New Yorkers were born outside the United States, about 37.5% of the city, and nearly half of all residents speak a language other than English at home. But while the city speaks hundreds of languages, much of the information that shapes its neighborhoods speaks just one.

**[Watch the 2-minute demo](https://www.youtube.com/watch?v=xCKf7bbWMu0&feature=youtu.be)** · **[Try the live app](https://news-next-door-production.up.railway.app/)** · **[Devpost](https://devpost.com/software/news-next-door)** ·

---

## Inspiration

Every day, government decisions, local news events, and neighborhood developments directly affect immigrant communities. A rezoning in Long Island City, a bus priority corridor in Jackson Heights, a playground redesign in Greenpoint — each one goes through a community board, a public review period, and a vote.

Yet almost all of it is written in dense English rather than plain language: 40-page PDFs full of ULURP numbers and CEQR identifiers, posted on websites built for people who already know how the process works. That makes it difficult for immigrant voters to understand what is happening in their own communities, while they can still say something about it.

## What it does

News Next Door works more like a neighborhood newspaper than a civic dashboard: big text, short sentences, one idea per line, and information in the language you actually think in.

Residents can:

- **Translate** local news and government updates into their preferred language
- **Simplify** complicated filings into clear, easy-to-understand language
- **Listen** to any article through text to speech
- **Follow** issues and neighborhoods and get personalized updates, like a newsletter
- **Talk to an agent in iMessage** to subscribe to news, get summaries, and translate

Whether your block is in Astoria, Ridgewood, Williamsburg, or Chelsea, the goal is the same: make local information easier to understand, easier to access, and harder to miss.

### Two feeds, one front page

| Feed | Source | What you get |
|---|---|---|
| **Zone news** | NYC Planning's Zoning Application Portal | Live zoning applications for your community district, mapped onto official Community District (BoroCD) boundaries |
| **City news** | The New York Times city and regional coverage | Citywide stories from all five boroughs, each rewritten as a 30-second read |

### Languages

English, 中文, Español, Français, 日本語, हिन्दी, العربية, and Русский — each labeled in its own script, with full right-to-left layout for Arabic. You choose every language you read, and the switcher flips between just those.

## How we built it

| Layer | What we used |
|---|---|
| **Web app** | React 19, Vite 8, Leaflet, hand-written CSS |
| **Backend** | Node with Hono, SQLite for persistent storage, audio cached on disk |
| **Authentication** | Better Auth (email and password, Google-ready) |
| **Plain-language AI** | Grok via xAI — summarizes articles, simplifies complex language, translates, and answers questions over iMessage |
| **Text to speech** | ElevenLabs — narrates articles in English and dubs them into Chinese |
| **iMessage** | Photon / Spectrum — sends personalized updates, translations, and answers directly in iMessage |
| **Civic data** | NYC Planning ZAP applications, BoroCD district boundaries, NYT RSS and Article Search |

**Zone news** queries ZAP for the community district on your account and draws its real boundary from the city's own Community Districts layer, not a hand-drawn outline. Each district is keyed by its official codes (ZAP `Q02`, BoroCD `402`), so covering more of the city is a data row rather than new code.

**Follow by text, no app required.** Tap Follow and you get a short code plus a QR code. Text the code to our iMessage line and the page flips to "Following" only after the backend has actually processed your message. You get a confirmation, a reminder 24 hours before the hearing, and `STOP` cancels everything queued.

**Extraction with receipts.** When Grok reads an official PDF, every field it fills in must come with a verbatim quote and the page it came from. The server re-checks each quote against the extracted page text, and a publish is blocked if any excerpt does not appear where the model said it does. Anything we cannot source stays empty and renders as "Not listed in source" instead of a confident guess.

---

## Run it

```bash
npm install
cp .env.example .env    # every key is optional
npm run dev             # web on http://localhost:5190, API on :8790
