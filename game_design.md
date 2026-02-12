# River & Thread — History Simulation Game Design

## 1) Core Pitch
**River & Thread** is a real-time historical factory simulation where the player runs a shirt factory anchored to a river town evolving from early industrialization into rail capitalism. The player learns by watching movement:
- workers flowing through streets,
- power flowing through machinery,
- goods flowing through roads, canals, rail, and markets,
- waste flowing downstream,
- and social pressure flowing into unrest and politics.

The player does not directly command citizens. They shape incentives (wages, housing, prices, policy requests), then observe the world respond.

---

## 2) Experience Pillars (What Must Always Be True)
1. **Readable Motion Over Menus**  
   Every key system has a visual behavior: queues, drift, clustering, congestion, degradation, recovery.
2. **Structural Change Over Flat Difficulty**  
   Problems transform over time: labor scarcity → labor surplus, power constraints → logistics constraints, local roads → integrated rail network.
3. **Partial Control, Real Consequences**  
   Player controls the factory and requests policy; the state, workers, market, and environment remain partly autonomous.
4. **Moral Drift Through Systems**  
   The player is not forced to be “good” or “evil”; the simulation can make humane choices feel optional unless politics/social pressure changes incentives.

---

## 3) Camera, Map, and Visual Language
### Map Layout
- Side-on/isometric hybrid view of a living town.
- River runs left → right across the map.
- Player factory sits on one bank with room for lateral expansion.
- Town district includes houses, markets, roads, civic buildings, competitor factory zones.
- Off-screen edges represent external buyers, rural migrants, and upstream/downstream effects.

### Visual Rules
- No important variable should exist only in a hidden number if it can be visualized.
- Use iconography sparingly as overlays for debugging (toggle mode), not primary play.
- Time-of-day and weather modify throughput and movement speed subtly.

---

## 4) Core Loop (Minute-to-Minute)
1. Observe movement bottlenecks (empty seats, idle workers, warehouse pileups, transport queues).
2. Adjust one or two levers (wages, shirt price, shift length, housing, expansion, transport contracts).
3. Watch adaptation in real time (worker route choices, cart arrival frequency, seat occupancy, cash flow).
4. Invest in physical changes (new wing, canal cut, station siding, storage yard).
5. React to external shocks and political windows.

---

## 5) System Design by Era

## Era I — Early Industrial Lift-Off
### Intended Feel
Fragile growth: every worker and every unit of water power matters.

### Active Systems
- **Power:** water wheel efficiency affected by river level and maintenance.
- **Labor Scarcity:** low town population; workers choose employers by effective utility.
- **Production Seats:** finite benches/machines; empty seats are visible opportunity cost.
- **Market Access by Cart:** buyers arrive as carts based on price competitiveness and reliability.

### Player Levers
- wage per shift,
- hiring priority (open seats),
- shirt price,
- basic housing construction near factory,
- wheel maintenance schedule.

### Visual Signals
- Worker “decision wobble” at intersections when employers compete.
- Factory gate inflow pulses after wage increase.
- Empty benches highlighted by idle machine animation.
- Cart frequency falls/rises after price changes.

### Win Condition for Era Transition
Sustain profitability and occupancy long enough to unlock major capital projects (canal/expansion).

---

## Era II — Expansion Without Relocation
### Intended Feel
Scale arrives, then hidden dependencies surface.

### Active Systems
- **Canal Engineering:** build branch canal with locks/embankments; increases bulk throughput.
- **Factory Footprint Growth:** add wings and mechanized lines.
- **Transport Congestion:** barge/carts share loading priorities; queueing dynamics become central.
- **Demographic Shock:** event chain introduces large labor influx (migration/proletarianization).

### Systemic Shift
- Wages become less elastic as labor surplus rises.
- Energy fades as primary concern; logistics becomes binding constraint.

### Visual Signals
- Barges stacked in canal queue.
- Warehouse shirt stacks physically grow and spill to yard markers.
- Street crowd density spikes after migration event.
- Wage changes produce little visible movement shift post-shock.

### Key Player Choices
- Add loading docks vs warehouse expansion.
- Prioritize outbound contracts vs local market.
- Accept lower margins for flow stability.

---

## Era III — Rail Integration and Externalization
### Intended Feel
Scale and speed, then social/environmental backlash.

### Active Systems
- **Rail Hub:** high-capacity transport collapses previous shipping bottlenecks.
- **Pollution Externality:** cheap dumping darkens river, kills fish, harms downstream health/productivity.
- **Urban Density & Unrest:** crowding + low conditions create protests/strikes.
- **Competitor Opacity:** rivals remain black boxes inferred by observed flows.

### Visual Signals
- River color and fauna loss as progressive state change.
- Downstream building wear and maintenance decline.
- Banners, marches, gate blockades, police dispersal behavior.
- Sudden rerouting of carts/workers toward rivals when player reliability drops.

### Late-Game Tension
Player can produce and sell at industrial scale but faces legitimacy risk: stoppages, sabotage, policy backlash, public health costs.

---

## 6) Agent Behaviors (Simulation Actors)

## Workers
Each worker has:
- home anchor (or transient state),
- wage expectation,
- fatigue/health,
- risk tolerance,
- social linkage to nearby workers (opinion contagion).

### Worker Decision Heuristic (per interval)
Choose destination maximizing:

