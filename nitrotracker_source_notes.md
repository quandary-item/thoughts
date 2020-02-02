reading the nitrotracker source code
====================================

today i will read NITROTRACKER - one of my favourite pieces of software ever 

this is the first existing C/C++ project I've tried to look at in a systemic way. i want to analyse the structure of this one in order to figure out:
- how to lay out the structure of C/C++ programs
- what patterns/anti-patterns people use in such programs
- what i like about C/C++, what i don't like

i will do this in a breadth-first manner, beginning at the top/abstract and descending to the bottom/concrete as far as i can before i get tired or bored. please do not take this article as some sort of authoritative reference - i have never written a single program for the nintendo ds. have you ever been on a tour around a city by somebody who just moved there? it's like that.

# level 0

from the website, i quote: "NitroTracker is a FastTracker II style tracker for the Nintendo DS". its source code is available from: https://code.google.com/archive/p/nitrotracker/ like most free software, it is an abandoned/dead project (the last commit was in 2011) which is a shame but is somewhat advantageous to me - it means that this article will probably never go out of date.

# level 1

like all good dead projects, nitrotracker uses the SVN blame-tracking tool. i do not know how to use SVN so i downloaded the .zip file (https://storage.googleapis.com/google-code-archive-source/v2/code.google.com/nitrotracker/source-archive.zip) and extracted to the `~/Downloads/nitrotracker` folder on my laptop. for the purpose of this article, i will be looking at the `trunk` folder.

its structure is as follows:

```
.
├── arm7 - folder
├── arm9 - folder
├── gpl-3.0.txt
├── gpl_header.txt
├── icon.bmp
├── logo.bmp
├── Makefile
├── ndsloader.bin
├── README.TXT
├── .svn - folder
└── tobkit - folder
```

# level 2

the first thing i want to know when reading the source code is what these files and folders are for, at a high level.

`.svn` - this is the subversion/SVN "administrative directory". it contains information that is used by subversion to track changes to the directory. since i am not interested in how version control is applied to this project it can be safely ignored for now.

`README.txt` - this contains human-readable information about the project

`gpl-3.0.txt` - this contains the licence agreement for this project - GPL v3. this is one of my favourite licences because it allows you to do whatever the hell you want with the code, provided that you let whoever you share the code with do the same. companies hate it and usually this makes it harder to 'commercialise' or 'lock down' GPL'd projects. perhaps this led to nitrotracker's demise. can you imagine EDM musicians going up on stage and using "Ableton NitroTracker"?

`gpl_header.txt` - a shortened form of the licence to be pasted at the top of every source file in the project

`icon.bmp` and `logo.bmp` - i *think* these are used by the nintendo DS operating system when it displays the program name on the home screen. hard to say for sure. i broke my nintendo ds years ago and never bought a new one because it was after i had renounced playing video games in order to focus on my education and more wholesome pursuits, a decision i only partly regret

`ndsloader.bin` - some executable file? i think this is some sort of bootloader/firmware thing?

`arm7`, `arm9` and `tobkit` - the source code of the program itself, more or less

`Makefile` - instructions for building the source code, turning it into a thing the nintendo ds can run

some of the above files (`.svn`, `README.txt`, `gpl-3.0.txt`, `gpl_header.txt`, `icon.bmp` and `logo.bmp`) are not really specific to C/C++ programming so i will not explore them in any more detail.

# level 3

the source code of nitrotracker is split between three directories:

`arm7` - code to be run by the ARM7 processor

`arm9` - code to be run by the ARM9 processor

`tobkit` - a graphical user interface toolkit, essentially a bunch of widgets and GUI things. things you click on and drag around and so on.

huh? why are there two processors? it turns out that if you want to make a games console backwards-compatible with a previous games console, the easiest way to do this is to just put the old games console inside the new games console, like some sort of twisted matryoshka doll. what this means is that when you insert a GBA cartridge into your DS, the ARM7 processor inside the DS runs the old GBA code. of course we don't want this extra processing power to go to waste, so when running a nintendo DS game you can make use of the ARM7 (old) and the ARM9 (new) processors. ain't that neat

# level 4

at this level we might see some actual code.

when i read someone else's code i like to know where it starts, i.e. what is the "entry point" of the program?

to know this it is worth understanding how compilation works (but only as a high level - we're still only on level 4). in short, compilation in C/C++ (??) works by converting a load of source files (.c and .cpp) into object files (.o) - these consist of the actual machine-runnable binary code. these are then linked together, i.e. turned into one big glob of machine code that can be run. in this resulting mess there is ONE function with the name `main`. this is the entry point.

oh wait no there are two entry points. one for arm7 and one for arm9. one is in `arm7/source/arm7_main.cpp` and the other is in `arm9/source/main.cpp`. why is this? why not just one entry point? i'm not sure. you'll notice that the arm9 source file is much much longer than the arm7 one. in fact, the arm7 source directory only contains this one single source file and its own `Makefile`. as far as i know, in nintendo ds programs the arm9 processor is 'in charge' of the arm7 processor. certain responsibilities are delegated by the arm9 processor to the arm7.

# level 5

coming next: what are these entry points doing? what are arm7 and arm9 for? how is this project built? i'll update this document or create a new one as i go, but it's time to have lunch.
