### CS 3700 Project 6: Distributed Key-Value Database

Daniel Szyc and Michelle Zinger

### High-level Approach

To begin, we had to spend quite a bit of time understanding the resources that we were given for this project.The Raft Paper, in particular, became our source of truth for this project. The paper was quite lengthy,but we recognized the importance of understanding the paper and its specifics in depth, in order to accurately be able to apply the Raft protocol to implement the Key-Value Datastore. Once we felt we had a stronger understanding of the paper, we went on to referencing the assignment itself and the 'Implementing Raft' section which had solid steps for how to begin. 

Next came understanding the functionality of the starter code, since we would be building off of it. Then, we decided to follow the recommended approach provided in the "Implementating Raft" section of the instructions, implementing the features in the suggested order. We started from Step 1, implementing the basics of responding to requests, handling the first leader election, and doing bare-bones implementations of RequestVote and AppendEntries RPCs. We found that following those outlined steps and working through the tests incrementally set a good pace for the project. From there, we continued on through the steps, continually referencing the paper for validity and running through test cases to understand our statistics and whether we were on the right track.

### Challenges We Faced

The main challenge that we faced while working through this project was definitely the logs and all of the associated variables used for tracking consensus. The raft paper had a lot missing details that we had to fill in with our intuition, which led to some confusion and many hours lost debugging. For example, we faced a lot of problems handling client requests during elections. 

Obviously, the crux of the algorithm came from implementing Step 6. After the milestone submission, we spent many, many hours working on implementing log replication and ensuring this was done accurately. This proved to be very difficult, as we struggled with log indexing and understanding quorum agreement. Another main struggle was in understanding why we responded to clients with the wrong key values sometimes and why we were losing put requests (this ended up coming down to not storing gets/puts during periods
of elections/no existing leader.) 

Another challenge we faced was setting the timeout thresholds to minimize the number of messages in between replicas but also limit the amount of time the system idled when the leader died. We eventually came to our current timeout values after some trial and error.

### Design Features

- Our Raft implmentation takes the approach of hard-coding the initial leader, which optimizes the performance of the algorithm. By doing so, we avoid having an election where all of the replica logs are empty once the system starts up. 
- We chose to utilize a dictionary to hold quorum/counts of votes for each key. We also hold a queue of unacknowledged put requests so that we can respond once we have reached a majority agreement on a put.
- The design of our code is very modular, and we focused on keeping functions relateively short and readable. This included avoiding excessive nesting wherever possible.
- We provided a lot of in depth documentation to help make the implementation easier to follow. 


### Testing 
We tested heavily using the configurations we were given. Each test had useful statistics that we relied on to guide us in our implementation. Particularly, the total get()/put() unanswered requests and get() with incorrect responses became a huge guiding point for us. 

We also relied on printing out the logs, datastores, quorum counts, queues, etc. during the more complex test cases to help us track down timing of sending the wrong keys. This helped us get an intuitive sense for how our raft implementation works, especially with adverserial circumstances.