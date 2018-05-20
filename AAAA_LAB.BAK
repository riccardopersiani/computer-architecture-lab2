;Problem Solving session n2
;Riccardo Persiani

DIM	 EQU	50
MIN	 EQU	20
CR	 EQU	0DH
LF	 EQU	0AH
NL	 EQU	10

.MODEL SMALL
.STACK
.DATA

FIRST_ROW  DB DIM DUP(0)
SECOND_ROW DB DIM DUP(0)
THIRD_ROW  DB DIM DUP(0)
FOURTH_ROW DB DIM DUP(0)
OCCURENCES DB 52 DUP(0)
 
.CODE
.STARTUP   		

		PUSH AX			;Save all registers used
		PUSH BX
		PUSH CX
		PUSH DX
		PUSH DI
		PUSH SI

		XOR DX, DX		;DX from 0 to 3

		XOR CX, CX		;Initialize CX
		XOR SI, SI

select:		LEA SI, FIRST_ROW	;Loading all the string starting from the first
		CMP DX, 0
		JE insert

		XOR CX, CX
		XOR SI, SI
		LEA SI, SECOND_ROW
		CMP DX, 1
		JE insert

		XOR CX, CX
		XOR SI, SI
		LEA SI, THIRD_ROW
		CMP DX, 2
		JE insert

		XOR CX, CX
		XOR SI, SI
		LEA SI, FOURTH_ROW
		CMP DX, 3
		JE insert


insert: 	MOV AH, 1		;Set AH=1 in order to read from keyboard
		INT 21H 		;Insert the char from keyboard
		CMP AL, CR		;Check if the user press ENTER button
		JE  range
		CMP AL, NL
		JE  range
		
		PUSH AX
		CALL CHAR_CHECK 	;Call the Proc. to Check if the input is a char
		POP AX

		CMP AX, 0		;If it is NOT a character, reinsert the input without increment the counter
		JE  insert

		INC CX			;INC counter[MAX 50]

		MOV [SI], AL
		INC SI
		
range:		CMP CX, MIN		;If S length<20? Continue the insert
		JB insert

		CMP AL,CR		; If ENTER button, stop the insert
		JE next

		CMP CX, DIM		;If S  lenght=50? Stop the insert
		JNE insert
		

next:		INC DX			;Go to the next ROW

		CMP DX,4		;If the user inserted every row proceed
		JNE select

		XOR BX, BX		;BX is the counter from 0 to 51
		XOR DH, DH
		MOV DL, OCCURENCES[BX]	;Put in DX the first value of Occurences
					;DX will be the MAX
find_max:	ADD BX,1			    ;
		CMP OCCURENCES[BX], DL	  ;If Occ[BX] < DL
		JB not_greater		  ;Need to keep the value

greater:	MOV DL, OCCURENCES[BX]

not_greater:	CMP BX, 51
		JNE find_max

		SHR DL, 1	 ;DL = max/2

		XOR BX, BX
		XOR DH, DH
		MOV AH, 2


find_max21:	CMP OCCURENCES[BX], DL
		JNE no_print1

print1:	       PUSH DX
	       ADD BX, 65
	       MOV DX, BX
	       SUB BX, 65
	       INT 21H
	       POP DX

no_print1:     ADD BX,1
	       CMP BX,26
	       JNE find_max21

find_max22:	CMP OCCURENCES[BX], DL
		JNE no_print2

print2:	       PUSH DX
	       ADD BX, 71
	       MOV DX, BX
	       INT 21H
	       SUB BX, 71
	       POP DX

no_print2:     ADD BX,1
	       CMP BX,53
	       JNE find_max22

		SHL DL, 1	 ;Restoring DL = max

		XOR BX, BX
		XOR DH, DH
		MOV AH, 2
		PUSH DX 	   ;va a capo
		XOR DH, DH
		MOV DL, LF
		INT 21H
		MOV DL, CR
		INT 21H
		POP DX

find_max31:	CMP OCCURENCES[BX], DL	   ;print Max Value
		JNE no_print3

print3:	       PUSH DX
	       ADD BX, 65
	       MOV DX, BX
	       SUB BX, 65
	       INT 21H
	       POP DX

no_print3:     ADD BX,1
	       CMP BX,26
	       JNE find_max31

find_max32:	CMP OCCURENCES[BX], DL
		JNE no_print4

print4:	       PUSH DX
	       ADD BX, 71
	       MOV DX, BX
	       INT 21H
	       SUB BX, 71
	       POP DX

no_print4:     ADD BX,1
	       CMP BX,53
	       JNE find_max32

;Chifer Caesar


		XOR DI, DI		;Initialize CX
		XOR SI, SI
		XOR BX, BX
		XOR CX, CX
		XOR DX, DX



prep_cipher:	INC DH
		LEA SI, FIRST_ROW	;Loading all the string starting from the first
		CMP DH, 1
		JE cipher

		XOR CX, CX
		XOR SI, SI
		XOR BX, BX
		LEA SI, SECOND_ROW
		CMP DH, 2		  ;K=2
		JE cipher

		XOR CX, CX
		XOR SI, SI
		XOR BX, BX
		LEA SI, THIRD_ROW
		CMP DH, 3
		JE cipher

		XOR CX, CX
		XOR SI, SI
		XOR BX, BX
		LEA SI, FOURTH_ROW
		CMP DH, 4
		JE cipher

		CMP DH,5
		JE end_program

