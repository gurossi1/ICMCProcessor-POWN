# ICMCProcessor-POWN
Based on the ICMC Processor architecture, the project added a new instruction: POWN. It consists of implementing the arithmetic operation potentiation within the processor's set of instructions.

# How to run the simulator
1) Open the terminal at the directory in which you installed the package
2) Run these set of commands: ```./montador teste.asm teste.mif```
```gcc Simple_Simulator.c -o Simple_Simulator```
```./Simple_Simulator```
3) The terminal should show the sequence "ABCDEFGIJKLMZNOP", idicating all the instructions are working properly

# New processor instruction
We added the instruction "POWN", which calculates the Nth power of a number (Rx <- Ry^Rz)
## defs.h
```c
//defs.h changes:

#define POWN_CODE 100
//----------------------
#define POWN "100110"
//----------------------
#define POWN_STR "POWN"
```
## montador.c
```c
//montador.c changes:

/* ==============
   Pown Rx, Ry, Rz
   ==============
*/

   case POWN_CODE :
      str_tmp1 = parser_GetItem_s();
      val1 = BuscaRegistrador(str_tmp1);
      free(str_tmp1);
      parser_Match(',');
      str_tmp2 = parser_GetItem_s();
      val2 = BuscaRegistrador(str_tmp2);
      free(str_tmp2);
      parser_Match(',');
      str_tmp3 = parser_GetItem_s();
      val3 = BuscaRegistrador(str_tmp3);
      free(str_tmp3);
      str_tmp1 = ConverteRegistrador(val1);
      str_tmp2 = ConverteRegistrador(val2);
      str_tmp3 = ConverteRegistrador(val3);
      sprintf(str_msg,"%s%s%s%s0",POWN,str_tmp1,str_tmp2,str_tmp3);
      free(str_tmp1);
      free(str_tmp2);
      free(str_tmp3);
      parser_Write_Inst(str_msg,end_cnt);
      end_cnt += 1;
      break;

//--------------------------------------------------------------------

else if (strcmp(str_tmp, POWN_STR) == 0)
    {
        return POWN_CODE;
    }

//--------------------------------------------------------------------

case POWN_CODE :
```
## simple_simulator.c
```c
//simple_simulator.c changes:

#define POWN 38     // "100110"; -- POWN Rx Ry Rz  -- Rx <- Ry^Rz

//--------------------------------------------------------------------
// DECODE_STATE
case POWN:
    selM3 = ry;
    selM4 = rz;
    OP = POWN;
    carry = IR;
    selM2 = sULA;
    LoadReg[rx] = 1;
    selM6 = sULA;
    LoadFR = 1;
    // -----------------------------
    state = STATE_FETCH;
    break;

//--------------------------------------------------------------------
//STATE_EXECUTE2
//pown rx, ry, rz ---> rx = ry^rz
//RX(resultado) RY(x) RZ(y)
        
case POWN:
    result = 1;
    for(int i = 0; i < y; i++) {
        result *= x;
    }
    if(result > MAX_VAL)// Arithmetic overflow
        auxFRbits[ARITHMETIC_OVERFLOW] = 1;
    else
        auxFRbits[ARITHMETIC_OVERFLOW] = 0;
    break;
```

### Useful links
- Eduardo's gitlab: https://gitlab.com/simoesusp/disciplinas

### Team:
- Gustavo Sanches Rossi
- Luis Henrique Hergesel Lima
- Sandy da Costa Dutra
- Breno Campos Vaz de Arruda
- Otavio Augusto Colucci de Oliveira
