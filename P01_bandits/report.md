# P01 report — <your name>, <student ID>

Keep it to one page, about five lines per answer, written in the body of your
email. Three figures: `regret.png`, the one for Q2, and the one after your Part C
change. Delete these instructions before submitting.

---

## Q1. The three growth rates

Attach `regret.png`. For each of the five algorithms, say which of the slopes in
the right-hand panel it matches, if any, and explain **why** in one sentence per
algorithm, referring to what the algorithm does — not to what the theorem says.

Greedy is the interesting one: its curve is a straight line in the left panel and
its spread across seeds is enormous. Explain both facts with the same argument.

![[regret_og.png]]

- **Greedy:** It has a linear slope because it commits after one pull, may be lucky and pick the best arm and with zero regret or unlucky and pick the worst arm explaining why spread across seed is enormous.

- **ETC:** It has a first flat slope part which is the exploration part and then commits, in this case the exploration is enough to always commit to the best arm.

- **UCB and Thompson:** They never stop exploring so they continuosly bend down the optimal arm.

## Q2. The bound is 17x loose. Is that a problem?

`experiments.py` prints the empirical UCB regret and the gap-dependent bound
$\sum_a 8\log T / \Delta_a$. The bound is about seventeen times larger than what
actually happens. Answer, in five lines: is the theorem wrong, is the experiment
wrong, or neither? What is the bound good for, if not for predicting the number?

Second part, and there is no "official" answer: at $T = 4000$ on this instance,
the median curve of explore-then-commit ends far **below** UCB, even though its
bound is $T^{2/3}$ and UCB's is $\sqrt{T}\log T$; its mean, printed by
`experiments.py`, is about level with UCB's. Reconcile the three facts. What
experiment would settle which of the two is better? Run it and attach the second
figure.

The theorem is not wrong neither the experiment. The bound is not good on predicting what will happen next but it predicts the maximum regret a run may produce. So the theorem does not say what is the expected value but it says how much is the maximum regret a run may encounter.

![[regret400k.png]]

In this experiment ETC is able to pull the optimal arm for at least half of the runs, so we can observe that the regret grows linearly in the exploration part and flattens in the exploitation part because the median is 0 while UCB continues on learning. To observe which algorithm is better we have to experiment a long run. In a long run the regret when ETC chooses a suboptimal arm explode and we can observe an higher mean.

## Q3. Breaking UCB

State the prediction you made **before** running Part C, then what happened, then
whether your explanation survived. If you predicted correctly for the wrong
reason, say so: it is worth more than a lucky guess.

![[regret_partc.png]]

My prediction was that given that the delta was a constant the confidence bound may only decrease because of $n$ so it would actually help UCB mantain more control on the bounds that would shrink continuosly based on how much an arm is pulled. It actually happened what i did predict. The exeption is when in a particular unlucky run the optimal arm gets a bound lower than a suboptimal arm may reach and there is no possibility to recover.

## Declaration

Time spent: ~6 h. Collaborators: Lorenzo Ventrone. AI tools used and for what: Gemini for understanding the questions.
(Using them is allowed and expected to be declared, exactly as for a human
collaborator: you may not ask for the solution, and you are responsible for what
you submit.)
