.SUFFIXES: .c .cpp .cc .o

TARGET_FILENAME := a

$(info ARCH: $(OS) $(PROCESSOR_ARCHITECTURE))

ifeq ($(OS),Windows_NT)
    TOP := $(shell cd ,)
    EXT_EXE := .exe
    RM := del

    ifeq ($(PROCESSOR_ARCHITEW6432),AMD64)
        LFLAGS := -L$(TOP)/libs/win/amd64
    else
        ifeq ($(PROCESSOR_ARCHITECTURE),AMD64)
            LFLAGS := -L$(TOP)/libs/win/amd64
        endif
        ifeq ($(PROCESSOR_ARCHITECTURE),x86)
            LFLAGS := -L$(TOP)/libs/win/x86
        endif
    endif
else
    TOP := $(shell pwd)
    EXT_EXE :=
    RM := rm -f

    ## OS Name(Unix or Darwin)
    UNAME_S := $(shell uname -s)
    ifeq ($(UNAME_S),Linux)
        LFLAGS := -L$(TOP)/libs/unix
    endif
    ifeq ($(UNAME_S),Darwin)
        LFLAGS := -L$(TOP)/libs/darwin
    endif

    ## arch(x86_64 or x86 or arm)
    UNAME_P := $(shell uname -p)
    ifeq ($(UNAME_P),x86_64)
        $(info "x86_64")
    endif
    ifneq ($(filter %86,$(UNAME_P)),)
        $(info "x86 ONLY")
    endif
    ifneq ($(filter arm%,$(UNAME_P)),)
        $(info "ARM")
    endif
endif

TOP_SRCS := $(TOP)/src
CFLAGS += -g -I$(TOP)/include

LDFLAGS += 

CROSS_COMPILE := 
CC := $(CROSS_COMPILE)g++
STRIP := strip

C_SRCS := $(wildcard $(TOP_SRCS)/*.c)
CPP_SRCS := $(wildcard $(TOP_SRCS)/*.cpp)
CC_SRCS := $(wildcard $(TOP_SRCS)/*.cc)
SRCS := $(C_SRCS) $(CPP_SRCS) $(CC_SRCS)

OBJS := $(C_SRCS:.c=.o)
OBJS += $(CPP_SRCS:.cpp=.o)
OBJS += $(CC_SRCS:.cc=.o)

TARGET := $(TARGET_FILENAME)$(EXT_EXE)

all: release

release: $(TARGET)
	$(STRIP) $(TARGET)

$(TARGET): $(OBJS)
	$(CC) $(LDFLAGS) $(CFLAGS) -o $(TARGET) $(OBJS)

.cpp.o:
	$(CC) $(LDFLAGS) $(CFLAGS) -c $< -o $@

.c.o:
	$(CC) $(LDFLAGS) $(CFLAGS) -c $< -o $@

.cc.o:
	$(CC) $(LDFLAGS) $(CFLAGS) -c $< -o $@

clean:
ifeq ($(OS),Windows_NT)
	$(RM) $(subst /,\,$(TOP_SRCS)/*.o)
	$(RM) $(subst /,\,$(TARGET))
else
	$(RM) $(TOP_SRCS)/*.o
	$(RM) $(TARGET)
endif
