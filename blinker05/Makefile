
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : blinker05.gcc.bin

all : gcc clang

clang : blinker05.clang.norm.bin blinker05.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



blinker05.gcc.o : blinker05.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c blinker05.c -o blinker05.gcc.o

blinker05.gcc.bin : memmap vectors.o blinker05.gcc.o
	$(ARMGNU)-ld -o blinker05.gcc.elf -T memmap vectors.o blinker05.gcc.o
	$(ARMGNU)-objdump -D blinker05.gcc.elf > blinker05.gcc.list
	$(ARMGNU)-objcopy blinker05.gcc.elf blinker05.gcc.bin -O binary



blinker05.clang.bc : blinker05.c
	clang $(LOPS) -c blinker05.c -o blinker05.clang.bc

blinker05.clang.norm.bin : memmap vectors.o blinker05.clang.bc
	llc $(LLCOPS) blinker05.clang.bc -o blinker05.clang.norm.s
	$(ARMGNU)-as blinker05.clang.norm.s -o blinker05.clang.norm.o
	$(ARMGNU)-ld -o blinker05.clang.norm.elf -T memmap vectors.o blinker05.clang.norm.o
	$(ARMGNU)-objdump -D blinker05.clang.norm.elf > blinker05.clang.norm.list
	$(ARMGNU)-objcopy blinker05.clang.norm.elf blinker05.clang.norm.bin -O binary

blinker05.clang.opt.bin : memmap vectors.o blinker05.clang.bc
	opt $(OOPS) blinker05.clang.bc -o blinker05.clang.opt.bc
	llc $(LLCOPS) blinker05.clang.opt.bc -o blinker05.clang.opt.s
	$(ARMGNU)-as blinker05.clang.opt.s -o blinker05.clang.opt.o
	$(ARMGNU)-ld -o blinker05.clang.opt.elf -T memmap vectors.o blinker05.clang.opt.o
	$(ARMGNU)-objdump -D blinker05.clang.opt.elf > blinker05.clang.opt.list
	$(ARMGNU)-objcopy blinker05.clang.opt.elf blinker05.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

