# word-divider
ORG 100h     ; Set the origin to 100h (default for DOS COM files)

MOV AH, 09h   ; Function 09h - Display String
MOV DX, offset hello_msg   ; Load address of hello_msg into DX register
INT 21h       ; Call DOS interrupt to display prompt message

MOV AH, 0Ah   ; Function 0Ah - Read String
MOV DX, offset input_buffer   ; Load address of input_buffer into DX register
INT 21h       ; Call DOS interrupt to read a string

; Start a new line before displaying characters
MOV AH, 02h   ; Function 02h - Display Character
MOV DL, 0Ah   ; Load the newline character to DL register
INT 21h       ; Call DOS interrupt to start a new line

; Display individual characters from input_buffer
MOV SI, offset input_buffer + 2   ; Load address of input_buffer into SI register (skip the length byte)

next_char:
MOV AL, [SI]   ; Move character from memory location pointed by SI into AL register

; Check if AL is the null terminator (end of string)
CMP AL, 0
JE terminate

; Display character
MOV AH, 02h   ; Function 02h - Display Character
MOV DL, AL    ; Load the character to DL register
INT 21h       ; Call DOS interrupt to display the character

MOV DL, ','   ; Display a comma
INT 21h       ; Call DOS interrupt to display the comma

INC SI        ; Increment SI to point to the next character in input_buffer

JMP next_char ; Jump to process the next character

; After displaying all characters and commas, start a new line
terminate:
MOV AH, 02h   ; Function 02h - Display Character
MOV DL, 0Ah   ; Load the newline character to DL register
INT 21h       ; Call DOS interrupt to start a new line

MOV AH, 4Ch    ; Function 4Ch - Terminate Program
INT 21h        ; Call DOS interrupt to terminate the program

hello_msg DB 'Enter a word: $'   ; Display prompt message

; Define a buffer to store the input word (adjust size as needed)
input_buffer DB 80, ?, 80 DUP (0) ; Declare a buffer with room for input and a length byte
