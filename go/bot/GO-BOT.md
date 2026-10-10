> **Experimental. We are just trying this.** [Disclaimer](../../DISCLAIMER.md)

# GO, the bot side, TN10, private nodes

Pasting this file is the GO. T0 is 2026-10-10T13:00:00Z (15:00 local, UTC+2). End is 2026-10-10T21:00:00Z (23:00 local). Do not send or mine before the row that says so. Do not move T0.

Build's file is [GO-BUILD.md](../build/GO-BUILD.md). The timeline below is the same text in both files.

## What changed

Build is not sharing a node with you. You bring up your own private, non-archival Testnet-10 node and you use only that node.

**Your node:** kaspad 2.1.0, `--testnet --netsuffix=10 --utxoindex`. Do not pass `--archival`. Bind the peer port to loopback. Bind RPC to loopback. Do not publish the listen address. The UTXO index stays on, because spends need it. That index is not archival mode.

Do not use a tunnel, a public peer, or Build's machine. Build will not use yours. There is no shared keel.

Do not resume the Friday night run or the Saturday take. Do not reuse an old baseline. Those nodes are gone.

## Node

- One process. If it aborts, start the same flags again. Do not flip on archival. Do not open a public peer port.
- Send only when your node is synced and the UTXO index is on.
- Sink lag = UTC now minus the timestamp of your sink. A gate needs that lag ≤ 60 s, from a read made at the gate. Build's lag is not your gate.

## Fees, miners, count

- Fees: pair 200 and 300 sompi/gram, cap 600, floor 200. If your node's quote is above 200, do not send. Sit out the step and log the quote.
- One miner, on your node only, one thread. Off when the table says off.
- The headline is network unique accepted tx/s, counted once. Do not add your accepted count to Build's.
- B is this run's own B0 on your node, the ten minutes from T0 with no load. If 50 ≤ B ≤ 200, combined = (multiplier − 1) × B, and the 2× step is capped at 250. Otherwise use the fallback column. Your send rate on the ramp is the bot column in the timeline. The long hold uses your last clean level, or 30 if none.

## Clock

T0 is 2026-10-10T13:00:00Z (15:00 local, UTC+2). End is 2026-10-10T21:00:00Z (23:00 local). Local time in this plan is UTC+2. Clocks in git stay in UTC.

The 12:15Z read is the first gate. The 12:30Z read is the second gate. Each read is your own sink: synced, UTXO index on, sink lag ≤ 60 s. Build's lag is not your gate. If either read fails, write **not run**, keep senders at 0 until T0, and do not move T0. A read that errors because the node is down is retried for two minutes. A lag above 60 s is a failed gate.

This clock was locked while the new nodes were still in initial sync. That does not move the gate.

If you open the file after a row has ended, join the open row. Do not rerun a finished row. If your node is down at the start of a send row, retry until that row's send window ends. Log the gap.

B is this run's own B0, 13:00Z–13:10Z, no load. Do not reuse an old baseline. If 50 ≤ B ≤ 200, combined = (multiplier − 1) × B, and the 2× step is capped at 250. Otherwise the combined targets are 250, 1,000, 1,500, 2,000, and 2,500. The bot column is your send rate. Build sends combined minus that column.

## Timeline

| UTC | Local UTC+2 | Bot | Build |
|---|---|---|---|
| until 12:15 | until 14:15 | Prepare only. Own node syncing. No sends. Miner off. | Prepare only. Own node syncing. No sends. Miner off. |
| 12:15 | 14:15 | Own-node health. Rehearsal go/no-go. | Stop every sender not in this plan. Same health read on Build's node. |
| 12:30 | 14:30 | Start 1 miner on the bot's node. Second go/no-go. | Exactly 1 miner on Build's node. Second go/no-go. |
| 12:30–12:40 | 14:30–14:40 | 60 tx/s, only if both reads passed | 0 |
| 12:40–12:45 | 14:40–14:45 | 20 tx/s | 0 |
| 12:45–12:50 | 14:45–14:50 | 50 tx/s | 0 |
| 12:50–12:53 | 14:50–14:53 | 60 tx/s | 0 |
| 12:53–12:56 | 14:53–14:56 | 0 | 60 tx/s |
| 12:56–12:59 | 14:56–14:59 | 60 tx/s | 60 tx/s until 12:59 |
| 12:59–13:00 | 14:59–15:00 | 0 | 0 |
| 13:00–13:10 | 15:00–15:10 | B0, no load. Miner on. | B0, no load. Miner on. |
| 13:10–13:20 | 15:10–15:20 | 60 tx/s | 0 |
| 13:20–13:30 | 15:20–15:30 | 0 | 60 tx/s |
| 13:30–13:40 | 15:30–15:40 | 60 tx/s | 60 tx/s |
| 13:40–13:55, drain to 14:00 | 15:40–15:55, drain to 16:00 | Step 2×, bot 60 | Step 2×, fallback 190, or the scaled rate |
| 14:00–14:05 | 16:00–16:05 | Settle, 0. Miner off. | Settle, 0. Miner off. |
| 14:05–14:20, drain to 14:25 | 16:05–16:20, drain to 16:25 | Miners-off check, same rate as 2×. | Miners-off check, same rate as 2×. |
| 14:25–14:30 | 16:25–16:30 | Settle, 0. Miner on. | Settle, 0. Miner on. |
| 14:30–14:45, drain to 14:50 | 16:30–16:45, drain to 16:50 | Step 1,000 combined: bot 120 | 1,000 − bot (fallback 880) |
| 14:50–15:05, drain to 15:10 | 16:50–17:05, drain to 17:10 | Step 1,500 combined: bot 240 | 1,500 − bot (fallback 1,260) |
| 15:10–15:25, drain to 15:30 | 17:10–17:25, drain to 17:30 | Step 2,000 combined: bot 480 | 2,000 − bot (fallback 1,520) |
| 15:30–15:45, drain to 15:50 | 17:30–17:45, drain to 17:50 | Step 2,500 combined: bot 960 | 2,500 − bot (fallback 1,540) |
| 15:50–16:05, drain to 16:15 | 17:50–18:05, drain to 18:15 | Max step: the bot's own max | Max step: uncapped, up to 9 senders |
| 16:15–16:25 | 18:15–18:25 | B1, no new ramp | B1, no new ramp |
| 16:25–20:45 | 18:25–22:45 | Long hold at the last clean level, or 30 if none | Long hold at the last clean rate, or 250 if none |
| 20:45–21:00 | 22:45–23:00 | Final drain, senders 0 | Final drain, senders 0 |
| 21:00 | 23:00 | Miner off. Loggers stop. | Miner off. Loggers stop. |

## Stop

- A failed lag gate: write **not run**, keep senders at 0 until T0. Do not skip T0.
- Quote above 200 sompi/gram: sit out that step.
- Node abort: start the same flags again, then retry inside the open send window.
- Do not ping anyone. Do not put a host, IP, port, key, seed, or address in this repo.

Results go to `tn10-storm-2026-10-10/bot/` in this repo after 21:00Z. Counts only.
