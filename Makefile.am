#
# Percona Playback
#
### BEGIN LICENSE
# Copyright (C) 2011 Stewart Smith <stewart@flamingspork.com>
# This program is free software: you can redistribute it and/or modify it 
# under the terms of the GNU General Public License version 3, as published 
# by the Free Software Foundation.
# 
# This program is distributed in the hope that it will be useful, but 
# WITHOUT ANY WARRANTY; without even the implied warranties of 
# MERCHANTABILITY, SATISFACTORY QUALITY, or FITNESS FOR A PARTICULAR 
# PURPOSE.  See the GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License along 
# with this program.  If not, see <http://www.gnu.org/licenses/>.
### END LICENSE

ACLOCAL_AMFLAGS = -I m4
LDADD = libpercona_playback.la


bin_PROGRAMS =
check_PROGRAMS =
noinst_PROGRAMS =
lib_LTLIBRARIES =
noinst_LTLIBRARIES =
nobase_include_HEADERS =
noinst_HEADERS =
pkgplugin_LTLIBRARIES =
CLEANFILES =
DISTCLEANFILES = \
  config/top.h
EXTRA_DIST = \
  docs/Doxyfile \
  docs/header.html.in \
  test_run.sh

include config/pandora-plugin.am

#
# libpercona_playback
#

lib_LTLIBRARIES += libpercona_playback.la

libpercona_playback_la_SOURCES = \
  percona_playback/percona_playback.cc

nobase_include_HEADERS += \
  percona_playback/percona_playback.h \
  percona_playback/visibility.h

libpercona_playback_la_CXXFLAGS = \
	$(AM_CXXFLAGS) \
	-DBUILDING_PERCONA_PLAYBACK

libpercona_playback_la_LIBADD = $(LIBDL_LIBS) $(BOOST_LIBS) $(TBB_LIBS)
libpercona_playback_la_LDFLAGS = $(AM_LDFLAGS) -version-info $(LIBPERCONA_PLAYBACK_VERSION) $(BOOST_LIBS) $(TBB_LIBS) $(pandora_plugin_libs)
libpercona_playback_la_DEPENDENCIES = ${noinst_LTLIBRARIES}

check_PROGRAMS += \
  percona_playback/test/basic

#
# Simple percona_playback application
#

bin_PROGRAMS += bin/percona_playback
bin_percona_playback_SOURCES = bin/percona_playback.cc
bin_percona_playback_LDADD= $(BOOST_LIBS) $(LDADD) $(TBB_LIBS) $(MYSQL_LIBS) $(pandora_plugin_libs) $(PANDORA_DYNAMIC_LDADDS)

TESTS = $(check_PROGRAMS)

TESTS_ENVIRONMENT = ${top_srcdir}/test_run.sh

check-verbose:
	@PERCONA_PLAYBACK_TEST_ARGS="v" make check

check-debug:
	@PERCONA_PLAYBACK_TEST_ARGS="vvv" make check

check-valgrind:
	@PERCONA_PLAYBACK_TEST_PREFIX="valgrind -q --leak-check=full --show-reachable=yes --suppressions=valgrind.suppressions" make check

check-valgrind-debug:
	@PERCONA_PLAYBACK_TEST_PREFIX="valgrind -q --leak-check=full --show-reachable=yes --suppressions=valgrind.suppressions" PERCONA_PLAYBACK_TEST_ARGS="vvv" make check

check-gdb:
	@PERCONA_PLAYBACK_TEST_PREFIX="gdb -q" make check

check-time:
	@PERCONA_PLAYBACK_TEST_PREFIX="/usr/bin/time" make check

check-strace:
	@PERCONA_PLAYBACK_TEST_PREFIX="strace -c" make check

check-truss:
	@PERCONA_PLAYBACK_TEST_PREFIX="truss -c" make check

docs: all
	@echo "/**" > docs/mainpage.h
	@echo "@mainpage README" >> docs/mainpage.h
	@echo "<pre>" >> docs/mainpage.h
	@cat ${top_srcdir}/README >> docs/mainpage.h
	@echo "</pre>" >> docs/mainpage.h
	@echo "*/" >> docs/mainpage.h
	${DOXYGEN} ${top_srcdir}/docs/Doxyfile
	@rm docs/mainpage.h