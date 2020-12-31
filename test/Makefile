ROOTDIR ?= $(CURDIR)/../
TESTDIR	:= $(CURDIR)/

# common.mk stores some of main target's variables ($(CC), $(INCLUDEDIR), $(LIBDIR), ...).
include $(ROOTDIR)common.mk

TESTBINDIR		:= $(TESTDIR)bin/
TESTSRCDIR		:= $(TESTDIR)src/
TESTTXTDIR		:= $(TESTDIR)txt/

TESTBUILDDIR	:= $(TESTDIR)build/
TESTBUILDBINDIR	:= $(TESTBUILDDIR)bin/
TESTBUILDOBJDIR	:= $(TESTBUILDDIR)obj/
TESTBUILDTXTDIR	:= $(TESTBUILDDIR)txt/

TESTFRAMEWORKDIR := $(LIBDIR)Unity/src/
TESTFRAMEWORKSRC := $(TESTFRAMEWORKDIR)unity.c
TESTFRAMEWORKOBJ := $(TESTBUILDOBJDIR)unity.o

TESTSRC := $(shell find $(TESTSRCDIR) -name '*.c' -type f)
TESTTGT := $(patsubst $(TESTSRCDIR)%.c, $(TESTBUILDBINDIR)%,	 $(TESTSRC))
TESTTXT := $(patsubst $(TESTSRCDIR)%.c, $(TESTBUILDTXTDIR)%.txt, $(TESTSRC))

IGNOREDRESULTSCMD := grep -s IGNORE $(TESTTXTDIR)*.txt
FAILINGRESULTSCMD := grep -s FAIL $(TESTTXTDIR)*.txt
PASSINGRESULTSCMD := grep -s PASS $(TESTTXTDIR)*.txt

TESTINCLUDES := -I$(INCLUDEDIR) -I$(TESTSRCDIR) -I$(TESTFRAMEWORKDIR)
TESTCFLAGS	 := -std=c18 -Wall $(TESTINCLUDES)

.DEFAULT_GOAL : all

all : run_tests


run_tests : $(TESTTXT)
	@clear
	@echo "-----------------------IGNORES:-----------------------"
	@echo "$$($(IGNORERESULTSCMD))"
	@echo "-----------------------FAILURES:-----------------------"
	@echo "$$($(FAILINGRESULTSCMD))"
	@echo "-----------------------PASSED:-----------------------"
	@echo "$$($(PASSINGRESULTSCMD))"

$(TESTTXT) : $(TESTBUILDTXTDIR)%.txt : $(TESTBUILDBINDIR)% | $(TESTTXTDIR)
	mkdir -p $(dir $@)
	-$< 1>$@ 2>&1
	cp $@ $(TESTTXTDIR)

$(TESTTGT) : $(TESTBUILDBINDIR)% : $(TESTBUILDOBJDIR)%.o $(TESTFRAMEWORKOBJ) | $(TESTBINDIR)
	mkdir -p $(dir $@)
	$(CC) $(CFLAGS) $^ -o $@ $(LDFLAGS)
	cp $@ $(TESTBINDIR)


$(TESTBUILDOBJDIR)%.o : $(TESTSRCDIR)%.c
	mkdir -p $(dir $@)
	$(CC) -c $(TESTCFLAGS) $< -o $@

$(TESTFRAMEWORKOBJ) : $(TESTFRAMEWORKSRC)
	mkdir -p $(dir $@)
	$(CC) -c $(TESTCFLAGS) $< -o $@


$(TESTBINDIR) $(TESTINCLUDEDIR) $(TESTSRCDIR) $(TESTTXTDIR) :
	mkdir -p $@