MODULE_big = orafce
OBJS= parse_keyword.o convert.o file.o datefce.o magic.o others.o plvstr.o plvdate.o shmmc.o plvsubst.o utility.o plvlex.o alert.o pipe.o sqlparse.o putline.o assert.o plunit.o random.o aggregate.o orafce.o varchar2.o nvarchar2.o charpad.o charlen.o replace_empty_string.o datefuncs.o ora_date.o number.o

EXTENSION = orafce

DATA = orafce--3.18.sql
DOCS = README.asciidoc COPYRIGHT.orafce INSTALL.orafce

# make "all" the default target
all:

REGRESS = orafce orafce2 dbms_output dbms_utility files varchar2 nvarchar2 aggregates nlssort dbms_random regexp_func ora_to_char convertfunc ora_to_number comparefunc null_relate_func datefuncs ascii vsize instrb substr substrb strposb lpad rpad rtrim ltrim btrim length lengthb bitand listagg number

#REGRESS_OPTS = --load-language=plpgsql --schedule=parallel_schedule --encoding=utf8
REGRESS_OPTS = --schedule=$(srcdir)/parallel_schedule --no-locale --encoding=utf8

#override CFLAGS += -Wextra -Wimplicit-fallthrough=0

subdir = contrib/$(MODULE_big)
top_builddir = ../..
include $(top_builddir)/src/Makefile.global
include $(top_srcdir)/contrib/contrib-global.mk

ifeq ($(enable_nls), yes)
SHLIB_LINK += $(filter -lintl,$(LIBS))
endif

# remove dependency to libxml2 and libxslt
LIBS := $(filter-out -lxml2, $(LIBS))
LIBS := $(filter-out -lxslt, $(LIBS))

plvlex.o: sqlparse.o

sqlparse.o: $(srcdir)/sqlscan.c

$(srcdir)/sqlparse.h: $(srcdir)/sqlparse.c ;

$(srcdir)/sqlparse.c: sqlparse.y
ifdef BISON
	$(BISON) -d $(BISONFLAGS) -o $@ $<
else
ifdef YACC
	$(YACC) -d $(YFLAGS) -p cube_yy $<
	mv -f y.tab.c sqlparse.c
	mv -f y.tab.h sqlparse.h
else
	bison -d $(BISONFLAGS) -o $@ $<
endif
endif

$(srcdir)/sqlscan.c: sqlscan.l
ifdef FLEX
	$(FLEX) $(FLEXFLAGS) -o'$@' $<
else
	flex $(FLEXFLAGS) -o'$@' $<
endif

distprep: $(srcdir)/sqlparse.c $(srcdir)/sqlscan.c

maintainer-clean:
	rm -f $(srcdir)/sqlparse.c $(srcdir)/sqlscan.c $(srcdir)/sqlparse.h $(srcdir)/y.tab.c $(srcdir)/y.tab.h
