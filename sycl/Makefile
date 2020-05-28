all: sycl cuda

sycl:
	make -f ../build/Makefile.sycl -C sycl all

cuda:
	make -f ../build/Makefile.cuda -C src all

clean:
	make -f ../build/Makefile.sycl -C sycl clean
	make -f ../build/Makefile.cuda -C src  clean
