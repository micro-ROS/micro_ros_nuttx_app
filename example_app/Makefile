include $(APPDIR)/Make.defs


CFLAGS += ${shell $(INCDIR) "$(CC)" ../micro_ros_lib/include}

MAINSRC = microros_main.c

PROGNAME = microros
PRIORITY = SCHED_PRIORITY_DEFAULT
STACKSIZE = 6000

FORMATED_CXXFLAGS := $(subst /,\/,$(CXXFLAGS))
FORMATED_CXXFLAGS := $(subst ",,$(FORMATED_CXXFLAGS))

FORMATED_CFLAGS := $(subst /,\/,$(CFLAGS))
FORMATED_CFLAGS := $(subst ",,$(FORMATED_CFLAGS))

include $(APPDIR)/Application.mk

