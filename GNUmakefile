# Make sure the working directory is under $GOPATH\src 

#-include Local.mk

PREFERRED_INTERACTIVE_SHELL ?= bash
GO ?= go
GO_TEST_ARGS ?= "-count=1"

PALA_PKGS := $(shell GOPATH=$(GOPATH) $(GO) list ./...)
.PHONY: test
test:
	$(GO) test $(GO_TEST_ARGS) $(PALA_PKGS)

.PHONY: dep
dep: Gopkg.dep

%.dep: %.toml
	@md5sum $< > $@.cur
	@cmp -s $@.cur $@ || ( cd $(@D); echo "Updating $@" && dep ensure ); mv -f $@.cur $@

.PHONY: clean-dep
clean-dep:
	rm -rf ./vendor/ ./Gopkg.dep ./Gopkg.lock

.PHONY: lint
lint:
	golangci-lint run 2>golangci-lint.log

.PHONY: clean
clean:
	rm -rf pkg/