cipher: 	MOV CL, byte ptr[SI]	;load in CL the char in the string loaded
		CMP CL, 0		;Is the string ended?
		JE prep_cipher		;If yes go to the next one

		MOV AL, byte ptr[SI]	;Metto il carattere della stringa in AL

		CMP DH,1		;k=1 Controllo su Z,z
		JE check1

		CMP DH,2		;k=2 Controllo su Z,z,Y,y
		JE check3

		CMP DH,3		;k=3 Controllo su Z,z,Y,y,X,x
		JE check5

		CMP DH,4		 ;k=4 Controllo su Z,z,Y,y,X,x,W,w
		JE check7

check7:		CMP CL, 'W'		;porta Z da 90 a 96
		JNE check8		;esci se il char non Š uguale a Z
		ADD AL, 6

check8:		CMP CL, 'w'		;porta z da 122 a 64 p
		JNE check5		;esci se il carattere non Š uguale a z
		SUB AL, 58

check5:		CMP CL, 'X'		;porta Z da 90 a 96
		JNE check6		;esci se il char non Š uguale a Z
		ADD AL, 6

check6:		CMP CL, 'x'		;porta z da 122 a 64 p
		JNE check3		;esci se il carattere non Š uguale a z
		SUB AL, 58

check3:		CMP CL, 'Y'		;porta Y da 89 a 95
		JNE check4		;esci se il char non Š uguale a Z
		ADD AL, 6

check4:		CMP CL, 'y'		;porta z da 122 a 64 p
		JNE check1		;esci se il carattere non Š uguale a z
		SUB AL, 58

check1:		CMP CL, 'Z'		;porta Z da 90 a 96
		JNE check2		;esci se il char non Š uguale a Z
		ADD AL, 6

check2:		CMP CL, 'z'		;porta z da 122 a 64 p
		JNE no_exept		;esci se il carattere non Š uguale a z
		SUB AL, 58

no_exept:	ADD AL, DH		;char+k

		MOV AH, 2
		MOV DL, AL
		INT 21H 		;Stampa

		INC SI			 ;Go to the next char

		CMP SI, 51		 ;string limit is 50
		JNE cipher


end_program:

		POP SI			;restore all registers used
		POP DI
		POP DX
		POP CX
		POP BX
		POP AX

.EXIT

CHAR_CHECK PROC
		PUSH BP 		;Store bp
		MOV BP, SP		;Riferimento per lo stack pointer
		PUSH BX

		MOV BX, [BP]+4		;Punto ai dati AX, scorro stack

		CMP BL, 'A'		;Is the char inserted < 'A'?
		JB not_correct		;If yes, is NOT a character
		
		CMP BL, 'Z'		;Is the char inserted <= 'Z'?
		JBE correct1 ;?		;If yes, is a character
		
		CMP BL, 'a'		;Is the char inserted < 'a' ?
		JB not_correct		;If yes, is NOT a character
		
		CMP BL, 'z'		;is the char inserted < 'z' ?
		JBE correct2		;If yes, is a character

					;Need to insert a correct letter (A-Z) or (a-z)
not_correct:	MOV BX, 0		;AX=0 recognise that is not a char
		JMP exit_p

correct1:	SUB BL, 65		;Char from A to Z
		MOV BH, 0		;Otherwise i have the wrong address because bh is not 0
		ADD OCCURENCES[BX], 1	;INC the Occurrences vector
		ADD BL, 65		;restore
		JMP  exit_p

correct2:	SUB BL, 71		;Char from a to z
		MOV BH, 0
		ADD OCCURENCES[BX], 1	;INC the Occurences vector
		ADD BL, 71

exit_p:
		MOV [BP]+4, BX		;Is a char
		POP BX
		POP BP
RET
CHAR_CHECK ENDP

CHAR_CIPHER PROC
		PUSH BP 		;Store bp
		MOV BP, SP		;Riferimento per lo stack pointer
		PUSH BX

		MOV BX, [BP]+4		;Punto ai dati AX, scorro stack

		CMP BL, 'Z'		;Is the char MAIUSC?
		JBE method1		;If yes, use +65

		CMP BL, 'Z'		;Is the char minusc?
		JBE method2		;If yes, use +71

method1:	SUB BL, 65		;Char from A to Z
		MOV BH, 0		;Otherwise i have the wrong address because bh is not 0
		ADD OCCURENCES[BX], 1	;INC the Occurrences vector
		ADD BL, 65		;restore
		JMP  exit_p

method2:	SUB BL, 71		;Char from a to z
		MOV BH, 0
		ADD OCCURENCES[BX], 1	;INC the Occurences vector
		ADD BL, 71

exit_p:
		MOV [BP]+4, BX		;Is a char
		POP BX
		POP BP
RET
CHAR_CIPHER ENDP
END
