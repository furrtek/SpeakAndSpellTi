# SpeakAndSpellTi
Commented source code of Texas Instrument's original Speak and Spell™. Picked up from [US patent #4189779](http://www.google.com/patents/US4189779).

## Labels map
|    Label    | Page # |   Description   |
| ----------- | ------ | --------------- |
| OFF | Page 0 | Turns power off by suicide |
| ADDCARRY | Page 0	| Add A to RAM with carry propagation |
| CLEAR | Page 1 | Clear display buffer and set cursor at position 0 |
| CURLEVL | Page 5 | Init ROM address in RAM to 0 |
| NOTFULL | Page 1 | Puts keycode in 0,14/0,15 in display buffer, increments cursor pos and BL to NO$TRANS
| MEMADDR | Page 10 | Address ROM |
| OUTADDR2 | Page 7 | Get 4 bits from ROM in A |
| F-SCORE | Page 1 | Called with A=number of words tried ? |
| **START** | Page 15 | Entry point |
| DIFFSLY | Page 4 | Displays SPELL IT/SAY IT and difficulty level |
| MEMDRED | Page 10 | Dummy ROM read |
| TONES | Page 11 | Plays 4 tones (4 random sequences possible) |
| ADDCTR6 | Page 4 | BL from TONES, reads pointer and starts speaking |
| ADDWDS2 | Page 14 | Address ROM and starts speaking... |
| SPKREG | Page 14 | |
| SPKREG+1 | Page 14 | Read synth statys, branch to BITSET0 when synth is talking (talk status = 1) |
| BITSET0 | Page 14 | Goes directly to DISP/KB |
| DISP/KB | Page 15 | Reset timeout counter 2 MSBs, reset debounce counter, goes into DSP1...... |

To do...

DSP1	Page 15	Reset R-line counter, goes into DSP2... |
DSP2	Page 15	Load output PLA for character display Y, turn on corresponding R line, BL to TIMEUP |
TIMEUP	Page 0	Check for key press, if yes CAR2, if not continue to... |
TIMEUP1	Page 0	inc timeout counter (3,8), RETN if called, turns off if counter > 3FFFF else CAR1 (X=0 and BL to DISP/KB1) |
DISP/KB1	Page 15	Increment R-line counter, turn off last one, check if 8 characters done. No : BL to DSP2, Yes : turn off filament, | call to TIMEUP1 |
		Increment debounce counter, limit to F |
		If disp loop counter < 10, go to DSP1 |
		Test talk flag in (3,15), SPKREG+1 if speaking |
		DISPL+1 if bit 0 of (3,14) = 1 |
		Else DSP1 |
DISPL+1	Page 8	
KEYSEVL	Page 15	Process key press A=keycode LSB, branches to...
KEY1	Page 13	Handle keycode 1,xx
KEY00	Page 13	Handle keycode 0,xx
ROM	Page 7	Module switch ? Adds 8 to (8,6)
REPEAT	Page 10	Copy DAM ROM address to page 1 ROM address and speak
FILSLOOP	Page 15	Fills RAM with A (dec Y)
RANDOM	Page 5	
RCOMX8		Y=(8,0)
COMX8		Y=(8,Y)
SETBIT1	Page 10	Sets (8,2).1=1
ADDCTR6	Page 4	MEMADDR + LOADRESS + Speak it
