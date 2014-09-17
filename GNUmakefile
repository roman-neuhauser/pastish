# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=         /usr/local
BINDIR ?=         $(PREFIX)/bin
MANDIR ?=         $(PREFIX)/share/man
MAN1DIR ?=        $(MANDIR)/man1

GZIPCMD ?=        gzip
INSTALL_DATA ?=   install -m 644
INSTALL_DIR ?=    install -m 755 -d
INSTALL_SCRIPT ?= install -m 755
RST2HTML ?=       $(call first_in_path,rst2html.py rst2html)

name =            pastish

installed =       $(name).1.gz $(name)
artifacts =       $(installed) README.html

wc:
	git submodule init
	git submodule update

all: $(artifacts)

most: $(installed)

all most:
	@touch .built

clean:
	$(RM) .buit $(artifacts)

check: $(name)
	SHELL=$(SHELL) $(SHELL) rnt/run-tests.sh tests $$PWD/$(name)

$(name): $(name).in
	$(INSTALL_SCRIPT) $< $@

html: README.html

%.html: %.rest
	$(RST2HTML) $< $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: .built
	$(INSTALL_DIR) $(DESTDIR)$(BINDIR)
	$(INSTALL_DIR) $(DESTDIR)$(MAN1DIR)
	$(INSTALL_SCRIPT) $(name) $(DESTDIR)$(BINDIR)/$(name)
	$(INSTALL_DATA) $(name).1.gz $(DESTDIR)$(MAN1DIR)/$(name).1.gz

.built:
	@printf "%s\n" '' "ERROR: run '$(MAKE) all' first." '' >&2
	@false

define first_in_path
  $(or \
    $(firstword $(wildcard \
      $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
    )) \
  , $(error Need one of: $(1)) \
  )
endef

.DEFAULT_GOAL := most

.PHONY: all
.PHONY: check
.PHONY: clean
.PHONY: html
.PHONY: install
.PHONY: most
.PHONY: wc
