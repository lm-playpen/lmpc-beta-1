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

| Rank | Submission Name                                                                                         | Date | Team Name | clemscore | statscore | Short Description |
|------|---------------------------------------------------------------------------------------------------------| ------- |-----------| -------|-----------|-----|
| 1    | Llama-3.1-8b-sft-combined                                                                               | 2025-07-17 | ULN       | 42.68      | 53.25     | Model trained on combined SFT data from clemgames and Tülu SFT data |
| 2    | [Llama-3.1-8B-curriculum](https://huggingface.co/alextsiak/llama8b-instruct-hardcoded-curriculum-final) | 2025-07-15 | ZATZ      | 34.21     | 32.70      | 4bit quantized, trained on game instances ordered by (human-defined) difficulty |
| 3    | [Llama-3.1-8b-grpo](https://huggingface.co/pm-25/llama3-8b-grpo)                                        | 2025-09-14 | ULN     | 32.82 | 57.86 | Base model trained on DPO data from clemgames, using custom scoring functions for verifiable reward computation |
| 4    | Llama-3.1-8B-Instruct (inference-supervised)                                                | 2025-09-15 | ZATZ     | 32.3 | 55.5 | Supervised playing architecture. The player (Llama-3.1-8B-Instruct) receives advice on how to improve its play from the advisor (Qwen2.5-32B-Instruct) during inference |
| 5    | Llama-3.1-8B-Instruct (base)                                                                            | 2025-07-03 | CLP       | 29.05      | 55.45     | The unmodified base model (*updated 2025-07-16, fixed eval pipeline*) |
| 6    | Llama-3.1-8B-It-4bit-game-specific-instructions                                                         | 2025-08-24 | ZATZ      | 27.66 | 50.47 | 4bit quantized model DPO-tuned on 2800 game-specific instructions |
| 7    | Llama-3.1-8B-It-4bit (base)                                                                             | 2025-07-03 | ZATZ      | 27.35 | 49.16       | The base model, 4bit quantized (*updated 2025-08-22, added statscore*) |
| 8    | Llama-3.1-8b-curriculum-2                                                                               | 2025-09-14 | ZATZ      | 26.84 | 38.53 | Trained on game instances ordered by (model-defined) difficulty |
| 9    | [Llama-3.1-8b-sft-grpo](https://huggingface.co/pm-25/llama3-8b-sft-grpo)                                | 2025-09-14 | ULN     | 26.68 | 57.86 | Llama-3.1-8b-sft-combined trained on DPO data from clemgames, using custom scoring functions for verifiable reward computation |
| 10   | Llama-3.1-8b-failure-learning                                                                           | 2025-09-14 | ZATZ      | 21.10 | 57.95 | Model finetuned on a subset of clemgames consisting only of tasks the base model failed |
| 11   | Llama-3.1-8b-dpo-turn-filtered                                                                          | 2025-07-17 | ULN     | 19.26 | 56.40       | Model trained on combined filtered DPO Turn dataset and Tülu DPO data |
| 12   | Llama-3.1-8B-Instruct (inference-ranking)                                                               | 2025-09-15 | ZATZ      | 16.05 | 46.87 | Reasoning architecture. The player (Llama-3.1-8B-Instruct) generates several answers, which are scored by the PRM (RLHFlow/Llama3.1-8B-PRM-Deepseek-Data); the best answer is then selected using a reasoning strategy (beam search) |
| 13   | Llama-3.1-8B-It-4bit-dpo-if                                                                             | 2025-07-03 | ZATZ      | 12.63 | 47.92       |  4bit quantized model DPO-tuned on allenai/tulu-3-pref-personas-instruction-following dataset (*updated 2025-08-22, added statscore*) |
| 14   | R1-distill-Llama-8b                                                                                     | 2025-09-03 | ZATZ      | 6.77 | 46.37 | R1-distill-llama-8b |



Here's how the sorting works (will eventually work): Entries will be sorted by clemscore (higher is better), <strike>but only those entries will enter the sorting that have a statscore that is not lower than that of the baseline agent.</strike> (This is to ensure that your agent does not regress on other desirable properties measured by the static benchmarking pipeline. (And no, obviously you may not add any of that test data to your training data.))


## The leaderboard (test phase)

The dev leaderboard stays open until September 15th 2025. At that point, we will ask the 5 highest scoring teams to submit their agent (the agent code and the model weights), as well as a log of their training run. We will then run the evaluation pipeline, but on a set of held-out games (for clemscore), and re-rank based on the outcome of this.

| Rank | Model                                              | Team | clemscore (holdout) (P/Q) | clemscore (test) (P/Q) | clemscore (val) |
|------|----------------------------------------------------|------|--------------------------|------------------------|---------------|
| 1    | Meta-Llama-3.1-8B-Instruct-t0.0                    | CLP  | 11.23 (79.35/14.15)      | 26.86 (57.33/46.86)    | 29.05 |
| 2    | llama3-8b-it-4bit-game-specific-instructions-t0.0  | ZATZ | 9.86 (76.28/12.92)       | 26.67 (60.12/44.36)    | 27.66 |
| 3    | llama3-8b-grpo-t0.0                                | ULN  | 9.30 (76.08/12.23)       | 28.93 (39.96/57.07)    | 32.82 |
| 4    | llama3-8b-sft-combined-t0.0                        | ULN  | 3.13 (85.99/3.64)        | 45.39 (35.91/76.67)    | 42.68 |
| 5    | llama3-8b-sft-curriculum-t0.0                      | ZATZ | 0.56 (37.04/1.50)        | 17.56 (43.82/40.07)    | 34.21 |



Detailed in-domain results:

| Rank | Model | - clemscore | all, Average % Played | all, Average Quality Score | adventuregame, % Played | adventuregame, Quality Score | codenames, % Played | codenames, Quality Score | guesswhat, % Played | guesswhat, Quality Score | imagegame, % Played | imagegame, Quality Score | matchit_ascii, % Played | matchit_ascii, Quality Score | privateshared, % Played | privateshared, Quality Score | referencegame, % Played | referencegame, Quality Score | taboo, % Played | taboo, Quality Score | textmapworld, % Played | textmapworld, Quality Score | textmapworld_graphreasoning, % Played | textmapworld_graphreasoning, Quality Score | textmapworld_specificroom, % Played | textmapworld_specificroom, Quality Score | wordle, % Played | wordle, Quality Score | wordle_withclue, % Played | wordle_withclue, Quality Score | wordle_withcritic, % Played | wordle_withcritic, Quality Score |
|------|-------|-------------|-----------------------|----------------------------|--------------------------|------------------------------|--------------------|---------------------------|---------------------|--------------------------|---------------------|---------------------------|--------------------------|-----------------------------|--------------------------|-------------------------------|--------------------------|--------------------------------|-----------------|---------------------|------------------------|-----------------------------|------------------------------------|---------------------------------------|-----------------------------------|--------------------------------------|------------------|-------------------|----------------------------|--------------------------------|-----------------------------|--------------------------------|
| 1    | llama3-8b-sft-combined-t0.0    | 45.39     | 35.91             | 76.67            | 63.28          | 59.2                  | 64.44       | 46.67             | 98.33       | 48.59             | 100.0       | 75.05             | 100.0           | 75.0                  | 100.0           | 93.15                 | 100.0           | 38.89                 | 100.0   | 43.06         | 94.0           | 69.96                 | 33.33                          | 62.07                             | 100.0                       | 100.0                           | 46.67    | 26.73          | 33.33             | 61.67                   | 40.0                | 50.0                       | 52.22 |
| 2    | llama3-8b-grpo-t0.0            | 28.93     | 39.96             | 57.07            | 38.28          | 50.7                  | 54.44       | 40.72             | 88.33       | 15.09             | 55.0        | 51.27             | 77.5            | 74.19                 | 78.0            | 50.46                 | 100.0           | 34.44                 | 95.0    | 44.44         | 32.0           | 54.0                  | 23.81                          | 37.9                              | 76.67                       | 100.0                           | 53.33    | 7.5            | 13.33             | 100.0                   | 13.33               | 58.33                      |
| 3    | Meta-Llama-3.1-8B-Instruct-t0.0 | 26.86 | 57.33 | 46.86 | 45.31 | 63.79 | 54.44 | 20.41 | 88.33 | 16.35 | 58.33 | 47.86 | 72.5 | 75.86 | 74.0 | 52.29 | 100.0 | 35.56 | 95.0 | 39.47 | 28.0 | 51.53 | 33.33 | 34.19 | 76.67 | 100.0 | 50.0 | 0.0 | 13.33 | 56.25 | 13.33 | 62.5 |
| 4    | llama3-8b-it-4bit-game-specific-instructions-t0.0 | 26.67 | 60.12 | 44.36 | 34.38 | 66.67 | 46.67 | 19.05 | 90.0 | 12.35 | 68.33 | 56.71 | 100.0 | 65.0 | 100.0 | 24.21 | 100.0 | 38.89 | 100.0 | 34.17 | 38.0 | 53.7 | 47.62 | 58.79 | 80.0 | 100.0 | 36.67 | 2.73 | 0.0 | 0 | 0.0 | 0 |
| 5    | llama3-8b-sft-curriculum-t0.0 | 17.56 | 43.82 | 40.07 | 16.41 | 60.32 | 5.56 | 0.0 | 96.67 | 31.61 | 0.0 | 0 | 100.0 | 45.0 | 52.0 | 32.74 | 85.56 | 42.86 | 100.0 | 33.89 | 24.0 | 62.18 | 0.0 | 0 | 66.67 | 100.0 | 30.0 | 11.11 | 6.67 | 50.0 | 30.0 | 11.11 |


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
