![Playpen Logo](playpen-logo.png)

# The LM-Playschool Challenge -- Round β.1

## What's that?

LLMs are increasingly used as *agents* that engage in multi-step interactions, yet they are trained either on purely observational data (text corpora) or on single stimulus-response pairs (instructions and replies). Could learning in multi-step conversational interaction (in what we call "dialogue games") improve their agentic capabilities? The LM-Challenge aims at finding out. We are currently preparing a large scale public edition of the challenge, to be attached to a major NLP conference in 2026 (hopefully). But you can contribute now already, in 2025! We're running an informal version of the challenge now, to fine tune the instructions and conditions.

## How can I contribute?

- Check out our pre-print, explaining the synthetic data / interaction generation / game play framework, playpen: <https://arxiv.org/abs/2504.08590>.
- Check out the playpen itself: <https://github.com/lm-playpen/playpen>.
- Use the training set of the playpen (or any other kind of data source!) to produce a (playpen) agent, based on Llama-3.1-8B-instruct; using not more than 100M training tokens.
- Run the included evaluation pipeline. 
- Make a pull request on this repository, with a new row added to the table below containing the numbers you got from the evaluation. Add some details about what you did, and a link to your repo.

We also very much welcome any kind of feedback on the framework, on these rules, and whatever you have comments on! File issues here or on the Playpen repository, or make pull request with suggested changes.

## What's in it for me?

For this beta-testing round, we can't offer any cash prizes, or much of anything really, other than our gratitude for helping iron out the kinks out of the framework and the challenge setup. Any discoveries you make will of course remain yours, so if you hit on a new reinforcement learning algorithm that ushers in the golden age of AGI, the fame and fortune would be all yours.



## The leaderboard (dev phase)

The following table lists the results from the `playpen eval` on the [playpen-data](https://huggingface.co/datasets/colab-potsdam/playpen-data) validation splits.
In particular, the clemscore is computed from the model's performance on the sub-selection of instances playing the text-only clemgames (v2), and the statscore is computed from the model's performance on the sub-selection of task for the static benchmarks (v1) (CLadder, EQBench, IFEval, MMLUPro).

| Rank | Submission Name | Date | Team Name | clemscore | statscore | Short Description |
| ---------- | ------- | ------- | ------- | -------|-----------|-----|
| 1 | Llama-3.1-8b-sft-combined | 2025-07-17 | Team Potsblitz | 42.68      | 53.25     | Model trained on combined SFT data from clemgames and Tülu SFT data |
| 2 | Llama-3.1-8B-PotsBlitz-2      | 2025-07-15 | Team Potsblitz | 34.21     | 32.70      | 4bit quantized, trained on game instances ordered by (human-defined) difficulty |
| 3 | Llama-3.1-8B-Instruct (base) | 2025-07-03 | OrgTeam | 29.05      | 55.45     | The unmodified base model (*updated 2025-07-16, fixed eval pipeline*) |
| 4 | Llama-3.1-8B-It-4bit-game-specific-instructions | 2025-08-24 | TeamPotzblitz | 27.66 | 50.47 | 4bit quantized model DPO-tuned on 2800 game-specific instructions 
| 5 | Llama-3.1-8B-It-4bit (base) | 2025-07-03 | Team Potzblitz | 27.35 | 49.16       | The base model, 4bit quantized (*updated 2025-08-22, added statscore*) |
| 6 | Llama-3.1-8b-dpo-turn-filtered | 2025-07-17 | Team Potzblitz | 19.26 | 56.40       | Model trained on combined filtered DPO Turn dataset and Tülu DPO data |
| 7 | Llama-3.1-8B-It-4bit-dpo-if | 2025-07-03 | Team Potsblitz | 12.63 | 47.92       |  4bit quantized model DPO-tuned on allenai/tulu-3-pref-personas-instruction-following dataset (*updated 2025-08-22, added statscore*)
| 8 | R1-distill-Llama-8b | 2025-09-03 | Team Potsblitz | 6.77 | 46.37 | R1-distill-llama-8b

Here's how the sorting works (will eventually work): Entries will be sorted by clemscore (higher is better), <strike>but only those entries will enter the sorting that have a statscore that is not lower than that of the baseline agent.</strike> (This is to ensure that your agent does not regress on other desirable properties measured by the static benchmarking pipeline. (And no, obviously you may not add any of that test data to your training data.))


## The leaderboard (test phase)

The dev leaderboard stays open until September 15th 2025. At that point, we will ask the 5 highest scoring teams to submit their agent (the agent code and the model weights), as well as a log of their training run. We will then run the evaluation pipeline, but on a set of held-out games (for clemscore), and re-rank based on the outcome of this.





## Some ideas

Here are some things you're explicitly allowed and encouraged to do:

- Hack the playpen games! If you have an idea for how to get an even better learning signal out of the games, by all means implement it. For example, you could try to set things up so that the players in the game (or the game master) provide corrective feedback, or evaluative feedback, that could be turned into a more dense reward signal.
- Experiment with curriculum learning. Many of the games come with instances sorted in to experiments of different difficulty. Maybe it helps to present these in order?
- Generally give the trainer more control over what the learner sees. If you have a method that could predict which instance would give the strongest learning signal at a given stage, try it out!
- Come up with neuro-symbolic hybrid agents! We only require that your agent responds to generic prompts (and hence can be prompted to run typical reference-based benchmarks), and that it contains as a central element a model that is derived (by you) from the base model. If you have an idea for a cognitive architecture that you could build around it (e.g., with a more explicit handling of memory, or of reasoning steps, etc.), try it!


## The fine print

*For this beta round of the challenge, we will use somewhat more relaxed rules than in the real challenge. These rules put fewer constraints on the participants, to allow for wider experimentation on what works and what doesn't. Comments on these rules are very welcome!*


This competition aims at fostering research on learning in conversational, task-oriented, multi-turn environments. As a consequence, all submissions must start from the same base LLM and must adhere to the same budgets with respect to training data and evaluation compute resources. *[For this beta round, there will be no limit on the training budget; you are just asked to report it as accurately as possible.]*
Each team will share a report that describes their approach, as well as important findings resulting from the evaluation. The following rules apply:

- Each submitted agent must be the result of a training run that starts with the base LLM distributed with the starter kit. There are no restrictions on the architecture, type of training data, or training algorithm, but a training run may not consume more than 100M tokens. *[For β.1, there is no limit on the size of the training data; it just has to be reported according to these guidelines.]* Consuming the same training token `n` times (e.g. across multiple epochs) counts as `n` tokens. Participants must submit a Playpen training log along with their agent to enable automatic checking of this rule.

- During the evaluation, the submitted agent will be made to play dialogue games that were unseen in training and make predictions on reference-based benchmarks that we will not announce beforehand.

- The agent will be evaluated on self-play: In a dialogue game that has room for multiple agents, the submitted agent will play all roles. Participants are free to use a different LLM as the partner agent during training, but should be aware of the risk of distribution shift.

- Participants will submit their agents as subclasses of the Playpen `agent` class, through a mechanism TO BE DETERMINED. Agents may not use any LLMs except for one fine-tuned version of the base LLM. They can download this fine-tuned model from the Internet, e.g. through Huggingface, but are not allowed any other form of Internet access.

-  During the evaluation, the submitted agents will be run on an Nvidia A100 GPU or similar. There is a time limit of two hours for each evaluation run, after which the agent will be terminated and the evaluation run will be counted as failed.

- Agents must not change the parameters of their LLM during evaluation (e.g. through continued reinforcement learning).