`utility = wage_score + distance_score + conditions_score + stability_score + social_score`

- **wage_score:** normalized offered pay.
- **distance_score:** travel time and congestion cost.
- **conditions_score:** pollution, shift length, safety.
- **stability_score:** recent layoffs/closures.
- **social_score:** where peers currently go.

Workers can:
- commute,
- idle in crowds,
- protest,
- leave town (rare, early eras),
- be replaced quickly in surplus era.

## Buyers/Transporters
- **Carts:** frequent, low volume, sensitive to price and wait time.
- **Barges:** medium frequency, medium-high volume, canal schedule dependent.
- **Railcars:** lower frequency, very high volume, contract dependent.

Spawn logic uses demand pressure and your effective offer:

`arrival_chance ∝ demand_index * price_competitiveness * fulfillment_reliability`

## Competitors (Black Box Model)
- Simulated in aggregate, not full player-equivalent detail.
- Expose only observable effects: hiring pull, buyer diversion, occasional expansion visuals.

## Politicians / State
- Event-driven meeting system; player submits requests, not laws.
- Approval depends on influence, public order, tax health, ideological climate.
- Implemented policies modify map/state physically (rails, sewers, policing, regulations).

---

## 7) Politics Loop (Paused Meetings)
### Cadence
- Trigger every X months or when crisis thresholds are crossed.

### Meeting Flow
1. Timeline pauses.
2. Official presents agenda context (budget crisis, unrest, election pressure, epidemic).
3. Player selects up to N requests.
4. Immediate yes/no/maybe outcomes with delayed implementation timers.

### Request Examples
- Rail expansion grant.
- Sewer and water sanitation.
- Extra policing near industrial district.
- Labor inspection and child labor limits.
- Canal dredging support.

### Design Principle
State is a co-author of the world, not a submenu the player owns.

---

## 8) Economy and Balancing Framework
### Core Flows
- Revenue = sold shirts × realized price.
- Costs = wages + inputs + maintenance + logistics + political rents/fines.
- Hidden long-term liabilities = pollution and unrest accumulation.

### Bottleneck Ladder (Target Progression)
1. labor access,
2. power stability,
3. local transport,
4. bulk transport,
5. social legitimacy/environment.

The game should keep moving the bottleneck so the player’s optimization target evolves.

---

## 9) Event and Shock Catalog
Use probabilistic event families with era gating:
- flood damage / drought power drop,
- enclosure/migration wave,
- export demand boom/bust,
- epidemic in dense districts,
- labor agitation movement,
- reformist government cycle.

Events should be mostly seen before read. The text panel confirms what players already suspected from motion patterns.

---

## 10) UX and Readability
- Hovering any entity shows last 10 seconds of intent arrows (where it tried to go).
- Optional overlays:
  - labor attraction heatmap,
  - congestion heatmap,
  - pollution plume,
  - unrest risk.
- “Why did this happen?” journal auto-logs causal summaries:
  - “Cart arrivals fell 22% after price increase.”
  - “Wage increase had minimal hiring effect due to labor surplus.”

---

## 11) Failure and Success States
### Fail States
- Insolvency (can’t service payroll/debt).
- Operational collapse (sustained logistics deadlock).
- Social shutdown (prolonged strike/riot forcing closure).
- Political sanction (extreme pollution + unrest triggers shutdown order).

### Success States (choose scenario length)
- Dynasty score after N years.
- Scenario goals (e.g., “Supply national contract while keeping unrest below threshold”).
- Sandbox endless mode with historical leaderboard metrics.

---

## 12) MVP Scope (First Playable)
### Must-Have
- Single town map with river, roads, one player factory, 1–2 competitors.
- Workers with destination choice based on wage + distance.
- Seats, production, warehouse, cart sales loop.
- Wage and price controls.
- Housing placement and simple migration growth.
- Visual queues for carts and workers.
- One political meeting prototype with 3 requests.

### Nice-to-Have (Post-MVP)
- Canal construction phase.
- Rail phase.
- Pollution and unrest full systems.
- Dynamic era transitions.

---

## 13) Implementation Notes (Engine-Agnostic)
- Prefer agent-lite simulation at distance (LOD):
  - full AI near camera,
  - aggregated flow farther away.
- Keep deterministic simulation ticks for replayability and balancing.
- Separate “visual actor” from “economic actor” for performance.
- Build telemetry hooks from day one:
  - seat occupancy,
  - travel time,
  - queue lengths,
  - policy acceptance rates,
  - protest triggers.

---

## 14) Example 30-Minute Session Arc
1. **0:00–8:00** — Player struggles with empty seats and low output, raises wages, builds first housing block.
2. **8:00–16:00** — Growth improves; carts become primary limit; player tweaks price and loading priority.
3. **16:00–22:00** — Canal unlock creates throughput surge then queue crisis.
4. **22:00–27:00** — Migration shock floods labor market, wages lose leverage.
5. **27:00–30:00** — Political meeting: rail petition approved; player prepares for massive scale and its consequences.

---

## 15) Why This Delivers Your Requested Fantasy
This design preserves your intended emotional arc:
- early empathy with scarcity,
- mid-game confidence through expansion,
- systemic hardening where humane levers stop mattering,
- late-game moral and political consequences visible in the world itself.

The player learns history not from cutscenes, but from watching a town move.
