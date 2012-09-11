
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : uart01.gcc.bin

all : gcc clang

clang : uart01.clang.norm.bin uart01.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



uart01.gcc.o : uart01.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c uart01.c -o uart01.gcc.o

uart01.gcc.bin : memmap vectors.o uart01.gcc.o
	$(ARMGNU)-ld -o uart01.gcc.elf -T memmap vectors.o uart01.gcc.o
	$(ARMGNU)-objdump -D uart01.gcc.elf > uart01.gcc.list
	$(ARMGNU)-objcopy uart01.gcc.elf uart01.gcc.bin -O binary



uart01.clang.bc : uart01.c
	clang $(LOPS) -c uart01.c -o uart01.clang.bc

uart01.clang.norm.bin : memmap vectors.o uart01.clang.bc
	llc $(LLCOPS) uart01.clang.bc -o uart01.clang.norm.s
	$(ARMGNU)-as uart01.clang.norm.s -o uart01.clang.norm.o
	$(ARMGNU)-ld -o uart01.clang.norm.elf -T memmap vectors.o uart01.clang.norm.o
	$(ARMGNU)-objdump -D uart01.clang.norm.elf > uart01.clang.norm.list
	$(ARMGNU)-objcopy uart01.clang.norm.elf uart01.clang.norm.bin -O binary

uart01.clang.opt.bin : memmap vectors.o uart01.clang.bc
	opt $(OOPS) uart01.clang.bc -o uart01.clang.opt.bc
	llc $(LLCOPS) uart01.clang.opt.bc -o uart01.clang.opt.s
	$(ARMGNU)-as uart01.clang.opt.s -o uart01.clang.opt.o
	$(ARMGNU)-ld -o uart01.clang.opt.elf -T memmap vectors.o uart01.clang.opt.o
	$(ARMGNU)-objdump -D uart01.clang.opt.elf > uart01.clang.opt.list
	$(ARMGNU)-objcopy uart01.clang.opt.elf uart01.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

