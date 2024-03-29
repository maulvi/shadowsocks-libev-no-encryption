VERSION_INFO = 2:0:0

AM_CFLAGS = -g -O2 -Wall -Werror -Wno-deprecated-declarations -fno-strict-aliasing -std=gnu99 -D_GNU_SOURCE
AM_CFLAGS += $(PTHREAD_CFLAGS)
AM_CFLAGS += -I$(top_srcdir)/libipset/include
AM_CFLAGS += -I$(top_srcdir)/libcork/include
AM_CFLAGS += $(LIBPCRE_CFLAGS)

SS_COMMON_LIBS = $(top_builddir)/libipset/libipset.la \
                 $(top_builddir)/libcork/libcork.la \
                 $(INET_NTOP_LIB) $(LIBPCRE_LIBS)
SS_COMMON_LIBS += -lev -lsodium -lm

bin_PROGRAMS = ss-local ss-tunnel ss-server ss-manager

sni_src = http.c \
          tls.c \
          rule.c

plugin_src = plugin.c

ss_local_SOURCES = utils.c \
                   jconf.c \
                   json.c \
                   encrypt.c \
                   udprelay.c \
                   cache.c \
                   acl.c \
                   netutils.c \
                   local.c \
				   $(plugin_src) \
                   $(sni_src)

ss_tunnel_SOURCES = utils.c \
                    jconf.c \
                    json.c \
                    encrypt.c \
                    udprelay.c \
                    cache.c \
                    netutils.c \
				    $(plugin_src) \
                    tunnel.c

ss_server_SOURCES = utils.c \
                    netutils.c \
                    jconf.c \
                    json.c \
                    encrypt.c \
                    udprelay.c \
                    cache.c \
                    acl.c \
                    resolv.c \
                    server.c \
				    $(plugin_src) \
                    $(sni_src)

ss_manager_SOURCES = utils.c \
                     jconf.c \
                     json.c \
                     netutils.c \
                     manager.c

ss_local_LDADD = $(SS_COMMON_LIBS)
ss_tunnel_LDADD = $(SS_COMMON_LIBS)
ss_server_LDADD = $(SS_COMMON_LIBS)
ss_manager_LDADD = $(SS_COMMON_LIBS)
ss_local_LDADD += -ludns
ss_tunnel_LDADD += -ludns
ss_server_LDADD += -ludns
ss_manager_LDADD += -ludns

ss_local_CFLAGS = $(AM_CFLAGS) -DMODULE_LOCAL
ss_tunnel_CFLAGS = $(AM_CFLAGS) -DMODULE_TUNNEL
ss_server_CFLAGS = $(AM_CFLAGS) -DMODULE_REMOTE
ss_manager_CFLAGS = $(AM_CFLAGS) -DMODULE_MANAGER

if BUILD_REDIRECTOR
bin_SCRIPTS = ss-nat
bin_PROGRAMS += ss-redir
ss_redir_SOURCES = utils.c \
                   jconf.c \
                   json.c \
                   encrypt.c \
                   netutils.c \
                   cache.c \
                   udprelay.c \
                   redir.c \
				   $(plugin_src) \
                   $(sni_src)

ss_redir_CFLAGS = $(AM_CFLAGS) -DMODULE_REDIR
ss_redir_LDADD = $(SS_COMMON_LIBS)
ss_redir_LDADD += -ludns
endif

lib_LTLIBRARIES = libshadowsocks-libev.la
libshadowsocks_libev_la_SOURCES = $(ss_local_SOURCES)
libshadowsocks_libev_la_CFLAGS = $(ss_local_CFLAGS) -DLIB_ONLY
libshadowsocks_libev_la_LDFLAGS = -version-info $(VERSION_INFO)
libshadowsocks_libev_la_LIBADD = $(ss_local_LDADD)
include_HEADERS = shadowsocks.h

noinst_HEADERS = acl.h encrypt.h json.h netutils.h redir.h server.h tls.h uthash.h \
				 cache.h http.h local.h plugin.h resolv.h tunnel.h utils.h \
				 common.h jconf.h manager.h protocol.h rule.h socks5.h udprelay.h
