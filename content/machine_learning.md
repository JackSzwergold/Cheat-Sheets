---
Title: Machine Learning
Description: A cheat sheet for machine learning related items.
Author: Jack Szwergold
Date: 2017-11-06
Robots: noindex,nofollow
Template: index
---

### Torch

 - `http://torch.ch/docs/getting-started.html`

<!-- -->

    git clone https://github.com/torch/distro.git ~/torch --recursive
    cd ~/torch;
    bash install-deps;
    ./install.sh

***

### char-rnn

 - `https://github.com/karpathy/char-rnn`

<!-- -->

    luarocks install nngraph
    luarocks install optim
    luarocks install nn

    th train.lua -gpuid -1
