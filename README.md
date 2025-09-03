## Task
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