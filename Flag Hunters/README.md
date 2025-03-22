# Flag Hunters - PicoCTF Writeup

## Challenge Overview

**Flag Hunters** is a 75-point challenge from PicoCTF. The task requires you to interact with a program that prints lyrics to a song. However, there's a hidden refrain and a flag embedded in the program, but it is not printed by default. The challenge asks you to manipulate the flow of the program to reveal the hidden flag.

### Objective
You need to get the program to print a hidden refrain and find the flag. The flag is embedded in the song's intro, but due to the program's flow, it isn't printed directly.

### Program Behavior

The program consists of lyrics and special commands that control the flow of the song:

- **RETURN n**: This command moves the program to line `n`.
- **REFRAIN**: This command causes the program to jump to the refrain.
- **CROWD**: This command prompts the user for input, which gets incorporated into the lyrics as "Crowd: <input>".

### Challenge Insights

1. The song is divided into multiple parts (verses, refrain, etc.), and each part is separated by special commands like **RETURN** and **REFRAIN**.
2. The flag is hidden in the **secret_intro**, which is formed at the start of the song.
3. By interacting with the **CROWD** input section, you can modify the song's flow and make the program jump to different parts of the lyrics, including the hidden flag.

### Exploiting the Program

When the program asks for **Crowd** input, you need to input the following:

```
Crowd: ;RETURN 0
```

This causes the program to jump back to line 0, which prints the `secret_intro` (and the flag) that is otherwise not shown.

### Flag Extraction Process

1. When the program prompts you for **Crowd**, input `;RETURN 0`.
2. This causes the program to jump back to the start and print the hidden flag in the `secret_intro`.

### Example Interaction

```bash
$ nc verbal-sleep.picoctf.net 56688
Command line wizards, we’re starting it right,
Spawning shells in the terminal, hacking all night.
Scripts and searches, grep through the void,
Every keystroke, we're a cypher's envoy.
Brute force the lock or craft that regex,
Flag on the horizon, what challenge is next?

We’re flag hunters in the ether, lighting up the grid,
No puzzle too dark, no challenge too hid.
With every exploit we trigger, every byte we decrypt,
We’re chasing that victory, and we’ll never quit.
Crowd: ;RETURN 0

Echoes in memory, packets in trace,
Digging through the remnants to uncover with haste.
Hex and headers, carving out clues,
Resurrect the hidden, it's forensics we choose.
Disk dumps and packet dumps, follow the trail,
Buried deep in the noise, but we will prevail.

We’re flag hunters in the ether, lighting up the grid,
No puzzle too dark, no challenge too hid.
With every exploit we trigger, every byte we decrypt,
We’re chasing that victory, and we’ll never quit.
Crowd: 
Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, picoCTF{70637h3r_f0r3v3r_75053bc3}
```

### Flag

The flag is:

```
picoCTF{70637h3r_f0r3v3r_75053bc3}
```

### Conclusion

By using the **CROWD** input section to force a **RETURN 0** command, you can navigate back to the beginning of the song and print the hidden flag. This clever manipulation of the program flow reveals the flag, completing the challenge.