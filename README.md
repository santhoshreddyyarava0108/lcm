# lcm
ORG 100h

JMP start

input1 db "Enter the First Digit: $"
input2 db 0Dh,0Ah,"Enter the Second Digit: $"
output db 0Dh,0Ah,"LCM of the Given two numbers is: $"

start:
    mov dx, offset input1
    mov ah, 09h
    int 21h  

    mov ah, 01h
    int 21h
    sub al, '0'
    mov cl, al

    mov dx, offset input2
    mov ah, 09h
    int 21h

    mov ah, 01h
    int 21h
    sub al, '0'
    mov ch, al

    mov al, cl
    mov bl, ch

GCD_LOOP:
    cmp bl, 0
    je DONE_GCD
    mov ah, 0
    div bl
    mov al, bl
    mov bl, ah
    jmp GCD_LOOP

DONE_GCD:
    mov bh, al

    mov al, cl
    mul ch

    mov bl, bh
    div bl

    mov bl, al
    mov dx, offset output
    mov ah, 09h
    int 21h 

    mov ax, 0
    mov al, bl

    mov cx, 10

dec_to_ascii:
    xor dx, dx
    div cx
    add dl, '0'
    push dx
    test al, al
    jnz dec_to_ascii

print_decimal:
    pop dx
    mov ah, 02h
    int 21h
    cmp sp, 0
    jne print_decimal

    mov ah, 4Ch
    int 21h
