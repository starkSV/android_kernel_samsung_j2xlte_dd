CFG_TEE_DRV_DEBUGFS?=0
ARCH?=arm64

ccflags-y+=-Werror
ccflags-y+=-I$(src)/include/linux -I$(src)/generic -I$(src)/core/$(ARCH)/
ccflags-y+=-I$(src)/include

ccflags-y+=-DCFG_TEE_DRV_DEBUGFS=${CFG_TEE_DRV_DEBUGFS}

obj-y += optee.o

optee-objs:=   \
		generic/tee_service.o \
		generic/tee_driver.o \
		generic/tee_kernel_api.o \
		generic/tee_supp_com.o \
		generic/tee_mem.o \
		generic/tee_op.o \
		generic/tee_mutex_wait.o

ifeq ($(ARCH),arm)
ccflags-y+=-iquote$(src)/core/armv7
optee-objs += core/armv7/tee_tz.o
optee-objs += core/armv7/stm-smc.o
# "smc" assembly intruction requires dedicated "armv7 secure extension"
secext := $(call as-instr,.arch_extension sec,+sec)
AFLAGS_stm-smc.o := -Wa,-march=armv7-a$(secext)
endif

ifeq ($(ARCH),arm64)
ccflags-y+=-iquote$(src)/core/arm64
optee-objs += core/arm64/tee_tz.o
optee-objs += core/arm64/smc.o
optee-objs += core/arm64/handle.o
endif

ifeq ($(CFG_TEE_DRV_DEBUGFS),1)

ifeq ($(ARCH),arm)
optee-objs += core/armv7/tee_tz_debug.o
endif

ifeq ($(ARCH),arm64)
optee-objs += core/arm64/tee_tz_debug.o
endif

optee-objs += generic/tee_debug.o
endif # CFG_TEE_DRV_DEBUGFS
