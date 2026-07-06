# InspectorBOTConnor

A real-time security event monitoring pipeline, built on Counter-Strike 2.

InspectorBOTConnor is a live overlay for CS2, but under the hood it is a SIEM in miniature. It ingests a continuous stream of telemetry from a log source, parses and normalizes the events, tracks state changes, and surfaces the ones that matter on a real-time dashboard. The log source just happens to be a video game.

## Why this exists

I work in cybersecurity, and I play a lot of Counter-Strike. At some point I realized that CS2's Game State Integration is structurally identical to the telemetry pipelines I work with professionally:

| SIEM concept | InspectorBOTConnor equivalent |
|---|---|
| Log source | CS2 game client |
| Event forwarding | GSI HTTP POST payloads (JSON) |
| Collector / listener | Local HTTP endpoint receiving the stream |
| Parsing and normalization | JSON payload parsing into structured event records |
| Correlation and state tracking | Round phase, player state, bomb status across payloads |
| Alerting | Overlay highlights for critical state changes |
| Dashboard | The real-time overlay itself |

So I built the whole thing the way I would build a monitoring pipeline: ingest, parse, correlate, alert, display.

## How it works

1. CS2 is configured with a Game State Integration config file that tells the client where to send telemetry.
2. The game POSTs JSON payloads to a local listener endpoint on every meaningful state change.
3. The listener ingests each payload, parses it, and diffs it against tracked state.
4. State changes that matter (round phase transitions, health thresholds, bomb events, score updates) are promoted to events.
5. The overlay renders those events in real time on top of the game.

## Setup

1. Clone this repo.
2. Copy the provided GSI config into your CS2 cfg directory:
   `steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg/`
3. Start the listener.
4. Launch CS2. The game will begin streaming telemetry to the listener automatically.

Detailed setup steps and configuration options are documented in the config file comments.

## What I learned building this

- Designing an ingestion endpoint that stays responsive under a high-frequency event stream
- Parsing and normalizing semi-structured JSON telemetry into consistent event records
- Stateful correlation: deciding what counts as a meaningful change versus noise
- Real-time presentation of security-relevant events, the same problem every SOC dashboard solves

## Stack

- CS2 Game State Integration (telemetry source)
- Local HTTP listener (event collector)
- Real-time overlay (dashboard)

## License

MIT
