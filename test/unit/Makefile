BINDIR := bin
BINS=$(patsubst %.c,$(BINDIR)/%,$(wildcard *.c))
AUX_BINS=$(patsubst %.c,$(BINDIR)/%,$(wildcard auxiliary/*.c))
LDFLAGS += $(shell pkg-config --libs rz_core)
CFLAGS += $(shell pkg-config --cflags rz_core) -g
ifeq ($(ASAN),1)
	ASAN_LD_PRELOAD=LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libasan.so.5
else
	ASAN_LD_PRELOAD=
endif

all: $(BINS) $(AUX_BINS)

$(BINDIR)/%: %.c
	mkdir -p "$(BINDIR)"
	mkdir -p "$(BINDIR)"/auxiliary
	$(CC) $< -o $@ $(CFLAGS) $(LDFLAGS)

run:
	r=0 ; cd .. ; for a in $(BINS) ; do $(ASAN_LD_PRELOAD) ./unit/$$a || r=1; done ; echo unit$$r ; exit $$r

clean:
	rm -f $(BINS)
	rm -fd "$(BINDIR)"

.PHONY: all
