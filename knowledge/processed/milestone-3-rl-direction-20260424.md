I’m mapping this into your existing trading-system shape so it stays narrow and disciplined rather than turning into a research sandbox. I’ll give you a Milestone 3 definition, a concrete project architecture, and then the ADR you should add next.

Thought for 4s

Yes — this is a strong direction if we keep it tightly bounded.

The right move is not “build a magical autonomous trader.”
The right move is:

build an RL training and evaluation capability inside your trading system, with strict domain boundaries, simulation-first design, and deterministic risk constraints around the learned policy.

That is a credible Milestone 3.

Milestone 3 — Reinforcement Learning Agent Foundation
Purpose

Introduce a narrow, simulation-based reinforcement learning capability that allows the system to train and evaluate a policy for trade decision support under controlled conditions.

This milestone should not attempt:

live trading
broker integration
production deployment
portfolio optimization across many instruments
autonomous unrestricted execution
deep learning infra sprawl
distributed training
hyperparameter platform work

This milestone should establish:

a formal RL environment over trading episodes
a bounded action space
a reward function tied to trading outcomes and rule compliance
a training adapter layer
policy evaluation workflow
clear separation between:
domain model
simulation environment
learning infrastructure
policy outputs
risk controls
What Milestone 3 should produce

At the end of Milestone 3, your repo should be able to do something like:

load historical or synthetic market episode data
construct a training environment
train a bounded RL policy
save policy artifacts and training metadata
run evaluation episodes
produce a report with:
cumulative reward
pnl-related metrics
drawdown
win/loss distribution
action distribution
policy behavior notes
rule violations or invalid-action counts

That is enough to make the system meaningfully more “agentic” without becoming bloated.

Core design principle

The learned policy must not replace the domain model.

Instead:

domain model remains canonical
risk and rule constraints remain authoritative
RL policy becomes one bounded decision component

That means the agent can propose or choose actions only within a strictly modeled environment.

This is the difference between:

disciplined trading system engineering
and a chaotic “AI trader” toy project
Recommended scope for the first RL agent

Keep the first agent narrow.

Best initial task

Train an agent to manage an already-approved trade opportunity within a single-instrument episode.

Examples:

hold
enter long
exit
maybe reduce position
maybe do nothing

Avoid in Milestone 3:

short selling
multi-asset portfolio allocation
options
complex order types
partial fill realism
intraday microstructure
slippage modeling sophistication
multi-agent training

The first agent should solve a small, legible decision problem.

Recommended RL formulation
Observation space

The observation should be compact and interpretable.

Possible contents:

current price
recent return window
moving average spread
volatility estimate
position state
unrealized pnl
bars since entry
max favorable excursion so far
max adverse excursion so far
stop distance
thesis/plan features that are safely numeric

Do not start with huge raw feature vectors.

Action space

Use a small discrete action space.

Good first version:

HOLD
ENTER_LONG
EXIT

Optional later:

REDUCE
ADD
MOVE_STOP

But for Milestone 3, three actions is enough.

Also: invalid actions should be handled explicitly.
Example:

ENTER_LONG when already in position
EXIT when flat

You can either:

mask invalid actions if framework supports it
or penalize invalid actions and treat as no-op

I recommend a domain-level action validator plus penalty/no-op semantics for simplicity.

Reward function

Reward design is where this lives or dies.

Start simple and explicit.

A practical first reward:

positive reward for realized profitable exit
negative reward for realized losing exit
small step penalty to discourage pointless inactivity
penalty for large drawdown
penalty for invalid actions
optional penalty for breaching risk thresholds

Do not optimize raw pnl only.
That often creates ugly behavior.

Better:

align reward with:
risk-adjusted outcome
disciplined execution
respecting constraints

For example, conceptually:

realized pnl component
drawdown penalty
invalid action penalty
overstaying penalty

Keep it transparent enough that you can explain why the agent learned what it learned.

Episode definition

One episode should represent one bounded trading scenario.

Possible options:

one instrument over one fixed window
one planned trade opportunity
one synthetic scenario

This is much better than “run forever over all market history.”

A clean episode has:

start state
finite horizon
terminal conditions

Terminal conditions:

max bars reached
stop hit
target hit
exit chosen
capital/risk breach
Where this fits in your architecture

Your existing system sounds domain-first. Keep that.

Use this separation:

1. Domain layer

Pure business concepts. No RL library imports.

Examples:

TradeIdea
TradeThesis
TradePlan
Position
Fill
TradeReview
RuleEvaluation
Violation

Add only minimal new domain concepts if needed:

PolicyDecision
SimulationEpisodeResult
AgentEvaluationReport

These should remain plain domain entities/value objects.

2. Application layer

Use cases / orchestration.

New use cases might include:

TrainTradingPolicy
EvaluateTradingPolicy
RunPolicyOnEpisode
GenerateAgentEvaluationReport

These coordinate repositories, simulation adapters, and policy artifacts.

3. Infrastructure layer

Concrete adapters.

New infra concerns:

RL environment adapter
policy artifact storage
training run persistence
metrics/report serialization

This is where Stable Baselines3 or another framework belongs, not in domain.

4. Simulation subdomain or bounded context

This deserves its own area, because simulation is not the same as core trade lifecycle.

Suggested boundary:

simulation/
agent/
or learning/

I recommend learning/ for the RL/training side, plus simulation/ for market episode mechanics if you want extra clarity.

Proposed project structure

Here is a clean, non-bloated direction.

src/trading_system/
  domain/
    entities/
    value_objects/
    services/
    repositories/

  application/
    use_cases/
      train_trading_policy.py
      evaluate_trading_policy.py
      run_policy_episode.py

  simulation/
    episode.py
    episode_state.py
    market_window.py
    transition.py
    reward.py
    termination.py
    action_validator.py

  learning/
    policy/
      policy_decision.py
      policy_metadata.py
      policy_repository.py
    environment/
      trading_env.py
      observation_builder.py
      action_mapping.py
    training/
      trainer.py
      training_config.py
      training_run.py
      training_metrics.py
    evaluation/
      evaluator.py
      evaluation_report.py
      evaluation_metrics.py

  infrastructure/
    json/
      ...
    rl/
      stable_baselines_trainer.py
      stable_baselines_policy.py
    files/
      policy_artifact_store.py
      report_writer.py

This keeps:

domain clean
RL isolated
simulation explicit
infra replaceable
Key architectural components
simulation/episode.py

Defines the lifecycle of a single trading episode.

Responsibilities:

initialize scenario
apply action
advance time
compute transitions
determine done/not done

This should be framework-agnostic.

simulation/action_validator.py

Knows whether an action is valid given current state.

Examples:

flat cannot exit
in-position cannot enter again
maybe no adding unless allowed

This is important because it prevents learning code from silently violating domain rules.

simulation/reward.py

Encapsulates reward logic.

This is a major design win:

easy to test
easy to revise
no reward logic buried inside environment glue
learning/environment/trading_env.py

This is the adapter from your simulation model into Gym-style environment semantics.

Responsibilities:

reset
step
observation emission
reward return
done flag
info payload

This file should translate, not own business logic.

learning/training/trainer.py

Abstraction for training engine.

Example interface:

train(config) -> TrainingRunResult

This lets you start with Stable Baselines3 and replace later if needed.

learning/evaluation/evaluator.py

Runs trained policy across holdout episodes and produces stable metrics.

This is essential. Training without evaluation becomes noise.

learning/policy/policy_repository.py

Abstraction for:

saving model artifacts
loading model artifacts
storing metadata

Do not bury policy files ad hoc in random folders.

Suggested new concepts

You do not need many new domain concepts, but a few are helpful.

PolicyDecision

Represents what the policy chose.

Example:

action selected
confidence or score if available
timestamp / bar index
policy version
TrainingRun

Represents a recorded training job.

Contains:

