# Makefile for Cstool of Capstone Disassembly Engine

include ../functions.mk

.PHONY: clean all

LIBNAME = capstone

ifndef BUILDDIR
BLDIR = .
OBJDIR = .
CAPSTONEDIR = ..
else
BLDIR = $(abspath $(BUILDDIR))
OBJDIR = $(BLDIR)/obj
CAPSTONEDIR = $BLDIR
endif


CFLAGS = -I../include
LDFLAGS = -O3 -Wall -L.. -l$(LIBNAME)

TARGET = cstool
SOURCES := $(wildcard *.c)
OBJECTS := $(addprefix $OBJDIR/,$(SOURCES:.c=.o))

LIBCAPSTONE = libcapstone.a

IS_CYGWIN := $(shell $(CC) -dumpmachine 2>/dev/null | grep -i cygwin | wc -l)
ifeq ($(IS_CYGWIN),1)
LIBCAPSTONE = capstone.lib
else
IS_MINGW := $(shell $(CC) --version 2>/dev/null | grep -i mingw | wc -l)
ifeq ($(IS_MINGW),1)
LIBCAPSTONE = capstone.lib
endif
endif

all: $(TARGET)

$(TARGET): $(CAPSTONEDIR)$(LIBCAPSTONE) $(OBJECTS)
ifeq ($(V),0)
	$(call log,LINK,$@)
	@${CC} $(OBJECTS) $(LDFLAGS) -o $@
else
	${CC} $(OBJECTS) $(LDFLAGS) -o $@
endif

clean:
	${RM} -rf $(OBJECTS) $(TARGET)

$(OBJDIR)/%.o: %.c
ifeq ($(V),0)
	$(call log,CC,$(@:$(OBJDIR)/%=%))
	@${CC} $(CFLAGS) -c $< -o $@
else
	${CC} $(CFLAGS) -c $< -o $@
endif
