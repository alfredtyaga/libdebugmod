# Hey Emacs, this is a -*- GNUmakefile -*-

# Copyright (C) 2014  Andre Colomb
#
# This file is part of libdebugmod.
#
# libdebugmod is free software: you can redistribute it and/or modify
# it under the terms of the GNU Lesser General Public License as
# published by the Free Software Foundation, either version 3 of the
# License, or (at your option) any later version.
#
# libdebugmod is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with this program.  If not, see
# <http://www.gnu.org/licenses/>.


# Definition of target file names
OBJ = debug_mod.o
LIB = libdebugmod.a
TESTBIN = test_debug_mod test_incremental_search

# Default compilation flags useful for code dump, can be changed from command line
CFLAGS = -O1 -g
# Additional flags, always used
override CFLAGS += -std=c99 -Wall -Wextra -Wstrict-prototypes -Werror
override CPPFLAGS += -I../include
ARFLAGS += -U

RANLIB = ranlib
OBJDUMP = objdump
ECHO = /bin/echo

# Default target: Compile native library
lib: $(LIB)

clean:
	$(RM) $(TESTBIN) $(LIB) $(OBJ)

dump: test_debug_mod
	$(OBJDUMP) -dS $< #-j .text

test: test_debug_mod
	./$<

test-enabled: CPPFLAGS += -DDEBUG_MOD_ENABLE
test-enabled: CPPFLAGS += -DDEBUG_MOD_MAX=10
test-enabled: test

test-full: CPPFLAGS += -DDEBUG_MOD_DYNAMIC -DDEBUG_MOD_SAVE
test-full: test-enabled

test-search: test_incremental_search
	$(ECHO) -e "fail\nfoo\nbar\nfrob\nfrobnicate\nfrog\nfa\nfar\nfoofoo\nfarfalle" \
		| ./$<


# Compile native test binary for build architecture and run test
host: clean test-full test-search

# Compile as native library for build architecture
avr: CC = avr-gcc
avr: RANLIB = avr-ranlib
avr: OBJDUMP = avr-objdump
avr: clean lib dump

.PHONY: lib clean dump test host avr


# Build targets follow

$(LIB): $(LIB)($(OBJ))
	$(RANLIB) $@

test_debug_mod: test_debug_mod.c test_ext_module.c $(LIB)

test_incremental_search: CPPFLAGS += -DDEBUG_MOD_ENABLE
test_incremental_search: CPPFLAGS += -DDEBUG_MOD_SAVE
test_incremental_search: CPPFLAGS += -DDEBUG_MOD_MAX=10
test_incremental_search: test_incremental_search.c $(LIB)
