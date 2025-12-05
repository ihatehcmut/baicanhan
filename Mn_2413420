    .data
# --- File & headers ---
filename:       .asciiz "SOLE.TXT"
line1:          .asciiz "So thuc 1 (2 so le): "
line2:          .asciiz "So thuc 2 (3 so le): "
line3:          .asciiz "So thuc 3 (4 so le): "
newline:        .asciiz "\n"

# --- Buffers & constants ---
buffer:         .space 64
THOUSAND:       .float 1000.0   # Random tu 0 -> 1000
HALF:           .float 0.5      # De lam tron

    .text
    .globl main

main:
    # 1) Khoi tao Seed
    li   $v0, 30                # Syscall 30: Lay thoi gian he thong
    syscall
    move $a0, $v0        
    li   $v0, 40                # Syscall 40: Set seed cho bo random
    syscall

    # 2) Mo file
    li   $v0, 13                # Syscall 13: Open file
    la   $a0, filename
    li   $a1, 1                 # write-only
    li   $a2, 0                 # Bo qua
    syscall
    move $s0, $v0        
    bltz $s0, end_main          

    lwc1 $f10, THOUSAND

    # =======================================
    # ---- Number 1: n = 2 ----
    # =======================================
    li   $v0, 43                # Syscall 43: Random float
    syscall
    mul.s $f2, $f0, $f10
    
    # Print to console - Header 1
    li   $v0, 4            
    la   $a0, line1
    syscall
    
    # Header 1 - File
    li   $v0, 15                # Syscall 15: Write to file
    move $a0, $s0
    la   $a1, line1             # Dia chi chuoi can ghi
    li   $a2, 21                # Do dai chuoi can ghi
    syscall

    # Convert n = 2
    mov.s $f12, $f2
    li   $a0, 2           
    jal  convert_float

    # Print to console - Number
    li   $v0, 4
    la   $a0, buffer
    syscall
    
    # Print to console - Newline
    li   $v0, 4
    la   $a0, newline
    syscall

    # Write to file - Number
    li   $v0, 15
    move $a0, $s0
    la   $a1, buffer            # Buffer chua chuoi so vua convert
    move $a2, $v1               # v1: Do dai chuoi
    syscall

    # Write to file - Newline
    li   $v0, 15
    move $a0, $s0
    la   $a1, newline
    li   $a2, 1
    syscall

    # =======================================
    # ---- Number 2: n = 3 ----
    # =======================================
    li   $v0, 43
    syscall
    mul.s $f2, $f0, $f10

    # Print to console - Header 2
    li   $v0, 4
    la   $a0, line2
    syscall

    # Header 2 - File
    li   $v0, 15
    move $a0, $s0
    la   $a1, line2
    li   $a2, 21
    syscall

    # Convert n = 3
    mov.s $f12, $f2
    li   $a0, 3
    jal  convert_float

    # Print to console - Number
    li   $v0, 4
    la   $a0, buffer
    syscall
    
    # Print to console - Newline
    li   $v0, 4
    la   $a0, newline
    syscall

    # Write to file - Number
    li   $v0, 15
    move $a0, $s0
    la   $a1, buffer
    move $a2, $v1
    syscall

    # Write to file - Newline
    li   $v0, 15
    move $a0, $s0
    la   $a1, newline
    li   $a2, 1
    syscall

    # =======================================
    # ---- Number 3: n = 4 ----
    # =======================================
    li   $v0, 43
    syscall
    mul.s $f2, $f0, $f10

    # Print to console - Header 3
    li   $v0, 4
    la   $a0, line3
    syscall

    # Header 3 - File
    li   $v0, 15
    move $a0, $s0
    la   $a1, line3
    li   $a2, 21
    syscall

    # Convert n = 4
    mov.s $f12, $f2
    li   $a0, 4
    jal  convert_float

    # Print to console - Number
    li   $v0, 4
    la   $a0, buffer
    syscall
    
    # Print to console - Newline
    li   $v0, 4
    la   $a0, newline
    syscall

    # Write to file - Number
    li   $v0, 15
    move $a0, $s0
    la   $a1, buffer
    move $a2, $v1
    syscall

    # Write to file - Newline
    li   $v0, 15
    move $a0, $s0
    la   $a1, newline
    li   $a2, 1
    syscall

    # Close file
    li   $v0, 16
    move $a0, $s0
    syscall

    j    end_main
    nop

end_main:
    li   $v0, 10
    syscall


