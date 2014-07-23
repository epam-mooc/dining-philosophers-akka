Dining philosophers problem
===========================
The dining philosophers problem is a **classical** synchronization problem. Taken at face value, it is a pretty meaningless problem, but it is typical of many synchronization problems that you will see when allocating resources in operating systems.
Problem statement
-----------------
<img align="left" src="http://docs.oracle.com/cd/E19205-01/820-0619/images/figure2.gif" />
Five silent philosophers sit at a round table with bowls of rice. Between each adjacent pair of philosophers is a chopstick. Each philosopher must alternately think and eat. He thinks for a while, and then stops thinking and becomes hungry. When the philosopher becomes hungry, he cannot eat until he owns the chopsticks to his left and right. Each chopstick can be held by only one philosopher and so a philosopher can use the chopstick only if it's not being used by another philosopher. After he finishes eating, he needs to put down both chopsticks so they become available to others. A philosopher can grab the chopsticks to his right or the one to his left as they become available, but can't start eating before getting both of them.
Eating is not limited by the amount of rice left: assume an infinite supply.<br /><br />
The problem is how to design a discipline of behavior (a concurrent algorithm) such that each philosopher won't starve; i.e., can forever continue to alternate between eating and thinking assuming that any philosopher cannot know when others may want to eat or think.<br /><br />
Since the chopsticks are shared between the philosophers, access to the chopsticks must be protected, but if care isn't taken, concurrency problems arise. Most notably, **starvation** (pun intended) via **deadlock** can occur: if each philosopher picks up a chopstick as soon as it is available and waits indefinitely for the other, eventually they will all end up holding only one chopstick with no chance of acquiring another. <br/><br/>
To see that designing a proper solution to this problem isn't obvious, consider the following proposal: instruct each philosopher to behave as follows:

* think until the left fork is available; when it is, pick it up;
* think until the right fork is available; when it is, pick it up;
* when both forks are held, eat for a fixed amount of time;
* then, put the right fork down;
* then, put the left fork down;
* repeat from the beginning.

This attempt at a solution ***fails***: it allows the system to reach a deadlock state, in which no progress is possible.
Solutions
---------
There are a few solutions of this situation:

1. Resource hierarchy solution
2. Arbitrator solution
3. Chandy/Misra solution

Arbitrator solution
-------------------
Here you can see implementation of second method: **arbitrator solution** via **actors**. This approach is to guarantee that a philosopher can only pick up both chopsticks or none by introducing an arbitrator, e.g., a waiter. In order to pick up the chopsticks, a philosopher must ask permission of the waiter. The waiter gives permission to only one philosopher at a time until he has picked up both his forks. Putting down a fork is always allowed. The waiter can be implemented as a ***mutex***. In addition to introducing a new central entity (the waiter), this approach can result in reduced parallelism: if a philosopher is eating and one of his neighbors is requesting the forks, all other philosophers must wait until this request has been fulfilled even if forks for them are still available.
