DMDPARAMS = -m32 -Iinclude
ASPARAMS = --32
LDPARAMS = -melf_i386

objects = loader.o kernel.o

%.o: %.d
	dmd $(DMDPARAMS) -of$@ -c $<

%.o: %.s
	as $(ASPARAMS) -o $@ $<

dkernel.bin: linker.ld $(objects)
	ld $(LDPARAMS)  -T $< -o $@ $(objects)

install: dkernel.bin
	sudo cp $< /boot/dkernel.bin
	
dkernel.iso: dkernel.bin
	mkdir iso
	mkdir iso/boot
	mkdir iso/boot/grub
	cp $< iso/boot/
	echo 'set timeout=0'                      > iso/boot/grub/grub.cfg
	echo 'set default=0'                     >> iso/boot/grub/grub.cfg
	echo ''                                  >> iso/boot/grub/grub.cfg
	echo 'menuentry "D_OS Sociomantic Zaheer" {' >> iso/boot/grub/grub.cfg
	echo '  multiboot /boot/dkernel.bin'    >> iso/boot/grub/grub.cfg
	echo '  boot'                            >> iso/boot/grub/grub.cfg
	echo '}'                                 >> iso/boot/grub/grub.cfg
	grub-mkrescue --output=dkernel.iso iso
	rm -rf iso
	
.PHONY: clean
clean:
	rm -rf obj dkernel.bin dkernel.iso
