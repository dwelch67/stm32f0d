
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : blinker04.gcc.bin

all : gcc clang

clang : blinker04.clang.norm.bin blinker04.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



blinker04.gcc.o : blinker04.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c blinker04.c -o blinker04.gcc.o

blinker04.gcc.bin : memmap vectors.o blinker04.gcc.o
	$(ARMGNU)-ld -o blinker04.gcc.elf -T memmap vectors.o blinker04.gcc.o
	$(ARMGNU)-objdump -D blinker04.gcc.elf > blinker04.gcc.list
	$(ARMGNU)-objcopy blinker04.gcc.elf blinker04.gcc.bin -O binary



blinker04.clang.bc : blinker04.c
	clang $(LOPS) -c blinker04.c -o blinker04.clang.bc

blinker04.clang.norm.bin : memmap vectors.o blinker04.clang.bc
	llc $(LLCOPS) blinker04.clang.bc -o blinker04.clang.norm.s
	$(ARMGNU)-as blinker04.clang.norm.s -o blinker04.clang.norm.o
	$(ARMGNU)-ld -o blinker04.clang.norm.elf -T memmap vectors.o blinker04.clang.norm.o
	$(ARMGNU)-objdump -D blinker04.clang.norm.elf > blinker04.clang.norm.list
	$(ARMGNU)-objcopy blinker04.clang.norm.elf blinker04.clang.norm.bin -O binary

blinker04.clang.opt.bin : memmap vectors.o blinker04.clang.bc
	opt $(OOPS) blinker04.clang.bc -o blinker04.clang.opt.bc
	llc $(LLCOPS) blinker04.clang.opt.bc -o blinker04.clang.opt.s
	$(ARMGNU)-as blinker04.clang.opt.s -o blinker04.clang.opt.o
	$(ARMGNU)-ld -o blinker04.clang.opt.elf -T memmap vectors.o blinker04.clang.opt.o
	$(ARMGNU)-objdump -D blinker04.clang.opt.elf > blinker04.clang.opt.list
	$(ARMGNU)-objcopy blinker04.clang.opt.elf blinker04.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

