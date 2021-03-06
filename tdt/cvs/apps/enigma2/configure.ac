AC_INIT(enigma2,2.4)
AM_INIT_AUTOMAKE(enigma2,2.4)

TUXBOX_APPS
TUXBOX_APPS_DIRECTORY

AC_PROG_CC
AC_PROG_CXX
AC_PROG_RANLIB

AC_ARG_WITH(libsdl,
	AC_HELP_STRING([--with-libsdl], [use sdl, yes or no]),
	[[withsdl=$withval]],
	[[withsdl=yes]]
)

AC_CHECK_HEADER(dreamdvd/ddvdlib.h,have_ddvdlib="yes",have_ddvdlib="no")
if test x"${have_ddvdlib}" = xyes ; then
	AC_DEFINE(HAVE_DDVDLIB, 1,[Define if ddvdlib is available])
else
	AC_MSG_NOTICE([ddvdlib not found... dont build dvd plugin])
fi
AM_CONDITIONAL(HAVE_DDVDLIB, test "$have_ddvdlib" = "yes")

AC_ARG_ENABLE([tf7700],
        [AS_HELP_STRING(--enable-tf7700,    enable topfield tf7700 stuff)],
#	AC_MSG_ERROR([bad value I${enableval}I for --enable-tf7700]))
        [case "${enableval}" in
                yes) enable_tf7700=true ;;
                no) enable_tf7700=false ;;
                *) AC_MSG_ERROR([bad value ${enableval} for --enable-tf7700]) ;;
        esac])
AM_CONDITIONAL([ENABLE_TF7700], [test x$enable_tf7700 = xtrue])

AC_ARG_ENABLE([cuberevo],
        [AS_HELP_STRING(--enable-cuberevo,    enable cuberevo stuff)],
        [case "${enableval}" in
                yes) enable_cuberevo=true ;;
                no) enable_cuberevo=false ;;
                *) AC_MSG_ERROR([bad value ${enableval} for --enable-cuberevo]) ;;
        esac])
AM_CONDITIONAL([ENABLE_CUBEREVO], [test x$enable_cuberevo = xtrue])

AM_PATH_PYTHON()
AC_PYTHON_DEVEL
#AM_PATH_XINE(1.1.0,, AC_MSG_ERROR([*** Please install xine-lib (devel) first ***]))

TUXBOX_APPS_DVB

_TUXBOX_APPS_LIB_PKGCONFIG_OPTIONAL(FREETYPE, freetype2, HAVE_FREETYPE2)
if test "$HAVE_FREETYPE2" = "no" ; then
	TUXBOX_APPS_LIB_CONFIG(FREETYPE, freetype-config)
fi
TUXBOX_APPS_LIB_PKGCONFIG(FRIBIDI,fribidi)
TUXBOX_APPS_LIB_PKGCONFIG(ID3TAG,id3tag)
TUXBOX_APPS_LIB_PKGCONFIG(MAD,mad)
#TUXBOX_APPS_LIB_PKGCONFIG(PLUGINS,tuxbox-plugins)
TUXBOX_APPS_LIB_PKGCONFIG(PNG,libpng)
TUXBOX_APPS_LIB_PKGCONFIG(SIGC,sigc++-1.2)
TUXBOX_APPS_LIB_PKGCONFIG(DVBSI,libdvbsi++)
_TUXBOX_APPS_LIB_PKGCONFIG_OPTIONAL(GSTREAMER,gstreamer,HAVE_GSTREAMER)
_TUXBOX_APPS_LIB_PKGCONFIG_OPTIONAL(GSTREAMERPBUTILS,gstreamer-pbutils,HAVE_GSTSTREAMERPBUTILS)
_TUXBOX_APPS_LIB_PKGCONFIG_OPTIONAL(LIBXINE,libxine,HAVE_LIBXINE)
AC_CHECK_LIB(jpeg, jpeg_set_defaults, HAVE_JPEG="yes", HAVE_JPEG="no")
JPEG_LIBS="-ljpeg"
AC_SUBST(JPEG_LIBS)

