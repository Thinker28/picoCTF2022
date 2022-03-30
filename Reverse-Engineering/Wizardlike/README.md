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

We decompile the ELF Binary given to us, using ghidra. Then we find a related function that checks for coordinate bounds, we can change the function in the binary and run it with the new patched binary to get the flag hidden with ascii art.

## Flag

```
picoCTF{ur_4_w1z4rd_4844AD6F}
```

## Detailed Solution

We can start off like usual, with decompiling with Ghidra then debugging with gdb. Since this is a stripped ELF Binary, we have to be a bit more careful since there will be no original function   names. After opening it up in Ghidra, we can start by going through some functions in the Symbol Tree.<p align="center">
![logo](https://github.com/Thinker28/picoCTF2021/blob/main/Reverse-Engineering/Wizardlike/Screen%20Shot%202022-03-29%20at%207.12.50%20PM.png "Raspberry pi")
</p>After going through a few, FUN_001015ac seems interesting.

```c
undefined8 FUN_001015ac(int param_1,int param_2)

{
  undefined8 uVar1;
  
  if ((((param_1 < 100) && (param_2 < 100)) && (-1 < param_1)) && (-1 < param_2)) {
    if (((&DAT_0011fea0)[(long)param_2 * 100 + (long)param_1] == '#') ||
       ((&DAT_0011fea0)[(long)param_2 * 100 + (long)param_1] == ' ')) {
      uVar1 = 0;
    }
    else {
      uVar1 = 1;
    }
  }
  else {
    uVar1 = 0;
  }
  return uVar1;
}
```

It seems to be checking if that (row, col) is valid. Checking, by checking if it's a wall and if it's in bounds. We can now assume, that we need to edit the return value to true always, to be able to go through walls and get through parts we normally couldn't go through. To specifically edit this function point, I used a hex editor. We also have to keep note that ghidra starts counting from address 0x00100000.<p align="center">![logo](https://github.com/Thinker28/picoCTF2022/blob/main/Reverse-Engineering/Wizardlike/Screen%20Shot%202022-03-30%20at%206.49.40%20PM.png)</p>We now see that the value of uVar1 is getting set with the bit after b8, so we need to edit that specific bit at that specific address in the hex editor.<p align="center">![logo](https://github.com/Thinker28/picoCTF2022/blob/main/Reverse-Engineering/Wizardlike/Screen%20Shot%202022-03-30%20at%206.54.05%20PM.png)</p> After we edit both the bits to 01, after B8 we can export our new patched ELF Binary and go through all the levels to get the flag. 
