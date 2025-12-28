**Split-Brain Scenario**

**Summary**:
Happens when two nodes lose communication with each other (a network partition), but each side continues
acting as if it's the only correct one. Effectively creating two brains making independent decisions.
This scenario is worse than downtime, because downtime is honest and split-brain lies.

**Why is it Dangerous:**

- data diverges (conflicting writes)
- two leaders get elected
- invariants break (double charges, double bookings)

**How to Prevent it:**
All defenses are variants of "who's allowed to act?"

1. Majority voting: only act if you can reach >50% votes, minority must stop
2. Fencing/Leases: leader gets a time-bound right for writes
3. External arbitral: a third party app decides who's the leader
4. Faill-stop, not fail-open: if unsure, shut up
