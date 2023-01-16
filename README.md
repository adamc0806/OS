# README 

It's an operating system, it'll eventually play tetris natively targeting x86_64 architecture. Potential for porting to other architectures such as ARM.


# OS

To build:

- Assemble the bootloader with:
- i686-elf-as boot.s -o boot.o
- Compile the kernel with:
- i686-elf-gcc -c kernel.c -o kernel.o -std=gnu99 -ffreestanding -O2 -Wall -Wextra
- Link the kernel with:
- i686-elf-gcc -T linker.ld -o myos.bin -ffreestanding -O2 -nostdlib boot.o kernel.o -lgcc
- Verify multiboot:
- grub-file --is-x86-multiboot myos.bin
- if grub-file --is-x86-multiboot myos.bin; then
  echo multiboot confirmed
else
  echo the file is not multiboot
fi


- For bootable image(You can run the os.bin file, unless you want to run on hardware):
- mkdir -p isodir/boot/grub
cp myos.bin isodir/boot/myos.bin
cp grub.cfg isodir/boot/grub/grub.cfg
grub-mkrescue -o myos.iso isodir

- To run in QEMU(Requires full QEMU packages):
- qemu-system-i386 -cdrom myos.iso
- To run with os.bin file:
- qemu-system-i386 -kernel myos.bin