# ==============================================================
# Ham: convert_float
# Co chuc nang PADDING ZERO (005, 042...)
# ==============================================================
convert_float:
    addi $sp, $sp, -24
    sw   $ra, 0($sp)
    sw   $s0, 4($sp)      
    sw   $s1, 8($sp)      
    sw   $s2, 12($sp)     
    sw   $s4, 20($sp)     

    move $s0, $a0         # Luu n (so thap phan)
    la   $s2, buffer      
    move $s4, $s2         # Luu diem bat dau buffer

    # --- Xu ly phan nguyen ---
    trunc.w.s $f0, $f12   # Chuyen float thanh int (cat bo phan thap phan)
    mfc1    $s1, $f0      # Chuyen ket qua tu f0 sang s1
    
    # Convert int -> string (nguoc)
    li      $t0, 10
convert_int_loop:
    div     $s1, $t0
    mfhi    $t1                   # Lay so du (chu so cuoi)
    mflo    $s1                   # Lay thuong (phan con lai)
    addi    $t1, $t1, '0'         # Chuyen thanh ASCII
    sb      $t1, 0($s2)           # Luu vao buffer
    addi    $s2, $s2, 1           # Tang con tro buffer
    bnez    $s1, convert_int_loop

# Kiem tra do dai hien tai. Neu < 3 thi them '0'
# Chuoi dang bi nguoc nen them '0' vao sau
pad_loop:
    sub     $t7, $s2, $s4       # t7 = do dai hien tai (con tro hien tai - diem dau)
    bge     $t7, 3, pad_done    # Neu da du 3 chu so (hoac hon) thi thoi
    li      $t6, '0'
    sb      $t6, 0($s2)         # Them '0'
    addi    $s2, $s2, 1         # Tang con tro
    j       pad_loop
pad_done:
    # --------------------------

    # Dao nguoc chuoi (vi dang bi nguoc + padding)
    move    $t1, $s4      # Dau chuoi
    move    $t2, $s2      # Cuoi chuoi + 1
    addi    $t2, $t2, -1  # Cuoi chuoi
reverse_loop:
    bge     $t1, $t2, reverse_done
    lb      $t3, 0($t1)   # Load dau
    lb      $t4, 0($t2)   # Load cuoi
    sb      $t4, 0($t1)   # Swap
    sb      $t3, 0($t2)   # Swap
    addi    $t1, $t1, 1   # Tien dau
    addi    $t2, $t2, -1  # Lui cuoi
    j       reverse_loop
reverse_done:

    # Them dau cham
    li      $t0, '.'
    sb      $t0, 0($s2)
    addi    $s2, $s2, 1

    # --- Xu ly phan thap phan ---    
    trunc.w.s $f0, $f12         # Lay phan nguyen (dang int)
    cvt.s.w $f1, $f0            # Chuyen int nguoc lai float de tru
    sub.s   $f3, $f12, $f1 

    # Nhan 10^n
    li      $t0, 1
    li      $t1, 10
    move    $t2, $s0      
pow_loop:
    beqz    $t2, pow_done
    mul     $t0, $t0, $t1
    addi    $t2, $t2, -1
    j       pow_loop
pow_done:
    # Chuyen 10^n sang float de nhan
    mtc1    $t0, $f4
    cvt.s.w $f4, $f4      
    mul.s   $f3, $f3, $f4        # f3 = Phan le * 10^n

    # Lam tron: cong them 0.5 roi cat bo phan du
    lwc1    $f5, HALF
    add.s   $f3, $f3, $f5
    trunc.w.s $f3, $f3
    mfc1    $s1, $f3      

    # In tung chu so thap phan: chia cho 10^(n - 1), 10^(n - 2)...
    li      $t3, 10
    li      $t4, 1                # t4: So chia
    addi    $t5, $s0, -1  
make_divisor:
    beqz    $t5, divisor_ready
    mul     $t4, $t4, $t3
    addi    $t5, $t5, -1
    j       make_divisor
divisor_ready:

frac_print_loop:
    beqz    $s0, frac_done
    div     $s1, $t4      
    mflo    $t6                   # Thuong = chu so can in hien tai
    mfhi    $s1                   # Du = phan con lai
    addi    $t6, $t6, '0'
    sb      $t6, 0($s2)           # Luu vao buffer
    addi    $s2, $s2, 1
    
    div     $t4, $t3              # Giam so chia di 10 lan
    mflo    $t4
    addi    $s0, $s0, -1
    j       frac_print_loop
frac_done:

    # Ket thuc chuoi
    sb      $zero, 0($s2)

    # Tinh do dai
    la      $t0, buffer
    sub     $v1, $s2, $t0        # v1 = con tro hien tai - con tro dau

    lw      $s4, 20($sp)
    lw      $s2, 12($sp)
    lw      $s1, 8($sp)
    lw      $s0, 4($sp)
    lw      $ra, 0($sp)
    addi    $sp, $sp, 24
    jr      $ra
    nop
