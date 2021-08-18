+++
title = "Jamie McQuire Research"
+++

## About Me

Computer Science PhD Researcher at [Newcastle University](https://www.ncl.ac.uk/bigdata/people/people/mcquirejamie.html) investigating the design and implementation of decentralized analytic systems for wearable healthcare applications.


## Research Summary

My research investigates the design and implementation of next generation sensors and analytics systems for healthcare IoT technologies. I am investigating how [federated learning](https://arxiv.org/pdf/1602.05629.pdf) [1] systems can 
be designed to run at-scale with inexpensive, resource-constrained devices for the analysis of wearable IoT devices. The benefits to using federated learning as opposed to traditional centralized machine learning (where the data is simply aggregated on the cloud) is the inherent privacy guarantees (data doesn't leave the data-owners local network) and communication efficiency (data is not transmitted to a central cloud-storage resource - only model updates/gradients) improvments.

At the lowest-level of my system I am developing custom wearable devices that collect data from a multitude of different sources to help develop context-aware understandings of human-gait in the free-living environment.
The analysis of the collected gait data is envisioned to help identify biomarkers in patients with neurodegenerative conditions. These wearables use a range of different sensors that measure readings, such as, acceleration, location, and atmospheric pressure, to help understand not only human movement, but also the surrounding environment, to help further understand the subtelities in the recorded signals. Part of my research will explore the benefits to running machine learning and signal processing algorithms directly at the source of data collection, potentially improving communication efficiency and making use of all available computational resources. 


## Research Interests

- Federated Learning
- Scalable Computing
- Embedded Program
- Edge Computing
- Signal Processing
- Gait Analytics


## References

1. McMahan, B., Moore, E., Ramage, D., Hampson, S. and y Arcas, B.A., 2017, April. Communication-efficient learning of deep networks from decentralized data. In Artificial intelligence and statistics (pp. 1273-1282). PMLR.