id
config snapshot
data source reference
algorithm
start/end time
metrics summary
artifact path
EvaluationReport

Represents evaluation outcome.

Contains:

policy id/version
dataset or scenario set
aggregate metrics
notes
per-episode summaries
Data and repository approach

Since you already chose local JSON persistence for Milestone 2, stay consistent.

For Milestone 3:

keep metadata in JSON
store model artifact files separately

Example:

.trading-system/
  data.json
  policies/
    policy-001/
      model.zip
      metadata.json
      evaluation.json

That is totally fine for this milestone.

Do not jump to:

model registry
MLflow
database-backed experiment store
cloud artifact platform

Not yet.

Training workflow

A good first workflow:

Train
load episode dataset
build environment factory
create trainer with config
train for bounded timesteps
save artifact + metadata
return TrainingRun
Evaluate
load saved policy
run on holdout episodes
aggregate metrics
persist evaluation report

This keeps training and evaluation separate, which is good discipline.

Evaluation metrics to include

Do not rely on reward alone.

Recommended first metrics:

cumulative reward
average episode reward
total realized pnl
average realized pnl per episode
max drawdown
percent profitable episodes
invalid action count
average episode length
forced termination count
action frequency breakdown

These make the policy inspectable.

Guardrails you absolutely want

The RL system should be constrained by deterministic rules.

Examples:

maximum risk per trade
no action that violates position state
no pyramiding unless allowed
terminate episode on catastrophic breach
log violations separately

This is architecturally important because it mirrors how real trading systems are built:

learning component proposes
deterministic controls constrain
Suggested issue roadmap for Milestone 3

Here is the clean roadmap.

Issue 1 — Define RL bounded scope and learning architecture

Deliverables:

milestone doc
domain boundary definition
architecture notes
terminology
Issue 2 — Add simulation episode model

Deliverables:

episode state
transitions
terminal conditions
action validator
Issue 3 — Add reward and observation model

Deliverables:

reward function module
observation builder
tests for reward semantics
Issue 4 — Implement Gym-compatible trading environment adapter

Deliverables:

environment wrapper
reset/step behavior
info payload
tests
Issue 5 — Add trainer abstraction and first Stable Baselines3 adapter

Deliverables:

trainer interface
PPO-based implementation
training config
Issue 6 — Add policy artifact storage and training run metadata

Deliverables:

artifact directory structure
metadata persistence
load/save policy behavior
Issue 7 — Add evaluation workflow and report generation

Deliverables:

evaluation runner
evaluation metrics
report output
Issue 8 — Add CLI workflow for train/evaluate demo

Deliverables:

demo command or script
sample run instructions
output paths
Issue 9 — Documentation, ADR, and milestone closeout

Deliverables:

README updates
milestone doc
ADR
future roadmap note
Practical first algorithm choice

Use PPO first.

Why:

stable
widely used
simpler than getting fancy
good enough for the first bounded problem

Do not start with:

SAC
multi-agent RL
transformers
custom actor-critic stack
offline RL research ideas

Just get the capability working.

What not to do in Milestone 3

Avoid these traps:

using RL directly on live brokerage execution
mixing backtesting engine complexity into training core
adding too many indicators/features
trying to predict market direction and execute simultaneously with no bounded setup
introducing LLM-driven decisions into the policy loop
skipping evaluation and relying on “it seems smart”
overfitting historical data and calling it intelligence
Recommended ADR

Yes, you should add an ADR next.

The ADR should capture this decision:

ADR-005 — Use a simulation-first, bounded reinforcement learning architecture for policy training and evaluation

Below is a drop-in-ready draft.

ADR-005: Use simulation-first bounded RL for trading policy experimentation
Status

Accepted

Context

The system currently models a disciplined, domain-driven trading workflow with explicit entities, repositories, rule evaluation, and durable persistence. The next stage of development introduces reinforcement learning to explore whether a bounded policy can learn useful trade decision behavior inside controlled scenarios.

