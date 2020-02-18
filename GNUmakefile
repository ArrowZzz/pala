# Make sure the working directory is under $GOPATH\src 

#-include Local.mk
TOP_DIR := $(shell pwd)
GOBIN := $(TOP_DIR)/bin
TARGET := pala

PREFERRED_INTERACTIVE_SHELL ?= bash
GO ?= go
GO_TEST_ARGS ?= "-count=1"

# Make the default target (first target) to all.
default: all

.PHONY: all
all: ${TARGET} test

PALA_PKGS := $(shell GOPATH=$(GOPATH) $(GO) list ./... | grep -v consensus_test | grep -v network_test )
PALA_ALL_PKGS := $(shell GOPATH=$(GOPATH) $(GO) list ./...)

.PHONY: $(TARGET)
$(TARGET):
	@echo "Building $(GOFUN_SRC) to ./bin"
	@GOPATH=$(GOPATH) GOBIN=$(GOBIN) $(GO) install $(PALA_PKGS)

.PHONY: test
test:
	$(GO) test -cover $(GO_TEST_ARGS) $(PALA_ALL_PKGS)

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

.PHONY: vet
vet:
	$(GO) vet $(PALA_ALL_PKGS)	

.PHONY: clean
clean:
	rm -rf pkg/ bin/
