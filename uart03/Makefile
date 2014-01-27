
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : uart03.gcc.bin

all : gcc clang

clang : uart03.clang.norm.bin uart03.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



uart03.gcc.o : uart03.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c uart03.c -o uart03.gcc.o

uart03.gcc.bin : memmap vectors.o uart03.gcc.o
	$(ARMGNU)-ld -o uart03.gcc.elf -T memmap vectors.o uart03.gcc.o
	$(ARMGNU)-objdump -D uart03.gcc.elf > uart03.gcc.list
	$(ARMGNU)-objcopy uart03.gcc.elf uart03.gcc.bin -O binary



uart03.clang.bc : uart03.c
	clang $(LOPS) -c uart03.c -o uart03.clang.bc

uart03.clang.norm.bin : memmap vectors.o uart03.clang.bc
	llc $(LLCOPS) uart03.clang.bc -o uart03.clang.norm.s
	$(ARMGNU)-as uart03.clang.norm.s -o uart03.clang.norm.o
	$(ARMGNU)-ld -o uart03.clang.norm.elf -T memmap vectors.o uart03.clang.norm.o
	$(ARMGNU)-objdump -D uart03.clang.norm.elf > uart03.clang.norm.list
	$(ARMGNU)-objcopy uart03.clang.norm.elf uart03.clang.norm.bin -O binary

uart03.clang.opt.bin : memmap vectors.o uart03.clang.bc
	opt $(OOPS) uart03.clang.bc -o uart03.clang.opt.bc
	llc $(LLCOPS) uart03.clang.opt.bc -o uart03.clang.opt.s
	$(ARMGNU)-as uart03.clang.opt.s -o uart03.clang.opt.o
	$(ARMGNU)-ld -o uart03.clang.opt.elf -T memmap vectors.o uart03.clang.opt.o
	$(ARMGNU)-objdump -D uart03.clang.opt.elf > uart03.clang.opt.list
	$(ARMGNU)-objcopy uart03.clang.opt.elf uart03.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

