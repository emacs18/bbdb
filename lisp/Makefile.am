# lisp/Makefile.am for BBDB
#
# Copyright (C) 2010-2023 Roland Winkler <winkler@gnu.org>

# Author: Roland Winkler <winkler@gnu.org>
#  	  Christian Egli <christian.egli@sbs.ch>
# 
# This file is part of the Insidious Big Brother Database (aka BBDB),
# 
# BBDB is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# BBDB is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with BBDB.  If not, see <http://www.gnu.org/licenses/>.

# --batch implies --no-init-file, yet let's be explicit about what we want
AM_ELCFLAGS += --no-init-file --no-site-file

dist_lisp_LISP = 				\
	bbdb.el					\
	bbdb-anniv.el				\
	bbdb-com.el				\
	bbdb-gnus.el				\
	bbdb-gnus-aux.el			\
	bbdb-ispell.el				\
	bbdb-message.el				\
	bbdb-mhe.el				\
	bbdb-migrate.el				\
	bbdb-mua.el				\
	bbdb-pgp.el				\
	bbdb-tex.el				\
	bbdb-rmail.el				\
	bbdb-sc.el				\
	bbdb-snarf.el				\
	bbdb-site.el

if VM
dist_lisp_LISP += bbdb-vm.el
dist_lisp_LISP += bbdb-vm-aux.el
# We use the Automake variable AM_ELCFLAGS to include the VM lisp directory
# in the Emacs load path when compiling BBDB with VM support.
endif

if MU4E
dist_lisp_LISP += bbdb-mu4e.el
# We use the Automake variable AM_ELCFLAGS to include the Mu4e lisp directory
# in the Emacs load path when compiling BBDB with Mu4e support.
endif

if WL
dist_lisp_LISP += bbdb-wl.el
# We use the Automake variable AM_ELCFLAGS to include the WL lisp directory
# in the Emacs load path when compiling BBDB with WL support.
endif

if NOTMUCH
dist_lisp_LISP += bbdb-notmuch.el
# We use the Automake variable AM_ELCFLAGS to include the Notmuch lisp directory
# in the Emacs load path when compiling BBDB with Notmuch support.
endif

lisp_DATA = bbdb-loaddefs.el

MOSTLYCLEANFILES = bbdb-loaddefs.el
CLEANFILES = bbdb-site.el bbdb-pkg.el

# The Emacs function define-package is not autoloaded.
# So we assume we need not compile bbdb-pkg.el (which otherwise
# results in a compiler warning that define-package is not defined).
EXTRA_DIST = bbdb-site.el.in bbdb-pkg.el makefile-temp

bbdb-loaddefs.el: $(dist_lisp_LISP)
#	2011-12-11: We switched from bbdb-autoloads.el to bbdb-loaddefs.el.
#	If the user still has an old bbdb-autoloads.el in the BBDB
#	lisp directory (and keeps loading it from the emacs init file),
#	we might get strange error messages that things fail.
#	So we throw an error if these old files are found.
	@if test -f bbdb-autoloads.el -o -f bbdb-autoloads.elc; then \
	  (echo "*** ERROR: Old file(s) \`bbdb-autoloads.el(c)' found ***" ; \
	  echo "*** Delete these files; do not load them from your init file ***") && \
	  false ; \
	fi
	@echo "(provide 'bbdb-loaddefs)" > $@;
	@echo "(if (and load-file-name (file-name-directory load-file-name))" >> $@;
	@echo "    (add-to-list 'load-path (file-name-directory load-file-name)))" >> $@;
	@echo "" >> $@;
#	Generated autoload-file must have an absolute path,
#	$srcdir can be relative.
#
#       The equivalent command line with bash $(...) construct is:
#       abs_target=$$($(CYGPATH_W) $(abs_builddir)/$@ | sed 's/\\/\\\\/g');
#       See http://lists.nongnu.org/archive/html/bbdb-user/2018-02/msg00000.html
	abs_target=`$(CYGPATH_W) $(abs_builddir)/$@ | sed 's/\\\\/\\\\\\\\/g'`; \
	$(EMACS) --batch $(AM_ELCFLAGS) $(ELCFLAGS) \
		--load autoload \
		--eval '(setq generated-autoload-file "'"$$abs_target"'")' \
		--eval '(setq make-backup-files nil)' \
		--funcall batch-update-autoloads $(srcdir)

# Generate bbdb-site.el here as pkgdatadir is only known at "make" time.
# We protect the autoconf variables in the sed regular expressions
# so as not to substitute them when processing Makefile.am.
# Warning: the sed expressions will break if PACKAGE_VERSION, PACKAGE_DATE,
# or pkgdatadir contain '='.

bbdb-site.el: $(top_builddir)/config.status bbdb-site.el.in
	sed -e "s=[@]pkgdatadir[@]=$(pkgdatadir)=" \
	    -e "s=[@]PACKAGE_VERSION[@]=$(PACKAGE_VERSION)=" \
	    < $@.in > $@

# Be sure bbdb-site.el exists early for "(require 'bbdb-site)"
BUILT_SOURCES = bbdb-site.el
