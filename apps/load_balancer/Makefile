# To build with SSL under windows, do:
# wine make load_balancer.exe SSL=openssl  # OpenSSL build
# wine make load_balancer.exe SSL=krypton  # Krypton build

PROG = load_balancer
SOURCES = $(PROG).c ../../fossa.c
CFLAGS = -W -Wall -pthread $(CFLAGS_EXTRA)

ifeq ($(SSL), openssl)
  OPENSSL_PATH = ./openssl-0.9.8
  CFLAGS_EXTRA += -DNS_ENABLE_SSL -I$(OPENSSL_PATH)/include
	CFLAGS_EXTRA += /link /libpath:$(OPENSSL_PATH)/lib ssleay32.lib libeay32.lib
endif

ifeq ($(SSL), krypton)
	KRYPTON_PATH = ../../../krypton
  CFLAGS_EXTRA += -DNS_ENABLE_SSL $(KRYPTON_PATH)/krypton.c -I$(KRYPTON_PATH)
endif

all: $(PROG)

$(PROG): $(SOURCES)
	$(CC) $(SOURCES) -o $@ $(CFLAGS)

$(PROG).exe: $(SOURCES)
	cl $(SOURCES) /I.. /MD /Fe$@ /DNS_ENABLE_THREADS advapi32.lib $(CFLAGS_EXTRA)

test: $(PROG)
	$(MAKE) -C ../api_server
	sh unit_test.sh $$(pwd)/$(PROG)

clean:
	rm -rf *.gc* *.dSYM *.exe *.obj *.o a.out $(PROG)
