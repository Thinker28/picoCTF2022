# picoCTF2022: Wizardlike

![Contest Date: 15.03.2022](https://img.shields.io/badge/Contest%20Date-15.03.2022-lightgrey.svg)
![Solve Moment: During The Contest](https://img.shields.io/badge/Solve%20Moment-During%20The%20Contest-brightgreen.svg)
![Score: 500](https://img.shields.io/badge/Score-500-brightgreen.svg)

## Description

> Do you seek your destiny in these deplorable dungeons? If so, you may want to look elsewhere. Many have gone before you and honestly, they've cleared out the place of all monsters, ne'erdowells, bandits and every other sort of evil foe. The dungeons themselves have seen better days too. There's a lot of missing floors and key passages blocked off. You'd have to be a real wizard to make any progress in this sorry excuse for a dungeon!
Download the game.
'w', 'a', 's', 'd' moves your character and 'Q' quits. You'll need to improvise some wizardly abilities to find the flag in this dungeon crawl. '.' is floor, '#' are walls, '<' are stairs up to previous level, and '>' are stairs down to next level.


## Attached Files

- game

## Summary

We decompile the ELF Binary given to us, using ghidra. Then we find a related function that checks for coordinate bounds, we can change the function in the binary and run it with the new patched binary.

## Flag

```

```

## Detailed Solution

Lorem ipsum
