# vim:set et ts=4:
#
# ibus-handwrite - The Ibus handwrite engine
#
# Copyright (c) 2007-2008 Huang Peng <shawn.p.huang@gmail.com>
# Copyright (c) 2010 microcai <microcai@fedoraproject.org>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.

# if not 1, append datestamp to the version number.
m4_define([package_name], [ibus-handwrite])
    
AC_PREREQ([2.53])
AC_INIT([package_name], [2.1.4], [http://code.google.com/p/ibus/issues/entry], [package_name])
AM_INIT_AUTOMAKE([-Wall])
AC_GNU_SOURCE

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# define PACKAGE_VERSION_* variables
AC_PROG_CC
AC_ISC_POSIX
AC_HEADER_STDC

PKG_PROG_PKG_CONFIG

AC_ARG_WITH(debian,[AC_HELP_STRING([--with-debian],[enable debian specific build])],[],[])

AC_ARG_ENABLE(zinnia,AC_HELP_STRING([--enable-zinnia],[build with zinnia engine]),[],[enable_zinnia=yes])

AC_SUBST(rpm_sepc_with_tomoe,"")
AC_SUBST(zinnia_tomoe,"")
AC_SUBST(zinnia,"")

AC_ARG_WITH(zinnia-tomoe,[AC_HELP_STRING([--with-zinnia-tomoe=[[dir]]],[specify the dir that hold zinnia-tomoe data file])],
	[
		tomoedir=${withval}
		
		rpm_sepc_with_tomoe="--with-zinnia-tomoe=${withval}"		
		zinnia_tomoe="zinnia-tomoe"	
		zinnia="zinnia"
		
		AC_DEFINE_UNQUOTED(TOMOE_DATA_DIR,"${tomoedir}",[the dir that hold zinnia-tomoe data file])
	],[
			AS_IF([test "x${enable_zinnia}" = "xyes"],
			[AC_MSG_WARN(zinnia engine disabled due to the missing --with-zinnia-tomoe)])	
			enable_zinnia=no;
])


# define GETTEXT_* variables
GETTEXT_PACKAGE="$PACKAGE_NAME"
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE,"$GETTEXT_PACKAGE", [Define to the read-only architecture-independent data directory.])

AS_IF([test "x${enable_zinnia}" = "xyes"],
	[
		AC_CHECK_LIB(zinnia,[zinnia_recognizer_open],[],[have_zinnia=no])
		AC_CHECK_LIB(zinnia,[zinnia_recognizer_open],[have_zinnia=yes],[have_zinnia=no])
	],
	[have_zinnia=no]
)

AS_IF([test "x${have_zinnia}" = "xyes" ],[
		AM_CONDITIONAL(WITH_ZINNIA,[true])
	],[
		AM_CONDITIONAL(WITH_ZINNIA,[false])		
])

# check ibus
PKG_CHECK_MODULES(IBUS, [
    ibus-1.0 >= 1.3
])

PKG_CHECK_MODULES(GTK2, [
    gtk+-2.0 >= 2.10
])

PKG_CHECK_MODULES(GTKGL, [
    gtkglext-1.0
])

PKG_CHECK_MODULES(ALL_NEED, [
    ibus-1.0 >= 1.2 gtkglext-1.0
])

AM_GNU_GETTEXT([external])
AM_GNU_GETTEXT_VERSION(0.16.1)

#AC_DEFINE_UNQUOTED(ICONDIR,"${sharedstatedir}/ibus/icons",[icon filedir])

AC_CHECK_DECL(MAP_ANONYMOUS,[],
	[
		AC_DEFINE(MAP_ANONYMOUS,MAP_ANON,[Define this because some platform only define MAP_ANON])
	],
	[[
		#include <sys/mman.h>
	]])


AC_DEFINE(MICROCAI_WITHEMAIL,"microcai@fedoraproject.org",[author microcai and his email])
# OUTPUT files
AC_CONFIG_FILES(po/Makefile.in
Makefile
ibus-handwrite.spec
src/Makefile
src/handwrite-zh.xml.in
src/handwrite-jp.xml.in
icons/Makefile
data/Makefile
doc/Makefile
)

AC_OUTPUT
