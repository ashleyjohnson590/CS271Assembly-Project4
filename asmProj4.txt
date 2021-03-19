TITLE Prime Numbers Program Project 4

; Author: Ashley Johnson
; Last Modified: 2/10/2021
; OSU email address: johnsas8@oregonstate.edu
; Course number/section:   CS271 Section 400
; Project Number:   4              Due Date:2/21/2021
; Description: This file asks user to enter a number between [0, 200] and
; displays the prime numbers from 0 to the number entered.  

INCLUDE Irvine32.inc
; initialize constant range limits
   upperLimit = 200
   lowerLimit = 0
.data
   progTitle BYTE   "Prime Numbers Program #4   ", 0
   printName BYTE   "by Ashley Johnson ", 0
   introduction2	BYTE	"I am the introduction ", 0
   instructions		BYTE   "Enter the number of prime numbers you would like to see.", 0
   instructions2	BYTE	"I'll accept orders for up to 200 primes. ", 0
   inputnum	BYTE	"Enter the number of primes to display [1....200]: ", 0
   errorMessage BYTE    "No primes for you! Number out of range. Try again. ", 0
   newPrime	SDWORD	?
   spaceString		BYTE	" ", 0
   possiblePrime	SDWORD	2
   printNum	BYTE	"looking at number", 0
   numFound SDWORD	0
   currentPrime	SDWORD	0
   quotient	SDWORD	?
   remainder SDWORD	?
   numEntered	SDWORD	?	; number user enters
	goodbye  BYTE   "Results certified by Ashley. Goodbye. " ,0

.code
main PROC

  
	mov edx, OFFSET progTitle  ;print program title
	call WriteString

	mov   edx, OFFSET printName    ;print my name
	call WriteString
	call CrLf
	call CrLf
	call introduction
	call CrLf
	call getUserData
	call showPrimes
	call farewell
;displays instructions
introduction PROC

;displays first instruction
	mov edx, OFFSET instructions
	call WriteString
	call crLf

	mov edx, OFFSET instructions2
	call WriteString
	call crLf

	RET
introduction ENDP

getUserData PROC

;get number of primes from user

	mov edx, OFFSET inputnum
	call WriteString

	call ReadInt
	mov numEntered, eax
	
	call validate
	
	RET
getUserData ENDP

;validate number entered within range
validate PROC
	_checkNum:
	mov eax, numEntered
	cmp eax, lowerLimit
	jle _errorMessage
	cmp eax, upperLimit
	jg _errorMessage
	RET

	_errorMessage:
		mov edx, OFFSET errorMessage
		call WriteString
		call crlf
		call getUserData
	RET

validate ENDP
 
;calculate prime numbers
showPrimes PROC
	mov ecx, numEntered
	_checkPrime:
	mov ebx, numFound
	cmp ebx, numEntered
	jge farewell
	call isPrime
	jmp _checkPrime

	RET
showPrimes ENDP

;prints prime number after validation
isPrime PROC
	mov ecx, possiblePrime
	dec ecx
	PrimeLoop:
		cmp ecx, 1
		je _isPrime
		mov eax, possiblePrime
		cdq
		div ecx
		cmp edx, 0
		je _notPrime

	loop PrimeLoop
	_isPrime:
	mov eax, possiblePrime
	call writeDec
	mov edx, OFFSET spaceString
	call WriteString
	inc numFound
	_notPrime:
	inc possiblePrime
	RET

isPrime ENDP

;print goodbye message
farewell PROC
	call CrLf
	call crlf
	mov edx, OFFSET goodbye
	call WriteString
farewell ENDP

	Invoke exitProcess,0	;exit to operating system
main ENDP

END main