AC_SUBST(LIBUNGIF_LIBS,"")
AC_CHECK_HEADERS(gif_lib.h,
	AC_CHECK_LIB(ungif,DGifOpen,
	[AC_DEFINE(HAVE_LIBUNGIF,1,[Define if you have libgif/libungif including devel headers])
		LIBUNGIF_LIBS="-lungif"],
		[AC_CHECK_LIB(gif,DGifOpen,
			[AC_DEFINE(HAVE_LIUNBGIF,1)
			LIBUNGIF_LIBS="-lgif"])]))

TUXBOX_APPS_LIB_PKGCONFIG(XML2,libxml-2.0)

# ok, we are in trouble: XMLCCWRAP only defines C++ symbols. So let's just check if linking works,
# and assume everything will be ok. Then just check for the "main" symbol, which should be always there.
# This check fails if the user decides to use the --pedantic CFLAG
AC_LANG_SAVE
AC_LANG_CPLUSPLUS
TUXBOX_APPS_LIB_SYMBOL(XMLCCWRAP, xmlccwrap, main)
AC_LANG_RESTORE

if test "$withsdl" = "yes" ; then
	_TUXBOX_APPS_LIB_PKGCONFIG_OPTIONAL(SDL,sdl,WITH_SDL)
	if test "$WITH_SDL" = "no" ; then
		TUXBOX_APPS_LIB_CONFIG(SDL,sdl-config)
		AC_DEFINE_UNQUOTED([WITH_SDL],[$withsdl],[With SDL])
	fi
fi
# allow 'if WITH_SDL' conditionals in Makefile.am
AM_CONDITIONAL(WITH_SDL, test "$WITH_SDL" = "yes")

CPPFLAGS="$CPPFLAGS "'-I$(top_srcdir)'
CPPFLAGS="$CPPFLAGS -D_REENTRANT $PYTHON_CPPFLAGS $MD5SUM_CFLAGS $FREETYPE_CFLAGS $FRIBIDI_CFLAGS $ID3TAG_CFLAGS $MAD_CFLAGS $PLUGINS_CFLAGS $PNG_CFLAGS $SDL_CFLAGS $SIGC_CFLAGS $XMLTREE_CFLAGS $DVBSI_CFLAGS $GSTREAMER_CFLAGS $GSTREAMERPBUTILS_CFLAGS $LIBXINE_CFLAGS $LIBUNGIF_CFLAGS"
#CPPFLAGS="$CPPFLAGS -DMEMLEAK_CHECK"
CXXFLAGS="$CXXFLAGS -fno-rtti -fno-exceptions -Wall"
LDFLAGS="$LDFLAGS -pthread $PYTHON_LDFLAGS $SDL_LDFLAGS $GSTREAMER_LDFLAGS $GSTREAMERPBUTILS_LDFLAGS $LIBXINE_LDFLAGS $LIBUNGIF_LDFLAGS"
#LDFLAGS="-rdynamic $LDFLAGS"

TUXBOX_APPS_GETTEXT