A direct jump from current domain workflows to live or unconstrained autonomous trading would create excessive architectural risk, blur domain boundaries, and encourage premature complexity. Reinforcement learning systems also require a clearly defined environment, observation model, action space, reward function, and evaluation workflow. These concerns are materially different from the core trade lifecycle and should not be allowed to leak across the domain model.

The project needs an approach that:

preserves clean architecture and domain boundaries
enables training and evaluation in a safe and reproducible way
supports future experimentation without committing to production autonomy
keeps deterministic risk controls authoritative over learned behavior
avoids premature infrastructure complexity
Decision

The system will adopt a simulation-first, bounded reinforcement learning architecture for Milestone 3.

Reinforcement learning will be introduced only through a dedicated learning/simulation capability that operates on finite trading episodes. The learned policy will act within a narrow, explicitly modeled action space and will be evaluated only in simulation. Deterministic domain rules and risk constraints will remain authoritative.

The architecture will separate concerns as follows:

the domain layer remains framework-agnostic and canonical
simulation components model episode state, transitions, termination, and reward behavior
a Gym-compatible environment adapter exposes simulation behavior to RL tooling
training and evaluation workflows live in application/learning orchestration
RL framework integration is implemented in infrastructure adapters
policy artifacts and training metadata are persisted separately from core domain records

The initial RL scope will be intentionally narrow:

single-instrument episodes
small discrete action space
simulation-only training and evaluation
no broker integration
no live trading
no distributed training
no multi-agent systems
no portfolio optimization

The first algorithm will be PPO through an infrastructure adapter. This is a pragmatic initial choice, not a permanent commitment.

Consequences
Positive
Preserves the integrity of the core domain model
Creates a safe experimentation boundary for RL
Makes reward, action, and observation design explicit and testable
Supports reproducible evaluation and artifact persistence
Allows future replacement of RL libraries without domain churn
Keeps deterministic controls in charge of risk behavior
Negative
Adds a new architectural area for simulation and learning
Requires careful design of reward and evaluation metrics
Simulation results may not transfer well to real markets
Early policies may appear promising while being brittle or overfit
Additional documentation and testing burden is introduced
Rejected alternatives
1. Embed RL logic directly into domain entities or services

Rejected because it would couple learning framework concerns to canonical business logic and make testing, replacement, and reasoning more difficult.

2. Train directly against live execution workflows

Rejected because it is unsafe, premature, and inconsistent with the project’s current maturity.

3. Defer RL until production integrations are complete

Rejected because simulation-first experimentation is a reasonable way to explore learning behavior early without taking on live trading risk.

4. Build a large-scale ML platform now

Rejected because experiment tracking stacks, distributed training, and registry platforms would add major complexity before the core learning problem is proven useful.

Implementation notes
Keep simulation logic framework-agnostic
Use bounded episodes and explicit terminal conditions
Keep the first action space small and discrete
Persist model artifacts separately from JSON metadata
Record training configurations and evaluation summaries
Treat reward design as a first-class, testable concern
Ensure invalid actions and rule breaches are visible in metrics and reports
Follow-up

Milestone 3 should implement:

episode model
observation builder
reward function
environment adapter
PPO trainer adapter
artifact storage
evaluation reporting
CLI training/evaluation demo
documentation updates
My recommendation on naming

Call the milestone something like:

Milestone 3 — Reinforcement Learning Simulation and Policy Evaluation

That is better than:

“AI trader”
“autonomous trading agent”
“intelligent trading engine”

Because it keeps the scope honest.

Best next step

I recommend you create three documents next:

docs/milestones/milestone-3-reinforcement-learning-simulation-and-policy-evaluation.md
docs/adr/ADR-005-simulation-first-bounded-reinforcement-learning.md
docs/raw/milestone-3-roadmap.md or your equivalent knowledgebase location

After that, the first coding issue should be:

Issue M3-01 — Define simulation episode model, action space, and reward boundary

That is the correct starting point, not the trainer.

I can draft the Milestone 3 document next in full drop-in-ready form.