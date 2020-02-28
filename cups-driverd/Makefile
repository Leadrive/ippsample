
include ../Makedefs

TARGETS = cups-driverd

all: $(TARGETS)

install: install-exec

install-exec:
	$(INSTALL_DIR) $(DESTDIR)/usr/lib/cups/daemon
	for file in $(TARGETS); do \
		echo "Installing " $$file $(DESTDIR)/usr/lib/cups/daemon; \
		$(INSTALL_BIN) $$file $(DESTDIR)/usr/lib/cups/daemon; \
	done
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(TARGETS); do \
			cp $$file $(SYMROOT); \
			dsymutil $(SYMROOT)/$$file; \
		done \
	fi

clean:
	$(RM) *.o
	$(RM) cups-driverd

uninstall:
	for file in $(TARGETS); do \
		$(RM) $(DESTDIR)/usr/lib/cups/daemon/$$file; \
	done

cups-driverd.o: cups-driverd.cxx
	$(CXX) -I.. -DCUPS_CACHEDIR=\"$(prefix)/var/cups/\" -c cups-driverd.cxx

util.o: util.c
	$(CC) -I.. -c util.c

cups-driverd: cups-driverd.o ../ppdc/libcupsppdc.a ../cups/$(LIBCUPS) util.o
	$(CXX) $(ALL_LDFLAGS) -o cups-driverd cups-driverd.o ../ppdc/libcupsppdc.a util.o $(COMMONLIBS) $(LINKCUPS)