AC_OUTPUT([
Makefile
data/Makefile
data/fonts/Makefile
data/countries/Makefile
data/defaults/Makefile
data/defaults/Dream/Makefile
data/extensions/Makefile
data/skin_default/Makefile
data/skin_default/menu/Makefile
data/skin_default/icons/Makefile
data/skin_default/buttons/Makefile
data/skin_default/spinner/Makefile
lib/Makefile
lib/actions/Makefile
lib/base/Makefile
lib/driver/Makefile
lib/dvb/Makefile
lib/dvb_ci/Makefile
lib/gdi/Makefile
lib/gui/Makefile
lib/mmi/Makefile
lib/nav/Makefile
lib/python/Makefile
lib/python/Components/Makefile
lib/python/Components/Converter/Makefile
lib/python/Components/Renderer/Makefile
lib/python/Components/Sources/Makefile
lib/python/Screens/Makefile
lib/python/Plugins/Makefile
lib/python/Plugins/SystemPlugins/Makefile
lib/python/Plugins/SystemPlugins/SoftwareManager/Makefile
lib/python/Plugins/SystemPlugins/FrontprocessorUpgrade/Makefile
lib/python/Plugins/SystemPlugins/PositionerSetup/Makefile
lib/python/Plugins/SystemPlugins/Hotplug/Makefile
lib/python/Plugins/SystemPlugins/DefaultServicesScanner/Makefile
lib/python/Plugins/SystemPlugins/NFIFlash/Makefile
lib/python/Plugins/SystemPlugins/Satfinder/Makefile
lib/python/Plugins/SystemPlugins/SkinSelector/Makefile
lib/python/Plugins/SystemPlugins/SatelliteEquipmentControl/Makefile
lib/python/Plugins/SystemPlugins/Videomode/Makefile
lib/python/Plugins/SystemPlugins/VideoTune/Makefile
lib/python/Plugins/SystemPlugins/DiseqcTester/Makefile
lib/python/Plugins/SystemPlugins/CommonInterfaceAssignment/Makefile
lib/python/Plugins/SystemPlugins/Tuxtxt/Makefile
lib/python/Plugins/SystemPlugins/Tuxtxt/tuxtxt/Makefile
lib/python/Plugins/SystemPlugins/VFD-Icons/Makefile 
lib/python/Plugins/DemoPlugins/Makefile
lib/python/Plugins/DemoPlugins/TestPlugin/Makefile
lib/python/Plugins/Extensions/Makefile
lib/python/Plugins/Extensions/TuxboxPlugins/Makefile
lib/python/Plugins/Extensions/FileManager/Makefile
lib/python/Plugins/Extensions/CutListEditor/Makefile
lib/python/Plugins/Extensions/MediaScanner/Makefile
lib/python/Plugins/Extensions/MediaPlayer/Makefile
lib/python/Plugins/Extensions/DVDBurn/Makefile
lib/python/Plugins/Extensions/DVDPlayer/Makefile
lib/python/Plugins/Extensions/DVDPlayer/src/Makefile
lib/python/Plugins/Extensions/PicturePlayer/Makefile
lib/python/Plugins/Extensions/PicturePlayer/data/Makefile
lib/python/Plugins/Extensions/GraphMultiEPG/Makefile
lib/python/Plugins/Extensions/SocketMMI/Makefile
lib/python/Plugins/Extensions/SocketMMI/src/Makefile
lib/python/Plugins/Extensions/WebInterface/Makefile
lib/python/Plugins/Extensions/WebInterface/etc/Makefile
lib/python/Plugins/Extensions/WebInterface/po/Makefile
lib/python/Plugins/Extensions/WebInterface/src/Makefile
lib/python/Plugins/Extensions/WebInterface/src/mobile/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/lib/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/gfx/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/img/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/tpl/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/tpl/default/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/tpl/default/streaminterface/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web-data/tpl/default/streaminterface/img/Makefile
lib/python/Plugins/Extensions/WebInterface/src/WebComponents/Makefile
lib/python/Plugins/Extensions/WebInterface/src/WebComponents/Sources/Makefile
lib/python/Plugins/Extensions/WebInterface/src/WebComponents/Converter/Makefile
lib/python/Plugins/Extensions/WebInterface/src/WebChilds/Makefile
lib/python/Plugins/Extensions/WebInterface/src/WebChilds/External/Makefile
lib/python/Plugins/Extensions/WebInterface/src/web/Makefile
lib/python/Plugins/Extensions/TopfieldVFD/Makefile
lib/python/Plugins/Extensions/CuberevoVFD/Makefile
lib/python/Tools/Makefile
lib/service/Makefile
lib/components/Makefile
po/Makefile
main/Makefile
tools/Makefile
enigma2.pc
])

#lib/python/Plugins/Extensions/SimpleRSS/Makefile
