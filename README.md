# Deep Portfolio Management

### Background
This repo contains an implementation of the paper [A Deep Reinforcement Learning Framework for the Financial Portfolio Management Problem](https://arxiv.org/pdf/1706.10059.pdf). The paper describes a cryptocurrency portfolio managing agent consisting of a fully convolutional neural network trained through reinforcement learning. The policy network, called the Ensemble of Identical Independent Evaluators (EIIE), takes a 3D tensor of historical market-relative price data as input, as well as the vector of portfolio weights from the previous period. The output of the EIIE is the portfolio weight vector for the subsequent period. 

![Ensemble of Identical Independent Evaluators](https://i.imgur.com/vbebH1c.png)

The implementation is written in Python, using Keras and Tensorflow to construct the policy network and calculate its gradient.

### Framework
##### Training
The EIIE is trained at the end of each trading period on stochastic mini-batches of historical data.
Mini-batch intervals are sampled from a geometric distribution so recent data is selected more often than older data. The agent maintains a memory of the portfolio weight vector at each trading period, known as the Portfolio Vector Memory (PVM). The PVM is overwritten both when the agent is redistributing the portfolio and during training. For each mini-batch, the agent ascends the reward gradient of the interval by an amount determined by the learning rate <img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/fd8be73b54f5436a5cd2e73ba9b6bfa9.svg?invert_in_darkmode&sanitize=true" align=middle width=9.553335pt height=22.745910000000016pt/>. The reward gradient with respect to the EIIE weights <img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/1f9fba1e48d4c49a6c5797348692dae8.svg?invert_in_darkmode&sanitize=true" align=middle width=12.635370000000002pt height=22.745910000000016pt/> of a trading period <img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.9139630000000025pt height=20.14650000000001pt/> is the logarithmic rate of return for the period given by the formula, 
<img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/69638df5a3699d49b88cb55142536baa.svg?invert_in_darkmode&sanitize=true" align=middle width=158.75359500000002pt height=24.56552999999997pt/>
where <img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/3b20867687e9b5bec3d45e0fe50dc663.svg?invert_in_darkmode&sanitize=true" align=middle width=33.4356pt height=14.102549999999994pt/> is the portfolio weight vector at the start of period <img src="./svgs/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.9139630000000025pt height=20.14650000000001pt/>, <img src="./svgs/371fd45e7034625fe91e89b9280894a2.svg?invert_in_darkmode&sanitize=true" align=middle width=12.976590000000002pt height=14.102549999999994pt/> is the market-relative price vector for period <img src="./svgs/4f4f4e395762a3af4575de74c019ebb5.svg?invert_in_darkmode&sanitize=true" align=middle width=5.9139630000000025pt height=20.14650000000001pt/>, and <img src="https://raw.githubusercontent.com/EthanBraun/DeepPortfolioManagement/master/svgs/7febcd8fabfac849c47f1b52dbd43d9a.svg?invert_in_darkmode&sanitize=true" align=middle width=14.815185000000001pt height=14.102549999999994pt/> is the factor by which the portfolio decreases in value due to trading fees in redistribution for the period.
