# Snake Iterative Policy Evaluation

----
## What is it?

Attempt at applying the dynamic programming approach iterative policy evaluation as defined by the [Sutton and Barto Reinforcement Learning book](https://webdocs.cs.ualberta.ca/~sutton/book/the-book.html) second draft. This is mainly done to get a better understanding of reinforcement learning as I read through the book.

The actual snake code is not mine and can be found [here](https://github.com/samertm/snake-python). All of the iterative policy evaluation code is at the bottom of `snake.py`.

----
## How to run?

Simply clone and run `python snake.py`. Assumes `pygame` is available.

----
## Result

An example run of it can be seen from this [YouTube video](https://www.youtube.com/watch?v=kcCBlNgVBoY&feature=youtu.be). On average, it's only able to eat around 40 food before it gets trapped or cannot find the food.

The way I decided to model the rewards is like so:
 - Any action that results in a blocked state (either edge of the screen or the snake itself) is not possible from the standpoint of the algorithm.
 - The only reward available throughout the grid is the action the results in the food being eaten.
 - Walls and the snake body parts have a static value function of -1. The food has a very large value equal to its reward value.
 - Policy evaluation is applied 30 times before each movement of the snake. The action that then results in the highest expected reward from the current state is chosen.

## Observations

Due to the way I modeled the problem, if the food is entirely surrounded by the snake, and the snake head is on the outside, then it is not possible for expected reward to propagate outside of the enclosed area. This results in every state outside the enclosed area having a expected reward of 0 meaning that the snake is pretty much roaming with no good action available. I actually did not implement randomly selecting an action from 2 or more equally expected return actions, but this probably would not help much in this case. This exact problem can be seen in the video above when the snake starts to just go up and down towards the left compacting all of the space available.

Not unexpectedly, the resultant policy is very similar to a uniform cost search since the expected value will obviously get larger as you get closer to the food. One thing that is different is how the snake prefers to stay compact even at the cost of taking a slightly longer path. This can also be seen in the video. I have not investigated the reason, but it can also sometimes cause the snake to run into a dead-end in a situation like so:

```
  BBB
F B BB
   H B
   BBB
```

Maybe not the exact situation as given, but in a situation similar to the one above where B is a snake body part, F is food, and H is the snake head, sometimes the snake will choose to run directly into the middle of the B's, for a reason I have not investigated.
