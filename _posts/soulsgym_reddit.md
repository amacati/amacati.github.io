---
title: 'SoulsGym - Beating Dark Souls III Bosses with Deep Reinforcement Learning'
date: 2023-05-01
permalink: /posts/2023/05/blog-post-1/
tags:
  - SoulsGym
  - RL
---

SoulsGym
========
I've been working on a new gym environment for quite a while, and I think it's finally at a point where I can share it. SoulsGym is an OpenAI gym extension for Dark Souls III. It allows you to train reinforcement learning agents on the bosses in the game. The Souls games are widely known in the video game community for being notoriously hard.

What is included?
~~~~~~~~~~~~~~~~~
SoulsGym
--------

There are really two parts to this project. The first one is SoulsGym, an OpenAI gym extension. It is compatible with the newest API changes after gym has transitioned to the Farama foundation. SoulsGym is essentially a game hacking layer that turns Dark Souls III into a gym environment that can be controlled with Python. However, you still need to own the game on Steam and run it before starting the gym. A detailed description on how to set everything up can be found in the package documentation.

Warning: If you want to try this gym, be sure that you have read the documentation and understood everything. If not handled properly, you can get banned from multiplayer.

Below, you can find a video of an agent training in the game. The game runs on 3x speed to accelerate training. You can also watch the video on YouTube.

RL agent learning to defeat the first boss in Dark Souls III.
At this point, only the first boss in Dark Souls III is implemented as an environment. Nevertheless, SoulsGym can easily be extended to include other bosses in the game. Due to their similarity, it shouldn't be too hard to even extend the package to Elden Ring as well. If there is any interest in this in the ML/DS community, I'd be happy to give the other ones a shot ;)

SoulsAI
-------

The second part is SoulsAI, a distributed deep reinforcement learning framework that I wrote to train on multiple clients simultaneously. You should be able to use it for other gym environments as well, but it was primarily designed for my rather special use case. SoulsAI enables live-monitoring of the current training setup via a webserver, is resilient to client disconnects and crashes, and contains all my training scripts. While this sounds a bit hacky, it's actually quite readable. You can find a complete documentation that goes into how everything works here.

Being fault tolerant is necessary since the simulator at the heart of SoulsGym is a game that does not expose any APIs and has to be hacked instead. Crashes and other instabilities are rare, but can happen when training over several days. At this moment, SoulsAI implements ApeX style DQN and PPO, but since PPO is synchronous, it is less robust to client crashes etc. Both implementations use Redis as communication backend to send training samples from worker clients to a centralized training server, and to broadcast model updates from the server to all clients. For DQN, SoulsAI is completely asynchronous, so that clients never have to stop playing in order to perform updates or send samples.

Live monitoring of an ongoing training process in SoulsAI.
Note: I have not implemented more advanced training algorithms such as Rainbow etc., so it's very likely that one can achieve faster convergence with better performance. Furthermore, hyperparameter tuning is extremely challenging since training runs can easily take days across multiple machines.

Does this actually work?
~~~~~~~~~~~~~~~~~~~~~~~~
Yes, it does! It took me some time, but I was able to train an agent with Duelling Double Deep Q-Learning that has a win rate of about 45% within a few days of training. In this video you can see the trained agent playing against Iudex Gundry. You can also watch the video on YouTube.

RL bot vs Dark Souls III boss.
I'm also working on a visualisation that shows the agent's policy networks reacting to the current game input. You can see a preview without the game simultaneously running here. Credit for the idea of visualisation goes to Marijn van Vliet.

Duelling Double Q-Learning networks reacting to changes in the game observations.
If you really want to dive deep into the hyperparameters that I used or load the trained policies on your machine, you can find the final checkpoints [here](https://drive.google.com/drive/folders/1cAK1TbY4e4HE4cxyAFEHRpj6MOgp5Zxe). The hyperparameters are contained in the config.json file.

... But why?
~~~~~~~~~~~~
Because it is a ton of fun! Training to defeat a boss in a computer game does not advance the state of the art in RL, sure. So why do it? Well, because we can! And because maybe it excites others about ML/RL/DL.

Disclaimer: Online multiplayer
------------------------------

This project is in no way oriented towards creating multiplayer bots. It would take you ages of development and training time to learn a multiplayer AI starting from my package, so just don't even try. I also do not take any precautions against cheat detections, so if you use this package while being online, you'd probably be banned within a few hours.

Final comments
~~~~~~~~~~~~~~
As you might guess, this project went through many iterations and it took a lot of effort to get it "right". I'm kind of proud to have achieved it in the end, and am happy to explain more about how things work if anyone is interested. There is a lot that I haven't covered in this post (it's really just the surface), but you can find more in the docs I linked or by writing me a pm. Also, I really have no idea how many people in ML are also active in the gaming community, but if you are a Souls fan and you want to contribute by adding other Souls games or bosses, feel free to reach out to me.

