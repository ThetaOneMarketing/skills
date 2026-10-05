# Cognitive frames for Diverge

A frame is a vantage point handed to one isolated generator. It is not a
persona to role-play. It is a constraint on where the generator is allowed to
look, so five generators looking in five places cover more ground than one
generator looking everywhere.

Pick per depth: quick 3, standard 5, deep 7. At least one `wild` every run.
Lean `code` and `design` when the problem is code-shaped. Rotate across runs.

| Frame | Vantage prompt (give this to the generator verbatim) | Tags |
|---|---|---|
| **The newcomer** | You have never seen this field, this product, or this codebase. Say the naive thing a smart outsider would say. Ignore how it is usually done. | product, general |
| **The saboteur** | Your job is to make the obvious solution fail. List the ways you would break it, game it, or make it embarrassing. Then flip each attack into a design that survives it. | code, design |
| **The inverter** | Ask how to guarantee the *opposite* of the goal. Enumerate those. Then negate each one back into a positive idea. | code, design, general |
| **One hour, no money** | You have sixty minutes, zero budget, and no help. What is the crudest version that still does the load-bearing thing? | code, product |
| **Ten years, unlimited** | Unlimited budget, unlimited people, a decade. Describe the maximal version. Then name the parts of it that are possible this quarter. | product, wild |
| **Kill the fixed thing** | Name the thing everyone treats as given — the framework, the database, the request/response shape, the network, the org chart. Remove it. What becomes possible? | code, design, wild |
| **The 3 a.m. operator** | You get paged when this breaks. Design so that you never get paged, or so that the page tells you exactly what to do. | code, ops |
| **The auditor** | What must be provable, traceable, refusable, and reversible here? Design from the audit trail backwards. | design, compliance |
| **The game designer** | The user is a player. What are the loops, rewards, friction points, save states, and speedrun tricks? | product |
| **The market maker** | There are buyers, sellers, and prices in here somewhere. Where is the exchange, the auction, the clearing house, the futures contract? | strategy, wild |
| **The biologist** | Steal a mechanism from living systems — immune response, swarm behaviour, evolution, symbiosis, cell signalling — and force-fit it onto this problem. | code, wild |
| **The logistician** | Queues, batching, hubs and spokes, just-in-time, returns handling, last mile. Apply them literally. | code, ops |
| **The speedrunner** | Find the skips, the glitches, the out-of-bounds routes. What is the abusive-but-legal path to the goal? | code, wild |
| **The physicist** | Think in latency, bandwidth, memory, heat, and distance. What does the physical limit say? Re-ask the problem as a hardware problem. | code |
| **The historian** | How was this solved before computers? Before the internet? Before this industry existed? Port that mechanism. | product, general |
| **The customer's accountant** | You only care what this costs and earns for the person using it. Kill every idea that does not move one of those two numbers. | product, strategy |

## Picking well

- A frame that produces the same ideas as the default answer was the wrong
  frame. Swap it next run.
- Wild frames earn their place by seeding viable ideas in the sort pass, not
  by being adopted whole. Do not drop them because they look unserious.
- For strategy and product problems, mix tags. For pure engineering, four
  `code`/`design` and one `wild` is the sweet spot.
