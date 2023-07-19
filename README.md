### Distributed Key-Value Database

## Project Description

This project implements a distributed key/value datastore based on the Stanford University research paper 'In Search of an Understandable Consensus Algorithm.' The system is intended to achieve CAP theorem, by working to provide strong consistency to client requests and handle network partitions and failures with accuracy.

## Design Features

- This Raft implmentation takes the approach of hard-coding the initial leader, which optimizes the performance of the algorithm. By doing so, we avoid having an election where all of the replica logs are empty once the system starts up. 
- It utilizes a dictionary to hold quorum/counts of votes for each key. It also holds a queue of unacknowledged put requests so that we can respond once we have reached a majority agreement on a put.
- The design of the code is very modular, and there was emphasis on keeping functions relateively short and readable. This included avoiding excessive nesting wherever possible.


### Testing 
This system was tested heavily using the configuration files. Each test had useful statistics that were relied on to guide in the implementation and catch defects. 
