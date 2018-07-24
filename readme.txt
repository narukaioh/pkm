****************************************************************************************************
seedminer (v2.1) - zoogie **************************************************************************
****************************************************************************************************

Prerequisites:
- A 3DS with firmware 11.4.0-X - 11.7.0-X (on 11.8.0-X and above, seedminer is likely to be patched)
- PC with windows and python 2.X or 3.X
- A DSiware Transfer compatible dsiware game. 
https://3ds.hacks.guide/installing-boot9strap-(dsiware-game-injection-list)
Choose one from your region. EA Sudoku is my recommendation for the US/EUR regions.
- For LFCS dump, ONE of the following: 
1. Userland entrypoint
2. A friend, online or irl, with a userland entrypoint or cfw
3. A PC with dedicated GPU.

Contents:
seedminer_launcher.py (python 2 script - user commandline interface - the next two files are the same and interchangable with this one)
seedminer_launcher3.py (python 3 script - * )
seedminer_launcher.exe (windows executable - * )
seedminer.exe (windows executable - for CPU brute force)
bfcl.exe (OpenCL windows executable - for GPU brute force)
cl/ (bfcl.exe support files)
save/lfcs.dat (old3ds msed_data database)
save/lfcs_new.dat (new3ds msed_data database)
whatever.dll (windows support files, ick)
seedstarter.3dsx (3ds program to fetch LFCS, msed_data, and ctcert)
seedstarter.cia (cfw only - 3ds program to fetch LFCS, msed_data, and ctcert)

Summary:
This procedure has 3 stages.
You only have to chose one letter in each of the first two stages, DONT do everything.
1. LFCS/ID0: A B or C
2. Brute Force: A or B
3. DSIHaxInjector

****************************************************************************************************
****LFCS/ID0 (choose ONE, A B or C)*****************************************************************
****************************************************************************************************
----------------------------------------------------------------------------------------------------
[A - Homebrew]- easiest

1) If you have a userland entrypoint (a way to run a .3dsx) run seedstarter.3dsx on your console, press A, and it should dump "movable_part1.sed" in the sdmc:/seedstarter folder.
2) Place movable_part1.sed in the seedminer folder and move on to the brute-forcing stage.
----------------------------------------------------------------------------------------------------
[B - No homebrew but friend with homebrew] - harder

1) If you know someone online or irl that has a hacked 3ds (even just userland) they can help you get LFCS. First, exchange friend
codes with them. Then they run seedstarter.3dsx and press B. There should be a file dumped in sdmc:/seedstarter/LFCS that has your
friend code in the filename (ex. 3_1235-5678-1234_part1.sed). 
NOTE: If your friend only has userland (non-cfw) homebrew, they might have to use app takeover to get the required frd:u service, as *hax doesn't have it.
To takeover an app (almost any game will do), create seedstarter.xml in same directory as the .3dsx with <targets selectable="true"></targets> inside.
2) They send you this file and you rename it to movable_part1.sed and put it inside your Nintendo 3DS folder on your system's sd card.
NOTE: There has been some concern expressed that someone could steal your LFCS and unban their console with it. This is an unfounded
fear -- LFCS ≠ LocalFriendCodeSeed_B. The LFCS needs its signature for it to be useful unbanning someone's console. Just the LFCS
alone is useless, and the 3ds friendslist save does not contain any signatures whatsoever.
3) Take the seedminer_launcher(3).py script and run it inside the sdmc:/Nintendo 3DS/ folder (with argument ID0) and it should add the ID0 to your movable_part1.sed. You may optionally add an ID0 with command line: python seedminer_launcher.py id0 <32 digit hex number>
4) Place the movable_part1.sed file in the seedminer folder and move on to the brute forcing stage.
----------------------------------------------------------------------------------------------------
[C - No homebrew and no friends with homebrew] - hardest
 
