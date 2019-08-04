PROJECT_NAME     := redow-w-keyboard-basic
TARGETS          := nrf51822_xxac
OUTPUT_DIRECTORY := _build

SDK_ROOT := ../../../..
PROJ_DIR := ../..

$(OUTPUT_DIRECTORY)/nrf51822_xxac.out: \
  LINKER_SCRIPT  := uart_gcc_nrf51.ld

# Source files common to all targets
SRC_FILES += \
  $(PROJ_DIR)/main.c \
  $(SDK_ROOT)/components/drivers_nrf/common/nrf_drv_common.c \
  $(SDK_ROOT)/components/drivers_nrf/uart/nrf_drv_uart.c \
  $(SDK_ROOT)/components/libraries/fifo/app_fifo.c \
  $(SDK_ROOT)/components/libraries/uart/app_uart_fifo.c \
  $(SDK_ROOT)/components/libraries/uart/retarget.c \
  $(SDK_ROOT)/components/libraries/util/app_error.c \
  $(SDK_ROOT)/components/libraries/util/app_error_weak.c \
  $(SDK_ROOT)/components/libraries/util/app_util_platform.c \
  $(SDK_ROOT)/components/libraries/util/sdk_errors.c \
  $(SDK_ROOT)/components/libraries/util/nrf_assert.c \
  $(SDK_ROOT)/components/toolchain/gcc/gcc_startup_nrf51.S \
  $(SDK_ROOT)/components/toolchain/system_nrf51.c \

# Include folders common to all targets
INC_FOLDERS += \
  $(PROJ_DIR) \
  $(SDK_ROOT)/components \
  $(SDK_ROOT)/components/device \
  $(SDK_ROOT)/components/drivers_nrf/common \
  $(SDK_ROOT)/components/drivers_nrf/delay \
  $(SDK_ROOT)/components/drivers_nrf/hal \
  $(SDK_ROOT)/components/drivers_nrf/nrf_soc_nosd \
  $(SDK_ROOT)/components/drivers_nrf/uart \
  $(SDK_ROOT)/components/libraries/fifo \
  $(SDK_ROOT)/components/libraries/log \
  $(SDK_ROOT)/components/libraries/log/src \
  $(SDK_ROOT)/components/libraries/uart \
  $(SDK_ROOT)/components/libraries/util \
  $(SDK_ROOT)/components/proprietary_rf/gzll \
  $(SDK_ROOT)/components/toolchain \
  $(SDK_ROOT)/components/toolchain/cmsis/include \
  $(SDK_ROOT)/components/toolchain/gcc \
  ../../config \

# Libraries common to all targets
LIB_FILES += \
  $(SDK_ROOT)/components/proprietary_rf/gzll/gcc/gzll_nrf51_sd_resources_gcc.a \

# C flags common to all targets
CFLAGS += -DGAZELL_PRESENT
CFLAGS += -DBOARD_CUSTOM
CFLAGS += -DNRF51
CFLAGS += -DUSE_SD_HW_RESOURCES
CFLAGS += -DGAZELL_ALTERNATIVE_RESOURCES
CFLAGS += -DSWI_DISABLE0
CFLAGS += -DNRF51822
CFLAGS += -mcpu=cortex-m0
CFLAGS += -mthumb -mabi=aapcs --std=gnu99
CFLAGS +=  -Wall -Werror -O3 -g3
CFLAGS += -Wno-unused-function
CFLAGS += -Wno-unused-variable
CFLAGS += -mfloat-abi=soft
# keep every function in separate section, this allows linker to discard unused ones
CFLAGS += -ffunction-sections -fdata-sections -fno-strict-aliasing
CFLAGS += -fno-builtin --short-enums

# C++ flags common to all targets
CXXFLAGS += \

# Assembler flags common to all targets
ASMFLAGS += -x assembler-with-cpp
ASMFLAGS += -DGAZELL_PRESENT
ASMFLAGS += -DBOARD_CUSTOM
ASMFLAGS += -DNRF51
ASMFLAGS += -DUSE_SD_HW_RESOURCES
ASMFLAGS += -DGAZELL_ALTERNATIVE_RESOURCES
ASMFLAGS += -DSWI_DISABLE0
ASMFLAGS += -DNRF51822
ASMFLAGS += -DBSP_DEFINES_ONLY

# Linker flags
LDFLAGS += -Xlinker -Map=$(OUTPUT_DIRECTORY)/$(TARGET).map
LDFLAGS += -mthumb -mabi=aapcs -L $(TEMPLATE_PATH) -T$(LINKER_SCRIPT)
LDFLAGS += -mcpu=cortex-m0
# let linker to dump unused sections
LDFLAGS += -Wl,--gc-sections
# use newlib in nano version
LDFLAGS += --specs=nano.specs -lc -lnosys
#suppress wchar errors
LDFLAGS += -Wl,--no-wchar-size-warning

.PHONY: $(TARGETS) default all clean help flash

# Default target - first one defined
default: nrf51822_xxac

# Print all targets that can be built
help:
	@echo following targets are available:
	@echo 	nrf51822_xxac

TEMPLATE_PATH := $(SDK_ROOT)/components/toolchain/gcc

include $(TEMPLATE_PATH)/Makefile.common

$(foreach target, $(TARGETS), $(call define_target, $(target)))

# Flash the program
flash: $(OUTPUT_DIRECTORY)/nrf51822_xxac.hex
	@echo Flashing: $<
	nrfjprog --program $< -f nrf51 --sectorerase
	nrfjprog --reset -f nrf51

erase:
	nrfjprog --eraseall -f nrf52
