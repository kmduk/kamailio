#
# WARNING: do not run this directly, it should be run by the master Makefile

include ../../Makefile.defs
auto_gen=
NAME=db_redis.so

ifeq ($(CROSS_COMPILE),)
HIREDIS_BUILDER = $(shell \
	if pkg-config --exists hiredis; then \
		echo 'pkg-config hiredis'; \
	fi)
endif

ifeq ($(HIREDIS_BUILDER),)
	HIREDISDEFS=-I$(LOCALBASE)/include -I$(LOCALBASE)/include/hiredis -I/usr/include/hiredis
	HIREDISLIBS=-L$(LOCALBASE)/lib -lhiredis $(LD_RPATH)$(LOCALBASE)/lib
else
	HIREDISDEFS = $(shell $(HIREDIS_BUILDER) --cflags)
	HIREDISLIBS = $(shell $(HIREDIS_BUILDER) --libs)

ifeq (,$(findstring hiredis,$(HIREDISDEFS)))
	DEFS+=-DWITH_HIREDIS_PATH
endif

ifeq ($(HIREDISLIBS),-L -lhiredis)
		HIREDISDEFS = $(shell $(HIREDIS_BUILDER) --cflags) /opt/local/include
		HIREDISLIBS = -L/opt/local/lib -lhiredis
endif

endif

DEFS+=$(HIREDISDEFS)
LIBS=$(HIREDISLIBS)

SERLIBPATH=../../lib
SER_LIBS=$(SERLIBPATH)/srdb2/srdb2 $(SERLIBPATH)/srdb1/srdb1

include ../../Makefile.modules

install-redis-scripts: $(bin_prefix)/$(bin_dir)
	REDISON=yes $(MAKE) -C ../../../utils/kamctl/ install-modules

install-scripts: install-redis-scripts