Currently for dedicated GPUs only. Needs Pycryptodomex installed. (pip install pycryptodomex)
0) (Optional) - if you wish to brute force your mii qr and movable.sed in one command, do step 3 in part B and place the 
movable_part1.sed in the seedminer folder. This file will automatically be generated with the id0 command if it doesn't exist.
1) Find a mii in mii maker that was created on YOUR system. That's very important. Make one from scratch if you're not sure where it came from.
2) Export the mii to a QR code. They're found in the "sdmc:/DCIM/100NIN03/" and named something like "HNI_1234.JPG".
3) Upload it to https://3ds.goombi.fr/editMii/ then "Import from -> QR code"
4) "Export to -> encrypted .bin"
5) Take resulting input.bin, place it in the seedminer/ directory, and run:
python seedminer_launcher.py mii new|old [year of 3ds manufacture]
NOTE: If you have a new3ds, you *might* have to choose "mii old". If you sys-transferred from an old3ds, and have never system
formatted, you will need to select old. Although an incorrect guess will cause the brute force to fail, you can try the other 
option next time. 
NOTE2: You can optionally select the year you guess the 3ds was manufactured. This is done in hopes of reducing the brute-force time.
If you have no clue, just don't enter a year. The brute force will then start in the midpoint of the possible LFCS range.
A wrong year guess will not fail the brute force. 
(old3ds 2011-2017, new3ds 2014-2017 inclusive are accepted year options)
7) Wait until movable_part1.sed dumps then move on to brute forcing movable.sed (if you did the optional step 0, the next brute force 
should start automatically and you can proceed to DSIHaxInjector stage when everythin is done).


****************************************************************************************************
**** Brute-Force (chose ONE, A or B) *********************************************
****************************************************************************************************
----------------------------------------------------------------------------------------------------
[A - GPU] - faster

Strongly recommended, but only if you have a dedicated graphics card in your PC. Integrated graphics (ex. Intel HD Graphics 530 etc.) will result in 
a very sluggish and unresponsive system. It will probably still be faster than the CPU but ... just don't do it. Use CPU.
1) Simply run "python seedminer_launcher.py gpu" and the brute force should start. The movable.sed will be dumped when it's done. Avg time should be
0-2 hours with 6 hours worst case. Depends a lot on how fast your GPU is, of course.
2) Proceed to DSIHaxInjector stage.
----------------------------------------------------------------------------------------------------
[B - CPU] - slower

1) Run "python seedminer_launcher.py cpu". You can optionally edit the number of CPU processes on the script to use first.  4 processes is good 
for 2x cores and 8 for 4x cores, etc. Don't change # of computers unless you know what you are doing (experimental feature). The default is 4 processes
and 1 computer. If you don't know what any of this means just launch the script.
2) Wait until the seedminer.exe process windows close and movable.sed is dumped. This could take 0-2 days on average but
should get better as seedminer matures (keep those msed_data coming!). Progress is saved if you have to quit. Changing the
# of processes mid brute force will mess up the saves and probably everything else. Also, don't close any windows as they all are
contributing to the brute force and even one closed window could spoil your chances.
3) Proceed to DSIHaxInjector stage.

**************************************************************************************************
**** DSIHaxInjector ******************************************************************************
**************************************************************************************************
1) Buy a game from here https://3ds.hacks.guide/installing-boot9strap-(dsiware-game-injection-list) 
(EA Sudoku is my strong rec. for US/EU)
2) Go to System Settings -> Data Management -> DSiWare -> System Memory Tab - Export it to sd card.
3) Find your exported game at sdmc:/Nintendo 3DS/<long hex number>/<another long hex number>/Nintendo DSiWare/<here, a .bin file>
4) Upload that file (4b344445.bin for EXAMPLE) AND your movable.sed to this site:
https://jenkins.nelthorya.net/job/DSIHaxInjector/build?delay=0sec
5) Press Build (your username can be anything)
6) Click the 4B344445.bin.patched_YourWeirdName, download and rename it to 4B344445.bin (this filename is an EXAMPLE).
7) Return the .bin file to sdmc:/Nintendo 3DS/<long hex number>/<another long hex number>/Nintendo DSiWare/<here>
8) Go to Go to System Settings -> Data Management -> DSiWare -> SD Card Tab - Import it to System Memory.
9) Download https://github.com/zoogie/b9sTool/releases/tag/3.0.0 and put the file at sdmc:/boot.nds
10)Download https://github.com/AuroraWright/Luma3DS/releases/tag/v9.0 and put the file at sdmc:/boot.firm
11)Download https://github.com/fincs/new-hbmenu/releases and put the file at sdmc:/boot.3dsx
12)Click https://3ds.hacks.guide/installing-boot9strap-(dsiware-game-injection)#section-vi---flashing-the-target-3dss-firm
and continue on from there.