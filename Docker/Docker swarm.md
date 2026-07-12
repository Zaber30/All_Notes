Docker Swarm is a tool built into Docker that connects multiple computers together so they act as a single system for running containers.


ocker Swarm works by dividing the computers (called **nodes**) in your cluster into two specific roles [1]: **Manager Nodes** and **Worker Nodes** 

1. The Cluster Roles

- **Manager Nodes:** These are the "brains" of the cluster . They receive your commands, decide which computers are healthy, and assign work to the other machines 
- **Worker Nodes:** These are the "musicals" of the cluster . Their only job is to receive containers from the Manager Node and run them 

text

```
               +-------------------+

               |    User Command   |
               +---------+---------+
                         |
                         v
               +-------------------+

               |   Manager Node    | <--- (Controls the cluster)
               +----+---------+----+

                    |         |
         +----------+         +----------+

         |                               |
         v                               v
+-----------------+             +-----------------+

|   Worker Node   |             |   Worker Node   | <--- (Runs the containers)
|  [Container A]  |             |  [Container B]  |
+-----------------+             +-----------------+
```

Use code with caution.