AC_INIT(ayatana-indicator-application, 0.5.3, mike.gabriel@das-netzwerkteam.de)
AC_COPYRIGHT([Copyright 2009, 2010 Canonical
Copyright 2015 Arctica Project])

AC_PREREQ(2.53)

AM_CONFIG_HEADER(config.h)
AM_INIT_AUTOMAKE(ayatana-indicator-application, 0.5.3)

AM_MAINTAINER_MODE

AC_ISC_POSIX
AC_PROG_CC
AM_PROG_CC_C_O
AM_PROG_AR
AC_STDC_HEADERS
AC_PROG_LIBTOOL

AC_SUBST(VERSION)
AC_CONFIG_MACRO_DIR([m4])

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AC_PATH_PROG([GLIB_MKENUMS], [glib-mkenums])
AC_PATH_PROG([GLIB_GENMARSHAL], [glib-genmarshal])

PKG_PROG_PKG_CONFIG

###########################
# Dependencies
###########################

GLIB_REQUIRED_VERSION=2.35.4
GTK_REQUIRED_VERSION=3.0.0
GIO_REQUIRED_VERSION=2.26
INDICATOR_REQUIRED_VERSION=0.6.0
DBUSMENUGTK_REQUIRED_VERSION=0.5.90
JSON_GLIB_REQUIRED_VERSION=0.7.6
DBUS_GLIB_REQUIRED_VERSION=0.82

LIBAPPINDICATOR_REQUIRED_VERSION=0.5.0

PKG_CHECK_MODULES(INDICATOR,  glib-2.0 >= $GLIB_REQUIRED_VERSION
                              gio-2.0 >= $GIO_REQUIRED_VERSION
                              gtk+-3.0 >= $GTK_REQUIRED_VERSION
                              ayatana-indicator3-0.4 >= $INDICATOR_REQUIRED_VERSION
                              json-glib-1.0 >= $JSON_GLIB_REQUIRED_VERSION
                              dbus-glib-1 >= $DBUS_GLIB_REQUIRED_VERSION
                              dbusmenu-gtk3-0.4 >= $DBUSMENUGTK_REQUIRED_VERSION)
PKG_CHECK_MODULES(APPINDICATOR, ayatana-appindicator3-0.1 >= $LIBAPPINDICATOR_REQUIRED_VERSION)

#########################
# Check for systemd
#########################
PKG_CHECK_MODULES(SYSTEMD,  systemd,
    [has_systemd=yes],
    []
)
if test "x$has_systemd" = "xyes"; then
       SYSTEMD_USERDIR=`$PKG_CONFIG --variable=systemduserunitdir systemd`
       AC_SUBST(SYSTEMD_USERDIR)
       AC_DEFINE(HAVE_SYSTEMD, 1, [create ayatana-indicator-application.service for systemd])
fi

###########################
# gcov coverage reporting
###########################
m4_include([m4/gcov.m4])
AC_TDD_GCOV
AM_CONDITIONAL([HAVE_GCOV], [test "x$ac_cv_check_gcov" = xyes])
AM_CONDITIONAL([HAVE_LCOV], [test "x$ac_cv_check_lcov" = xyes])
AM_CONDITIONAL([HAVE_GCOVR], [test "x$ac_cv_check_gcovr" = xyes])
AC_SUBST(COVERAGE_CFLAGS)
AC_SUBST(COVERAGE_CXXFLAGS)
AC_SUBST(COVERAGE_LDFLAGS)
AC_SUBST(INDICATOR_CFLAGS)
AC_SUBST(INDICATOR_LIBS)
AC_SUBST(APPINDICATOR_CFLAGS)
AC_SUBST(APPINDICATOR_LIBS)

###########################
# Indicator Info
###########################

if test "x$with_gtk" = x2; then
	INDICATORDIR=`$PKG_CONFIG --variable=indicatordir ayatana-indicator-0.4`
	INDICATORICONSDIR=`$PKG_CONFIG --variable=iconsdir ayatana-indicator-0.4`
else
	INDICATORDIR=`$PKG_CONFIG --variable=indicatordir ayatana-indicator3-0.4`
	INDICATORICONSDIR=`$PKG_CONFIG --variable=iconsdir ayatana-indicator3-0.4`
fi
AC_SUBST(INDICATORDIR)
AC_SUBST(INDICATORICONSDIR)

###########################
# Files
###########################

AC_OUTPUT([
Makefile
src/Makefile
data/Makefile
data/upstart/Makefile
tests/Makefile
])

###########################
# Results
###########################

AC_MSG_NOTICE([

Application Indicator Configuration:

	Prefix:        $prefix
	Indicator Dir: $INDICATORDIR
	gcov:          $use_gcov
	systemd unit:  $SYSTEMD_USERDIR
])
