---
layout: post
published: true
title: "Sequential data & Recurrent neural network - 순차 데이터와 순환 신경망"
feature-img: "/assets/img/Charlie/article_6/RNN-thumbnail.png"
thumbnail:  "/assets/img/Charlie/article_6/RNN-thumbnail.png"
date: 2021-12-14
excerpt: "What makes RNN superior to feed-forward networks, and for what data types should they be used?"
tags: [RNN, Sequential data, neural network, deep learning, NLP, time series, timestep, hidden state, prediction, AI, ML, Artificial intelligence, machine learning, megazone, ai center]
author: charlie
---

# Sequential data & Recurrent neural network - 순차 데이터와 순환 신경망

## What is sequential data?

Any data for which an order is relevant: time series is an exemple of data for which the order (chronological order) is important. 

## What is a Recurrent Neural Network (RNN)?

Google's assistant and Apple's Siri both use RNNs.

Recurrent Neural Network (RNN) is a Deep learning algorithm that is specialized for processing sequential data. What makes RNN special is that it maintains **internal memory**. It is the reason why RNNs are so efficient at solving machine learning problems that involve sequential data. The internal memory allows for back propagation, meaning the data doesn't go a one way direction (feed-forward), but also travel back and forth between layers.

Through backpropagation, at each pass back, the weights are adjusted to obtain a more accurate model. This is how nodels learn.

By default, the memory is short term, though with the addition of LSTM they can have long term memory.

![rnn-vs-fnn](/assets/img/Charlie/article_6/rnn-vs-fnn.png)



We can take a closer look at the looping process by unrolling the network:

![timestep](/assets/img/Charlie/article_6/timestep.png)

Inputs all go through the same cell A, then passes through it again with the additional data from the new input... etc. How many of the past data points should be kept for the next loop is called a **timestep**.
→ in analysing a sentence of 5 words, set the timestep to 5 so the whole sentence is taken into account by the RNN. If you set it to 4, when analysing the fifth word, the first one will not be taken into account: "~~What~~ time is it _now_ ?"

The **hidden state** is the result output of the pass, that will go on to the next timestep. It kind of is a draft-output. When the whole sequence has passed through the cell, the last hidden state becomes the final output.

![whattimeisit](/assets/img/Charlie/article_6/whattimeisit.gif)

# Sources

-  [Sequence Models and RNN - Towards Data Science](https://towardsdatascience.com/sequence-models-and-recurrent-neural-networks-rnns-62cadeb4f1e1)

- [RNN and LSTM - Builtin](https://builtin.com/data-science/recurrent-neural-networks-and-lstm)
- [RNN Illustrated - Towards Data Science](https://towardsdatascience.com/illustrated-guide-to-recurrent-neural-networks-79e5eb8049c9)