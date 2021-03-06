THRIFT_ARCHIVE_URL = https://dist.apache.org/repos/dist/release/thrift/$(THRIFT_VERSION)/thrift-$(THRIFT_VERSION).tar.gz
TEST_ARTIFACTS = \
	test-compilation-stamp \
	test-configuration-stamp \
	test-installation-stamp \
	test-merge-stamp \
	test-stamp \
	thrift-$(THRIFT_VERSION) \
	thrift-$(THRIFT_VERSION).tar.gz \
	thrift-extraction-stamp \
	tree \
	tree-stamp

export THRIFT=$(CURDIR)/thrift-$(THRIFT_VERSION)
export THRIFT4GO=$(CURDIR)/../..

all: test

test: test-stamp

test-stamp: test-installation-stamp
	touch $@

test-merge-stamp: thrift-extraction-stamp
	cd $(THRIFT4GO) && bash ./scripts/merge_and_build.sh -b
	touch $@

test-configuration-stamp: test-merge-stamp
	cd $(THRIFT) && ./configure \
		--with-go \
		--without-c_glib \
		--without-erlang \
		--without-haskell \
		--without-java \
		--without-perl \
		--without-php \
		--without-python \
		--without-ruby
	touch $@

test-compilation-stamp: test-configuration-stamp
	cd $(THRIFT) && make
	touch $@

test-installation-stamp: test-compilation-stamp tree-stamp
	cd $(THRIFT) && DESTDIR=$(CURDIR)/tree make install
	touch $@

tree-stamp:
	mkdir -vp tree
	touch $@

thrift-$(THRIFT_VERSION).tar.gz:
	wget -c https://dist.apache.org/repos/dist/release/thrift/$(THRIFT_VERSION)/thrift-$(THRIFT_VERSION).tar.gz

thrift-extraction-stamp: thrift-$(THRIFT_VERSION).tar.gz
	tar xzvf thrift-$(THRIFT_VERSION).tar.gz
	touch $@

clean:
	rm -rf $(TEST_ARTIFACTS)

.PHONY: test
