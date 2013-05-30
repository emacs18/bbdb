dnl configure.ac --- configuration setup for BBDB

dnl Author:        Didier Verna <didier@xemacs.org>
dnl Maintainer:    Roland Winkler <winkler@gnu.org>
dnl Created:       Tue Nov 14 18:28:52 2000
dnl Last Revision: $Date: 2013/05/26 17:45:59 $

dnl Copyright (C) 2000-2001 Didier Verna <didier@xemacs.org>
dnl Copyright (C) 2011-2013 Roland Winkler <winkler@gnu.org>

dnl BBDB is free software: you can redistribute it and/or modify
dnl it under the terms of the GNU General Public License as published by
dnl the Free Software Foundation, either version 3 of the License, or
dnl (at your option) any later version.
dnl
dnl BBDB is distributed in the hope that it will be useful,
dnl but WITHOUT ANY WARRANTY; without even the implied warranty of
dnl MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
dnl GNU General Public License for more details.
dnl
dnl You should have received a copy of the GNU General Public License
dnl along with BBDB.  If not, see <http://www.gnu.org/licenses/>.

dnl Process this file with autoconf to produce a new configure script

AC_PREREQ([2.64])

AC_INIT([bbdb],[3.02.2013.05.26],[bbdb-info@lists.sourceforge.net],[],[http://savannah.nongnu.org/projects/bbdb/])

AC_COPYRIGHT([Copyright (C) 2000-2001 Didier Verna <didier@xemacs.org>.
Copyright (C) 2011-2013 Roland Winkler <winkler@gnu.org>.])
AC_REVISION([$Revision: 1.16 $])

AC_CONFIG_SRCDIR([lisp/bbdb.el])
AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE([foreign])

AM_SILENT_RULES

# AM_PATH_LISPDIR sets $EMACS to the emacs executable, as needed by
# BBDB_WITH_VM, so it must come first.
AM_PATH_LISPDIR
AS_IF([test "$EMACS" = no], [AC_MSG_ERROR([cannot find Emacs])])

BBDB_WITH_VM
if test "$VMDIR" = no; then
  AC_MSG_WARN([VM mailer sources not found.
      Will not create autoloads for, compile, or install VM support.])
fi
AM_CONDITIONAL([HAVE_VM], test "$VMDIR" != no)

if test x"$texmfdir" = x; then
  AC_MSG_WARN([texmfdir not set, TeX files will not be installed.
      For potential values for texmfdir, run: kpsewhich -show-path .tex])
fi
AC_SUBST(texmfdir)
AM_CONDITIONAL([HAVE_TEXMF], test x"${texmfdir}" != x)

AC_CONFIG_FILES([Makefile
                 lisp/Makefile
                 doc/Makefile
                 tex/Makefile
		 lisp/bbdb-version.el])

AC_OUTPUT
