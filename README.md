# THE HUNDRED

A 10-seat paper-trading pit. Everyone starts with **$100**. Real market prices. Every order hits a public tape **before** it fills. After **90 days**, highest equity wins.

## Rules

- Up to **10 seats** per room (humans + AI bots).
- Starting cash: **$100**. Fractional shares allowed.
- You may buy **shares** or **listed equity options** (calls and puts).
- You may only sell what you own. No naked short options. No margin.
- Options use the real **100-share multiplier**. A $0.40 contract costs $40.
- **Announce first.** The tape posts your intended trade, waits 3 seconds, then fills at the live quote. If you cannot afford it by fill time, the order dies on the tape.
- Contest clock: **90 calendar days** from the moment the host starts the room.
- Winner = highest marked-to-market equity when the clock hits zero.

## Run it

```bash
cd paper-arena
python3 server.py
```

Needs Python 3.11+ and `pip install websockets`. No API keys.

Open [http://localhost:3000](http://localhost:3000). Create a room, share the 4-letter code, add bots if you want company, hit **Open the pit**.

## Data

- Stock last / change: Yahoo Finance chart endpoint (no key).
- Option chains: Nasdaq public option-chain API (no key).
- Quotes are cached ~15s (stocks) / ~45s (chains) so a full room does not hammer the sources.
- If a source blips, the last good print is used.

This is a **simulator**. Fills are marketable at mid/last. Spreads, halts, and corporate actions are simplified. Do not treat it as a broker.

## Bots

Three seats you can drop in from the lobby:

| Bot | Style |
| --- | --- |
| **MOMO-7** | Chases names that are green on the day |
| **FADE-9** | Buys dips, trims winners |
| **LOTTO-X** | Announces cheap out-of-the-money option lottery tickets |

Bots use the same announce-then-fill rule as humans.

## Deploy

Any Python 3.11+ host works (Fly, Railway, a VPS). Set `PORT` if needed. Game state is written to `data/games.json` so a restart does not wipe an in-progress contest.
