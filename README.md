; Assembly-80x86
; Bubble Sorting

.586
.MODEL FLAT

INCLUDE io.h            ; header file for input/output

.STACK 4096

.DATA 
array DWORD 40 DUP (?)
string DWORD 40 DUP (?), 0
user_entry BYTE "Enter a number:", 0
show_array	BYTE  "The array values are:", 0
sorted_prompt BYTE "The array is sorted", 0
not_sorted_prompt BYTE "The array is NOT sorted", 0
num_elmts DWORD 10
nest_loop_counter DWORD 9
.CODE
_MainProc PROC
	call User_Input ; askes user to input numbers
	call Show_Array_Values ; shows what user have entered
	call Bubble_Sort ; sorts the values using bubble sort algorithm
	call Show_Array_Values ; shows what user sorted array
	ret
_MainProc ENDP


User_Input PROC

lea ebx, array; load array address into EBX register
mov ecx, num_elmts     ; number of elements
doWhile:	input user_entry, string, 40 ; prompt the user to enter number
atod string; convert to integer
mov [ebx], eax ; move user inputted number into the array element
add ebx, 4    ; point at next element
loop	doWhile; continue in the while loop
ret
User_Input ENDP


Show_Array_Values PROC
lea ebx, array; load array address into EBX register
mov ecx, num_elmts     ; number of elements

doWhile0:	;for loop for number of elements
mov eax, [ebx]
dtoa string, eax; convert to integer
output show_array, string
add ebx, 4    ; point at next element
loop	doWhile0; continue in the while loop
ret
Show_Array_Values ENDP

Bubble_Sort PROC
begin_sort:
mov ecx, num_elmts     ; number of elements to control the loops
dec ecx
lea ebx, array; load array address into EBX register
forCount:
	cmp ecx, 0
	jle endFor; exits loop if ecx is zero.
mov eax, [ebx]	; first element placed in eax
add ebx, 4    ; point at next element
mov edx, [ebx]	; second element placed in edx

compare_elements:
cmp eax, edx; comparing the first and second element
jl	no_exchange ;  elements in right order, compare eax to next element
jnl exchange	;not_sorted first element is greater than the next element and places will need to swap
loop forCount
ret
no_exchange:	loop forCount
				dec nest_loop_counter; largest number moved to end of array, decrementing nest loop counter
				jz endFor
				jmp begin_sort
				cmp ecx, 0
				jle endFor; exits loop if ecx is zero.
exchange:
	mov dword ptr [ebx], eax
	mov dword ptr [ebx-4], edx
	loop forCount; goes to next element
	dec nest_loop_counter; largest number moved to end of array, decrementing nest loop counter
	jz endFor
	jmp begin_sort
	
endFor:
ret

Bubble_Sort ENDP



END                             ; end of source code


