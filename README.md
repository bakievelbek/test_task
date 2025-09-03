## Task 1
Most of the time simulation runs fine, but nearly once every 7-8 runs it just freezes.
Find the reason of such freezes and fix it.

---

### Fix
To stop the infinite loop, remove these lines (132–133):
```python
else:
    self.cur['raunds_left'] -= 1
```

### Explanation

The reason why it runs infinitely is that the counter
```self.cur['raunds_left']``` never reaches exactly 0.

Eventually it goes into negative values (−1, −2, …), and the script gets stuck because the 
condition of the while loop on line 64: ```while self.cur['raunds_left'] != 0``` never satisfied.   


### Why it happens only when the ```num_spins``` is close to 7 or 8?

Okay, that is all depends on how the numbers are 
distributed ```generate_initial_src()``` function. 


```handler_spin()``` function trigger ```num_spins``` times. 
When it is triggered the variable ```bonus_chance``` is 
generated out of  ```get_next_random()``` function. 

The value returned is fetched from  ```self.src``` which is ```generate_initial_src()``` function. 

It contains 100000 values. with ~20% chance of triggering a bonus per spin, 
by the time we reach 7–8 spins the probability that at 
least one bonus has triggered is quite high.


Once the bonus starts, the faulty logic in ```handler_bonus()``` may push
raunds_left into negative values - infinite loop.

## Task 1 (Optional)
Balance the game to have RTP in range 95%-96.5%.

---
First, the thing that I have noticed the balance never goes down. That is why I introduced  ```self.cur['bet']``` 
variable. I should be subtracted each time when the spin is triggered. 

Second, the thing like RTP should be regulated with payment multiplicator ```self.cur["payment_mult"]```. 
which regulates the amount of money that should be added to the player's balance.

Depending on the situation it either makes a win more than bet, equal or less.

During the user's iteration RTP should be calibrated between for example 0.5 and 1.5. 
here is a pseudocode of the calibration algorithm

1 - Compute midpoint: mid = (low + high) / 2.

2- Create pay_mult = mid.

3 - Simulate many spins → measure RTP.

4 - If this RTP is closer to the target than the previous best → update best.

5 - If RTP < target_low → set low = mid.

Else if RTP > target_high → set high = mid.

Else (RTP inside target range) → stop and return (mid, rtp).


## Task 2

Considering potential issues, solutions 1 by 1 based in the Task 1:

- in the system we have number like 20 (bonus chance) and 50 (new_symbol_chance). 
QAs must know that numbers which make it a bit complicated for them. So developers 
should consider a scenario where QAs just insert human-readable indicator in 
some config file and in the code that human-readable indicator converted into a number

- If balance in the system is changed and some constant values are changed we still consider the 
solution for the first issue because from our side that constant values should changed and 
still mapped to human-readable indicators

- Some pretest system can be created which checks out constants. for example we calculate some thresholds
yesterday is was``` value = (roll % 10) + 1```. Today it’s ```value = (roll % 12) + 1```. 
We expect the specific value, for example 7, but we are getting different. and here is fails because we changed 10 -> 12

- Very interesting issue, I didn't get that, but I think I have some 
thoughts in my mind which can be discussed in a live interview

- Provide for QAs progress manipulation tweak. So that they input numbers and 
progress manually. Very useful for the developers too, when they want to test their changes


----
### Summary

In general, it took about __1.5 hours__ to complete the whole task.  
I found a bug in __10 minutes__ and started to write the explanations and
solution which took __20 minutes__ (I also create git and pushed the code there)

about __30 minutes__ to complete the optional task and understand how to balance the game
Since it is optional and I am running out of time I just wrote a pseudocode here.
Which can be also rewritten during the interview and implemented in the code

and the rest __30 minutes__ to understand and write something for the second task

---

PS I spent about 40 minutes and played slots before starting the task. 
It allowed me to understand the task better