<div align="center">

## Matrix Graphic


</div>

### Description

This code is made to mimic the graphic effect seen on the move "The Matrix".

I was wondering if anyone could tell me if they knew of a way which I could display the text in 256 or more colors while still writing to memory. Using the old 16 colors doesn't allow for a very interesting effect.
 
### More Info
 
Only that you can use pointers to write to memory instead using cprintf();.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Justin P](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/justin-p.md)
**Level**          |Intermediate
**User Rating**    |4.0 (8 globes from 2 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Graphics/ Sound](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics-sound__3-15.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/justin-p-matrix-graphic__3-5714/archive/master.zip)

### API Declarations

stdio.h, stdlib.h, conio.h, string.h, and dos.h


### Source Code

```
/* "Matrix Program" developed to mimic the effect seen on the motion picture, "The Matrix"
 Justin Potter 01/31/03 */
#include <dos.h>
#include <string.h>
#include <conio.h>
#include <stdlib.h>
#include <stdio.h>
#define MAXY 50  /* Screen height       */
#define MAXX 80  /* Screen width        */
#define NUMBER 80 /* Amount of strings there are*/
#define DELAY 0  /* Delay variable       */
#define LENGTH 500
typedef struct
	 {
	 int x;
	 int y;
	 int len;
	 char text[LENGTH];
	 int type;
	 }PLNE;
PLNE master[NUMBER];
int loop;      /*Temporary place holder  used for loops */
int length,current;
int far *farptr;
int memx, memy;
union REGS regs;
void init(int num)/*Sets up the array for the inputed number */
 {
   int proloop; /*Procedure loop */
   master[num].type=random(2);          /* Sets color to either white */
   if(master[num].type==0)master[num].type=2;  /* or greeen         */
   else master[num].type=15;          /*          */
   master[num].x=random(MAXX-3)+1;        /* Generates random x */
   master[num].y=random(MAXY-3)+1;        /* and y cords    */
   master[num].len=random(LENGTH-5)+5;
   for(proloop=0;proloop<=master[num].len;proloop++)master[num].text[proloop]=(random(36))+65;
 }
void move(int num)  /* Moves the array of the inputed number down one */
 {
 master[num].y++;
 if(master[num].y>MAXY+master[num].len)init(num);
 }
void write(int num,int color, int start, int end,int clear)/* Writes the inputed string in the inputed */
 { 			    	              /* color for the inputed length       */
 int tempy; //Tempy used as a temp counter
 farptr=(int far *) 0xB8000000;       /*Sets pointer to write to memory, faster then cprintf();*/
 for(tempy=start;tempy<=end;tempy++)    /*Writes all text that start-end allows*/
  {
  if(master[num].y-tempy>0&&master[num].y-tempy<MAXY) /*Only writes if on the screen*/
  {
  memx=(master[num].x)-1;                 /* These convert X&Y cords to the correct place */
  memy=((master[num].y-tempy)-1)*80;            /* in memory                  */
  if(color==2) *(farptr+memx+memy) = master[num].text[tempy] | 0x0220; /* The 0x0220 is hex decimal, turns text green */
  else *(farptr+memx+memy) = master[num].text[tempy] | 0x0720;     /* The hex decimal turns text white */
  }
  }
 if(clear==1)
  {
  memx=(master[num].x)-1;
  memy=((master[num].y-tempy)-1)*80;
  *(farptr+memx+memy) = 0x0720;
  }
 }
main(void)
 {
 clrscr();     			    /* Prep of  */
 textmode(C4350);  			    /* screen and */
 randomize();            /* textmode  */
 for(loop=0;loop<=NUMBER-1;loop++)init(loop);
 while (!kbhit())
  {
  current=random(NUMBER);
  write(current,master[current].type,0,0,0); /*Writes first letter of the text, done to write white letters*/
  write(current,2,1,(master[current].len),1);/*Writes the remaining string and "clears" the screen behind the string*/
  move(current);
  delay(DELAY);
  }
 return 0;
 }
```

