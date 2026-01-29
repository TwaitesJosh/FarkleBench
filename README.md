# FarkleBench

**Novel game-theoretic benchmark for evaluating strategic reasoning capabilities in large language models through competitive gameplay scenarios**

FarkleBench is a benchmarking framework that evaluates large language models (LLMs) by having them design strategies for the dice game **Farkle**, then compete in automated tournaments.

> ⚠️ **Work in progress** – the core framework is written, but requires cleaning before upload. Additional features, metrics, and visualizations are coming.

---

## What is Farkle?

Farkle is a turn-based dice game where players try to accumulate points by rolling six dice. On each turn, players must decide whether to:

* **Roll again** to try to score more points (but risk losing points if they “Farkle” — roll no scoring combinations)
* **Stop** and bank the points accumulated so far

Points are scored through specific dice combinations (e.g., ones, fives, three-of-a-kind, straights). The first player to reach a target score (typically 10,000 points) wins.

Farkle is ideal for strategic reasoning because:

* Each turn involves **risk vs. reward** decisions
* **Opponent scores** influence whether a player should play aggressively or conservatively
* **Small rule variations** can dramatically change optimal strategies

FarkleBench leverages this environment to test LLMs on **strategic decision-making, adaptability, and long-term planning**.

---

## How it works

An LLM is prompted to write a Python class that optimizes for specific Farkle rule sets. It must implement a single decision interface:

```python
def should_I_roll_or_stop(
    my_score: int,
    opponent_scores: list[int],
    remaining_dice: int,
    current_turn_points: int,
    rules: dict
) -> bool:
    ...
```

The function decides whether the player:

* ✅ rolls again
* 🛑 stops and banks points

The framework then:

1. Loads multiple LLM-generated strategies
2. Applies configurable rule sets
3. Runs a round-robin tournament
4. Aggregates statistics
5. Produces rankings and performance metrics

Strategies are evaluated not just on **win rate**, but on **consistency across rule variants**.

 _Note; in the future we will get the LLM to write the function automatically, at the moment we just paste it in._
 
---

## Installation (Coming soon)

```bash
git clone https://github.com/yourname/farklebench
cd farklebench
pip install -r requirements.txt
```

---

## Running a tournament (Coming soon)

```bash
python run_tournament.py
```

Example output:

```
=== FarkleBench Tournament Results ===

GPT-Strategy-A     62.3% win rate
Claude-Strategy-B  58.9% win rate
Gemini-Strategy-C  54.1% win rate
Baseline-Greedy    41.7% win rate
```

You can configure:

* number of games
* rule variations
* scoring thresholds
* tournament format
* random seeds

via the config file:

```
config/tournament.yaml
```

---

## Adding a new strategy

1. Create a file in `players/`
2. Implement the strategy function
3. Register the player in `registry.py`

Example:

```python
class MyStrategy(FarklePlayer):
    def should_I_roll_or_stop(...):
        return current_turn_points < 500
```

Then rerun the tournament.

---

## Metrics

FarkleBench records:

* Win rate
* Average score
* Variance
* Aggression index (roll tendency)
* Expected value per turn

These metrics help analyze **how** strategies behave, not just who wins.

---

## Roadmap (WIP)

* Elo-style ranking system
* Self-play evolution experiments
* Visualization dashboard
* Strategy introspection tools
* Tournament replay viewer
* Human vs LLM tournaments

---

## License

MIT License
