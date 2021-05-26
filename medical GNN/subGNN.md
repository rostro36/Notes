# [Subgraph Neural Networks](https://arxiv.org/abs/2006.10538)
## Introduction
Graph-level representations tend to lose some finer details, while node-level structure tend to forget the big picture and impose an assumption that the nodes stay in the graph.

A potential middle ground are subgraphs, that may consist of many disconnected components, but they also have challenges from both representations; there is a non-trivial and potentially changing boundary and interactions between the members.

SubGNN of this paper proposes a method for subgraph representations, that tackles these problems with three channels: position, neighbourhood and structure.
## SubGNN subchannels
| SubGNN Channel | **Internal** | **Border** |
|:-:|:-:|:-:|
| **Position** | Distance between subgraph components | Distance between subgraph and rest of graph |
| **Neighbourhood** | Identity of subgraphs node | Identity of border nodes |
| **Structure** | Internal connectivity of subgraph | Border connectivity |

## Subgraph-Level Message Passing
SubGNN adds a level of aggregation on the subgraph components.

For each of the three channels, there is a randomly sampled anchor subgraph. A message is computed from the anchor subgraph to all the components comprising of a similarity function between the anchor and the subgraph component multiplied with a representation of the anchor patch.

In a first step, the messages of all anchors of a channel get aggregated to an aggregated state. This aggregated state then gets put together with a representation of the component in a one-layer MLP. There is now a representation of the component also with relation to all the anchors.

This is one layer and one can make this process many times for many more layers with more anchor patches.
## Channel specifications
For all channels there is:
- an anchor sampling function there may be many anchors
| SubGNN Channel | **Internal** | **Border** |
|:-:|:-:|:-:|
| **Position** | one node from subgraph | one node outside subgraph |
| **Neighbourhood** | one node from component | one node from component neighbourhood |
| **Structure** | Sampled triangular walks | Sampled triangular walks |
- an anchor encoder
	- since in two channels there is only one node, it is only a node embedding that can be obtained with pre-training e.g. a GIN on link prediction
	- in the last channel, triangular walks are used to get neighbourhood information
- a similarity function
	- often with an average shortest path
## Experiments
In the synthetic datasets we can see that the different channels perform according to the respective tasks; a structure task is best solved by the structure channel alone. This means, if we can directly sort the task into a category, it may make sense to not use all channels, but only the best subset of them.

SubGNN outclasses all baselines by a significant margin both on the synthetic as well as the real world datasets.