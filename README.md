This is a simple project that attempts to train deep RL agents - particularly DQN, A2C and PPO - on the simple math game Bulls and Cows. There are two models: one that follows
the rules of the game exactly, and an alternative that has alternative rules.

Original model:
1. A four-digit sequence x with each digit distinct, e.g. 1234, is randomly generated.
2. For every turn, the player guesses a four-digit sequence with each digit distinct, e.g. 9824.
3. The player is then provided with the number of matches: Bulls for correct digits in the right position, and Cows for correct digits in the wrong position. For simplicity, we
   will use A and B in place of Bulls and Cows respectively - inspired by the games’ alternative name 1A2B.
4. This repeats until the player guesses the correct four-digit sequence.

State Space, Sk:
Sk is an m by 2 grid: m is the maximum number of guesses, the first column represents the 4-digit sequence guess and the second column represents the number of ’A’ and ’B’. 
S0 starts off as a grid with every element denoted by 0 (empty guesses). At each step k (or the kth guess), the kth row is filled.

Action Space, Ak:
Guess a 4 digit sequence, with each digit distinct. The total possible number of guesses is |A| = 5040.

Reward, Rk:
Rk = 1 if the kth row is (x, 4A0B); this is a terminal state. Rm = −1 if the mth row is filled and is not (x, 4A0B), this is a terminal state. Otherwise, Rk = 0.

I trained DRL agents on the following cases: [12,21] (gym_env_1221) and [12,13,21,23,31,32] (gym_env_reduced)


Alternative model: 
In this version, the digits that are assigned ’A’ and ’B’ are made known. Additionally, the game would return ’N’ to any digit that does not exist in the four-digit sequence. 

State Space, Sk:
Sk is an array of size 11: each index represents each digit 0 − 9, and the final index represents the remaining number of guesses left (initialized as m). S0 starts off as an 
array with every element denoted by 0 (unexplored). At each step k, the value of each digit of the guess is updated to: 1 if the digit has been explored and has ever attained 
’N’, 2 if the digit has been explored but has ever attained ’B’, 3 if the digit has been explored and has ever attained ’A’. 

Action Space, Ak:
Guess a 4 digit sequence, with each digit distinct. The total possible number of guesses is |A| = 5040.

Reward, Rk:
Rk = 1 if at the kth guess, the correct digits all have a value of 3, no matter the values of other digits; this is a terminal state. Rm = −1 if this is not achieved before the 
number of remaining guesses reaches 0, this is a terminal state. Otherwise, Rk = 0.

I trained DRL agents on the following cases: Permutation of 2-digit sequence without replacement from digits 0 − 3 (gym_env_alternative_reduced), 
Permutation of 4-digit sequence without replacement from digits 0 − 4 (gym_env_alternative)
