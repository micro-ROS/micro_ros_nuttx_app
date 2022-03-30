include $(APPDIR)/Make.defs


CFLAGS += ${shell $(INCDIR) "$(CC)" ../micro_ros_lib/include}

INCLUDE_ROS2_PACKAGES = $(shell cd ../micro_ros_lib/micro_ros_src && colcon list | awk '{print $$1}' | awk -v d=" " '{s=(NR==1?s:s d)$$0}END{print s}')
MICROROS_INCLUDES += $(foreach pkg, $(INCLUDE_ROS2_PACKAGES), ${shell $(INCDIR) "$(CC)" ../micro_ros_lib/include/$(pkg)})
CFLAGS += $(MICROROS_INCLUDES)

MAINSRC = microros_main.c

PROGNAME = microros
PRIORITY = SCHED_PRIORITY_DEFAULT
STACKSIZE = 6000

FORMATED_CXXFLAGS := $(subst /,\/,$(CXXFLAGS))
FORMATED_CXXFLAGS := $(subst ",,$(FORMATED_CXXFLAGS))

FORMATED_CFLAGS := $(subst /,\/,$(CFLAGS))
FORMATED_CFLAGS := $(subst ",,$(FORMATED_CFLAGS))

include $(APPDIR)/Application.mk

