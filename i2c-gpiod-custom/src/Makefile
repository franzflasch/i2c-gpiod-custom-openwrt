KERNEL ?= /lib/modules/$(shell uname -r)/build
obj-m += i2c-gpiod-custom.o

all:
		make -C $(KERNEL) M=$(PWD) modules

clean:
		make -C $(KERNEL) M=$(PWD) clean
