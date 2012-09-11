
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : uart02.gcc.bin

all : gcc clang

clang : uart02.clang.norm.bin uart02.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



uart02.gcc.o : uart02.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c uart02.c -o uart02.gcc.o

uart02.gcc.bin : memmap vectors.o uart02.gcc.o
	$(ARMGNU)-ld -o uart02.gcc.elf -T memmap vectors.o uart02.gcc.o
	$(ARMGNU)-objdump -D uart02.gcc.elf > uart02.gcc.list
	$(ARMGNU)-objcopy uart02.gcc.elf uart02.gcc.bin -O binary



uart02.clang.bc : uart02.c
	clang $(LOPS) -c uart02.c -o uart02.clang.bc

uart02.clang.norm.bin : memmap vectors.o uart02.clang.bc
	llc $(LLCOPS) uart02.clang.bc -o uart02.clang.norm.s
	$(ARMGNU)-as uart02.clang.norm.s -o uart02.clang.norm.o
	$(ARMGNU)-ld -o uart02.clang.norm.elf -T memmap vectors.o uart02.clang.norm.o
	$(ARMGNU)-objdump -D uart02.clang.norm.elf > uart02.clang.norm.list
	$(ARMGNU)-objcopy uart02.clang.norm.elf uart02.clang.norm.bin -O binary

uart02.clang.opt.bin : memmap vectors.o uart02.clang.bc
	opt $(OOPS) uart02.clang.bc -o uart02.clang.opt.bc
	llc $(LLCOPS) uart02.clang.opt.bc -o uart02.clang.opt.s
	$(ARMGNU)-as uart02.clang.opt.s -o uart02.clang.opt.o
	$(ARMGNU)-ld -o uart02.clang.opt.elf -T memmap vectors.o uart02.clang.opt.o
	$(ARMGNU)-objdump -D uart02.clang.opt.elf > uart02.clang.opt.list
	$(ARMGNU)-objcopy uart02.clang.opt.elf uart02.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

