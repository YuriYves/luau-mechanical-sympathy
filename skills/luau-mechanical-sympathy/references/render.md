## Objective
The objective of this skill is to teach the agent how to better separate graphics-related rendering tasks, in order to implement smarter algorithmic solutions and increase the potential frames per second.

## Investigation

Given a budget, and knowing that if a running code must run at 60 FPS, the maximum rendering time for the scene is 16.66 ms. Within the space itself, time should be divided among other tasks that are not part of the current code execution process. Thus becoming necessary the need to share the frame budget to the processor.

Some important questions should be asked.

* How quickly does this need to be done? And in what?