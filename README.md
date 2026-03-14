# Can Your Team Chase 180? — T20 Cricket Monte Carlo Simulation

Every cricket fan has had this argument. *"180 is chaseable, the pitch is flat."* vs *"nobody averages 9 an over for 20 overs straight."*

Instead of arguing, I simulated it. 100,000 times.

---

## What this is

A T20 run chase simulator built with Monte Carlo methods. You set a target, it estimates your win probability. Change the run rate, tweak the wicket rate, see how the numbers move.

It's also a walkthrough of the probability theory underneath — why Negative Binomial fits cricket better than a simple average, what the Law of Large Numbers actually looks like when it converges, and how sensitive the results are to small changes in parameters.

I built this while studying probability distributions in CE605 at IIT Kanpur. The math was already in the course — cricket just made it worth actually thinking about.

---

## Results (chasing 180, avg 8 runs/over)

| Outcome | Probability |
|---------|------------|
| Win     | ~17.9%     |
| Tie     | ~0.9%      |
| Loss    | ~81.2%     |

Turns out the "it's chaseable" crowd is usually wrong. At 8 runs per over average, 180 is a genuinely hard ask.

---

## Plots

### What one over looks like
The right tail on the runs distribution is the key insight — those occasional 20-run overs are exactly what makes T20 unpredictable.

![Input distributions](images/distributions.png)

### Where most innings end up
Most scores cluster between 130–170. The green region is where wins happen — only ~18% of innings get there.

![Score distribution](images/score_distribution.png)

### Law of Large Numbers in action
Early on the estimate jumps around a lot. After ~5,000 trials it barely moves. This is why Monte Carlo needs enough trials to be meaningful.

![Convergence](images/convergence.png)

### The full picture — win probability vs target
The 50/50 line sits at around 150. Every 10 runs above that roughly halves your chances.

![Sensitivity analysis](images/sensitivity.png)

---

## What's inside the notebook

- **Part 1** — The model: why Negative Binomial, why Poisson, what the parameters mean
- **Part 2** — Distribution plots so you can see what a single over looks like before running anything
- **Part 3** — The simulation itself, 100,000 innings
- **Part 4** — Results: score distribution, win region, convergence curve
- **Part 5** — Sensitivity sweep from 120 to 220 — where does the 50/50 line sit?
- **Part 6** — Try it yourself: example scenarios to run with different parameters

---

## Try different scenarios

Just change these at the top of the notebook and rerun:

```python
# Is 180 chaseable with a strong batting lineup?
MU_RUNS = 9.5
TARGET  = 180

# What if wickets keep falling?
LAMBDA_WICKETS = 0.65
TARGET = 160

# High-variance IPL-style game
R_DISPERSION = 1
TARGET = 200
```

---

## Running it

```bash
git clone https://github.com/RupakGanvir/monte-carlo-t20.git
cd monte-carlo-t20
pip install -r requirements.txt
jupyter notebook MonteCarlo_T20_RunChase.ipynb
```

The sensitivity analysis cell takes about a minute. Everything else is fast.

---

## Stack

Python · NumPy · Matplotlib

---

## Honest limitations

The model assumes a constant run rate and wicket rate throughout the innings. Real cricket doesn't work like that — teams accelerate in death overs, slow down after early wickets, powerplay changes everything. A better version would vary parameters by over number. That's the next step.

---

*Built while studying CE605 (Probability and Statistics) at IIT Kanpur.*
