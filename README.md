> **Experimental. We are just trying this.** [Disclaimer](DISCLAIMER.md)
>
> **Part of [tn-master](https://github.com/STP-KAS/tn-master)**, the map of every TN10 storm. Renamed on 11 Oct 2026 from `tn10-private`; old links redirect here.
>
> ← previous storm: [tn10-storm-2026-10-10](https://github.com/STP-KAS/tn10-storm-2026-10-10) | next storm: [tn10-storm-2026-10-10-public-nodes](https://github.com/STP-KAS/tn10-storm-2026-10-10-public-nodes) →

# tn10-storm-2026-10-10-plan

Each side runs its own private, non-archival Testnet-10 node. There is no shared desk node and no tunnel between the sides.

The old pair is retired. Do not resume `tn10-storm-2026-10-09`. Do not reuse its baseline.

| Side | Paste this | Node |
|---|---|---|
| The bot | [go/bot/GO-BOT.md](go/bot/GO-BOT.md) | the bot's own private node |
| Grok Build | [go/build/GO-BUILD.md](go/build/GO-BUILD.md) | Build's own private node |

T0 is 2026-10-10T13:00:00Z (15:00 local, UTC+2). End is 2026-10-10T21:00:00Z (23:00 local). The first gate is 12:15Z (14:15 local). Until that gate: no sends, miner off.

Same 8-hour shape as the last plan: rehearsal, baseline, one side then the other, both, the ramp, miners-off, max, a long hold, a drain. Fees stay 200 and 300 sompi/gram. The headline is network unique accepted tx/s, counted once. Do not reuse a baseline of 66 or 67.

Counts only. No txid, key, seed, host, IP, port or address.
