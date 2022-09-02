default: althttpd althttpsd 
VERSION_NUMBER = 2.0
CC=cc
CFLAGS=-Os -Wall -Wextra

manifest:
	@if which fossil > /dev/null; then \
		fossil update --nosync current; \
	else \
	  echo "fossil binary not found. Version hash/time might be incorrect."
	fi
manifest.uuid: manifest

# We do the version-setting CPPFLAGS this way, instead of via
# $(shell...), for the sake of portability with BSD Make. Some hoops
# have to be jumped through to get the escaping "just right," though.
version: Makefile manifest.uuid
	@hash=`cut -c1-12 manifest.uuid`; \
	time=`sed -n 2p manifest | cut -d' ' -f2`; \
	{ echo -n "ALTHTTPD_VERSION=\""; \
		echo '$(VERSION_NUMBER)' "[$$hash] [$$time]\""; \
	} > $@

althttpd:	althttpd.c version
	@flags="`cat version`"; set -x; \
	$(CC) $(CFLAGS) "-D$$flags" -o althttpd althttpd.c

althttpsd:	althttpd.c version
	@flags="`cat version`"; set -x; \
	$(CC) $(CFLAGS) "-D$$flags" -fPIC -o althttpsd -DENABLE_TLS althttpd.c -lssl -lcrypto

static-althttpd:	althttpd.c version
	@flags="`cat version`"; set -x; \
	$(CC) $(CFLAGS) "-D$$flags" -static -o althttpd althttpd.c

static-althttpsd:	althttpd.c version
	@flags="`cat version`"; set -x; \
	$(CC) $(CFLAGS) "-D$$flags" -static -fPIC -o althttpsd -DENABLE_TLS althttpd.c -lssl -lcrypto -lpthread -ldl

clean:	
	rm -f althttpd althttpsd version
