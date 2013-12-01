
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : stepper01.gcc.bin

all : gcc clang

clang : stepper01.clang.norm.bin stepper01.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



stepper01.gcc.o : stepper01.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c stepper01.c -o stepper01.gcc.o

stepper01.gcc.bin : memmap vectors.o stepper01.gcc.o
	$(ARMGNU)-ld -o stepper01.gcc.elf -T memmap vectors.o stepper01.gcc.o
	$(ARMGNU)-objdump -D stepper01.gcc.elf > stepper01.gcc.list
	$(ARMGNU)-objcopy stepper01.gcc.elf stepper01.gcc.bin -O binary



stepper01.clang.bc : stepper01.c
	clang $(LOPS) -c stepper01.c -o stepper01.clang.bc

stepper01.clang.norm.bin : memmap vectors.o stepper01.clang.bc
	llc $(LLCOPS) stepper01.clang.bc -o stepper01.clang.norm.s
	$(ARMGNU)-as stepper01.clang.norm.s -o stepper01.clang.norm.o
	$(ARMGNU)-ld -o stepper01.clang.norm.elf -T memmap vectors.o stepper01.clang.norm.o
	$(ARMGNU)-objdump -D stepper01.clang.norm.elf > stepper01.clang.norm.list
	$(ARMGNU)-objcopy stepper01.clang.norm.elf stepper01.clang.norm.bin -O binary

stepper01.clang.opt.bin : memmap vectors.o stepper01.clang.bc
	opt $(OOPS) stepper01.clang.bc -o stepper01.clang.opt.bc
	llc $(LLCOPS) stepper01.clang.opt.bc -o stepper01.clang.opt.s
	$(ARMGNU)-as stepper01.clang.opt.s -o stepper01.clang.opt.o
	$(ARMGNU)-ld -o stepper01.clang.opt.elf -T memmap vectors.o stepper01.clang.opt.o
	$(ARMGNU)-objdump -D stepper01.clang.opt.elf > stepper01.clang.opt.list
	$(ARMGNU)-objcopy stepper01.clang.opt.elf stepper01.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

