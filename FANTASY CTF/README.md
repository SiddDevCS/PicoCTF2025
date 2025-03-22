# PicoCTF 2025 - FANTASY CTF Writeup

## Challenge Overview

**FANTASY CTF** is a beginner-level challenge in picoCTF designed to help players familiarize themselves with basic terminal interactions and follow important rules set by the competition organizers. The simulation involves navigating through a set of options that mimic actions in a typical picoCTF challenge.

## Challenge Steps

1. **Connect to the Simulation:**
   Use the following command to connect to the challenge:
   ```
   $ nc verbal-sleep.picoctf.net 63159
   ```

2. **Start the Simulation:**
   Once connected, the simulation will introduce you to Eibhilin, a student navigating through a series of choices. These choices represent various actions that players must take in line with picoCTF's rules.

3. **Make Choices:**
   The challenge will present several options, where you'll need to choose the correct option based on picoCTF's guidelines:
   
   - **First Decision:**
     ```
     Options:
     A) *Register multiple accounts*
     B) *Share an account with a friend*
     C) *Register a single, private account*
     [a/b/c] > c
     ```
     Choose **C** to follow the correct practice of registering a single, private account.

   - **Second Decision:**
     ```
     Options:
     A) *Play the game*
     B) *Search the Ether for the flag*
     [a/b] > a
     ```
     Choose **A** to play the game and progress in the simulation.

4. **Complete the Game:**
   After making the correct choices, you’ll be presented with the flag:
   ```
   "Thanks, Nyx! Here's the flag I found: picoCTF{m1113n1um_3d1710n_76b680a5}"
   ```

5. **End of Simulation:**
   Once the flag is revealed, the simulation ends, and you'll be reminded of the rules:
   - Register only one account.
   - Do not share accounts, flags, or artifact downloads.
   - Wait to publish writeups until after the competition ends.

6. **Submit the Flag:**
   The flag revealed is **picoCTF{m1113n1um_3d1710n_76b680a5}**, which you should submit to earn points in picoCTF 2025.

## Conclusion

By following the instructions and making the correct choices during the simulation, you will obtain the flag. This challenge is designed to reinforce important rules of picoCTF, such as not sharing accounts or flags and waiting until after the competition to publish writeups.