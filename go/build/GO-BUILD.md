> **Experimental. We are just trying this.** [Disclaimer](../../DISCLAIMER.md)

# GO, the Build side, TN10, private nodes

Pasting this file is the GO. Do nothing that sends or mines until a later line sets T0. This file does not set T0.

The bot's file is [GO-BOT.md](../bot/GO-BOT.md). The timeline below is the same text in both files.

## What changed

The two-node desk is retired. Build deletes nothing on the bot's machine. Build runs one new node of its own.

**Claim:** that node is private and not archival. Peer listen is loopback only. RPC is loopback only. `--archival` is off. The UTXO index is on, because spends need it. A UTXO index is not an archival node.

The bot does the same on its own machine. Build does not send to the bot's node. The bot does not send to Build's node. There is no tunnel and no shared mempool to read.

Do not resume the Friday night run or the Saturday take. Do not reuse a baseline of 66 or 67. Those nodes are gone.

## Node

- kaspad 2.1.0, `--testnet --netsuffix=10 --utxoindex`.
- No `--archival`.
- Peer port bound to loopback. RPC bound to loopback. Do not publish the listen address.
- One process. If it aborts, start the same flags again. The 10 Oct aborts were `0xc0000409` at one offset inside kaspad. Starting again on the same flags is the workaround. Do not flip on archival to "fix" it. Do not open a public peer port.
- Send only when the node is synced and the UTXO index is on.
- Sink lag = UTC now minus the timestamp of this node's sink. A gate needs that lag ≤ 60 s, from a read made at the gate.

## Fees, miners, count

- Fees: pair 200 and 300 sompi/gram, cap 600, floor 200. Half the lanes at 200, half at 300, frozen for the step from this node's quote. If the quote is above 200, do not send. Sit out the step and log the quote.
- One miner, on this node only, one thread. Off when the table says off. No miner on the bot's node. No second miner.
- Build's max step uses at most 9 senders.
- The headline is network unique accepted tx/s, counted once. Do not add Build's node to the bot's node.
- B is this run's own B0, the ten minutes from T0 with no load on this node. If 50 ≤ B ≤ 200, combined = (multiplier − 1) × B, and the 2× step is capped at 250. Otherwise use the fallback column in the table (2× 250, then 1,000, 1,500, 2,000, 2,500). A baseline from a deleted node is not B.

## Clock

T0 is not in this file. When a later line sets T0, the rows below are offsets from that T0. End is T0 + 8 hours. If you open the file after a row has ended, join the open row. Do not rerun a finished row.

If this node is down at the start of a send row, retry until that row's send window ends. Log the gap. Do not move T0 because of one gap.

## Timeline

| From T0 | Bot | Build |
|---|---|---|
| before T−45 min | Prepare only. Own node syncing. No sends. Miner off. | Prepare only. Own node syncing. No sends. Miner off. |
| T−45 min | Own-node health. Rehearsal go/no-go: fresh sink lag ≤ 60 s. | Stop every sender not in this plan. Same health read on Build's node. |
| T−30 min | Start 1 miner on the bot's node. Second go/no-go read. | Exactly 1 miner on Build's node. Second go/no-go read. |
| T−30 min to T0 | Rehearsal, only if both reads passed: 60 tx/s for 10 min, light lane 20 then 50, 60, a short 0, then 60. | Rehearsal, only if both reads passed: 0 until T−7 min, then 60 tx/s until T−1 min. |
| T0 to T+10 | B0, no load. Miner on. | B0, no load. Miner on. |
| T+10 to T+20 | 60 tx/s | 0 |
| T+20 to T+30 | 0 | 60 tx/s |
| T+30 to T+40 | 60 tx/s | 60 tx/s |
| T+40 to T+55, drain to T+60 | Step 2×, bot level (60) | Step 2×, fallback 190, or the scaled rate |
| T+60 to T+65 | Settle, 0. Miner off. | Settle, 0. Miner off. |
| T+65 to T+80, drain to T+85 | Miners-off check, same rate as 2×. | Miners-off check, same rate as 2×. |
| T+85 to T+90 | Settle, 0. Miner on. | Settle, 0. Miner on. |
| T+90 to T+105, drain to T+110 | Step 1,000 combined: bot level | 1,000 − bot (fallback 880) |
| T+110 to T+125, drain to T+130 | Step 1,500 combined: bot level | 1,500 − bot (fallback 1,260) |
| T+130 to T+145, drain to T+150 | Step 2,000 combined: bot level | 2,000 − bot (fallback 1,520) |
| T+150 to T+165, drain to T+170 | Step 2,500 combined: bot level | 2,500 − bot (fallback 1,540) |
| T+170 to T+185, drain to T+195 | Max step: bot level | Max step: uncapped, up to 9 senders |
| T+195 to T+205 | B1, no new ramp | B1, no new ramp |
| T+205 to T+7 h 45 min | Long hold at the last clean level, or 30 if none | Long hold at the last clean rate, or 250 if none |
| T+7 h 45 min to T+8 h | Final drain, senders 0 | Final drain, senders 0 |
| T+8 h | Miner off. Loggers stop. | Miner off. Loggers stop. |

## Stop

- A failed lag gate: write **not run**, keep senders at 0 until T0. Do not skip T0.
- Quote above 200 sompi/gram: sit out that step.
- Node abort: start the same flags again, then retry inside the open send window.
- Do not ping anyone. Do not put a host, IP, port, key, seed, or address in this repo.

Results, once T0 exists, go to `tn10-storm-<date>/build/` in this repo. Counts only.
