
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : blinker03.gcc.bin

all : gcc clang

clang : blinker03.clang.norm.bin blinker03.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



blinker03.gcc.o : blinker03.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c blinker03.c -o blinker03.gcc.o

blinker03.gcc.bin : memmap vectors.o blinker03.gcc.o
	$(ARMGNU)-ld -o blinker03.gcc.elf -T memmap vectors.o blinker03.gcc.o
	$(ARMGNU)-objdump -D blinker03.gcc.elf > blinker03.gcc.list
	$(ARMGNU)-objcopy blinker03.gcc.elf blinker03.gcc.bin -O binary



blinker03.clang.bc : blinker03.c
	clang $(LOPS) -c blinker03.c -o blinker03.clang.bc

blinker03.clang.norm.bin : memmap vectors.o blinker03.clang.bc
	llc $(LLCOPS) blinker03.clang.bc -o blinker03.clang.norm.s
	$(ARMGNU)-as blinker03.clang.norm.s -o blinker03.clang.norm.o
	$(ARMGNU)-ld -o blinker03.clang.norm.elf -T memmap vectors.o blinker03.clang.norm.o
	$(ARMGNU)-objdump -D blinker03.clang.norm.elf > blinker03.clang.norm.list
	$(ARMGNU)-objcopy blinker03.clang.norm.elf blinker03.clang.norm.bin -O binary

blinker03.clang.opt.bin : memmap vectors.o blinker03.clang.bc
	opt $(OOPS) blinker03.clang.bc -o blinker03.clang.opt.bc
	llc $(LLCOPS) blinker03.clang.opt.bc -o blinker03.clang.opt.s
	$(ARMGNU)-as blinker03.clang.opt.s -o blinker03.clang.opt.o
	$(ARMGNU)-ld -o blinker03.clang.opt.elf -T memmap vectors.o blinker03.clang.opt.o
	$(ARMGNU)-objdump -D blinker03.clang.opt.elf > blinker03.clang.opt.list
	$(ARMGNU)-objcopy blinker03.clang.opt.elf blinker03.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

