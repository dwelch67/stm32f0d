
ARMGNU = arm-none-eabi

AOPS = --warn --fatal-warnings 
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding 

LOPS = -Wall -m32 -emit-llvm
LLCOPS = -march=thumb
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : stepper02.gcc.bin

all : gcc clang

clang : stepper02.clang.norm.bin stepper02.clang.opt.bin 

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o



stepper02.gcc.o : stepper02.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c stepper02.c -o stepper02.gcc.o

stepper02.gcc.bin : memmap vectors.o stepper02.gcc.o
	$(ARMGNU)-ld -o stepper02.gcc.elf -T memmap vectors.o stepper02.gcc.o
	$(ARMGNU)-objdump -D stepper02.gcc.elf > stepper02.gcc.list
	$(ARMGNU)-objcopy stepper02.gcc.elf stepper02.gcc.bin -O binary



stepper02.clang.bc : stepper02.c
	clang $(LOPS) -c stepper02.c -o stepper02.clang.bc

stepper02.clang.norm.bin : memmap vectors.o stepper02.clang.bc
	llc $(LLCOPS) stepper02.clang.bc -o stepper02.clang.norm.s
	$(ARMGNU)-as stepper02.clang.norm.s -o stepper02.clang.norm.o
	$(ARMGNU)-ld -o stepper02.clang.norm.elf -T memmap vectors.o stepper02.clang.norm.o
	$(ARMGNU)-objdump -D stepper02.clang.norm.elf > stepper02.clang.norm.list
	$(ARMGNU)-objcopy stepper02.clang.norm.elf stepper02.clang.norm.bin -O binary

stepper02.clang.opt.bin : memmap vectors.o stepper02.clang.bc
	opt $(OOPS) stepper02.clang.bc -o stepper02.clang.opt.bc
	llc $(LLCOPS) stepper02.clang.opt.bc -o stepper02.clang.opt.s
	$(ARMGNU)-as stepper02.clang.opt.s -o stepper02.clang.opt.o
	$(ARMGNU)-ld -o stepper02.clang.opt.elf -T memmap vectors.o stepper02.clang.opt.o
	$(ARMGNU)-objdump -D stepper02.clang.opt.elf > stepper02.clang.opt.list
	$(ARMGNU)-objcopy stepper02.clang.opt.elf stepper02.clang.opt.bin -O binary



clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

