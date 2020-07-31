PREFIX    = /3rd/gcc-arm-none-eabi-4_9-2015q3-20150921-win32/bin/arm-none-eabi-
CC        = $(PREFIX)gcc
AS        = $(PREFIX)gcc -x assembler-with-cpp
OBJCOPY   = $(PREFIX)objcopy
OBJDUMP   = $(PREFIX)objdump
AR        = $(PREFIX)ar
SZ        = $(PREFIX)size
LD        = $(PREFIX)ld

###############################################################################
# the default target
###############################################################################

default : image

OUT_DIR   = out
TARGET    = app.elf

###############################################################################
# rules to compile files
###############################################################################

SRCS      = $(wildcard *.c)
OBJS      = $(addprefix $(OUT_DIR)/,$(SRCS:%.c=%.o))

CFLAGS    = -mcpu=cortex-m4                                 \
            -mthumb                                         \
            -mfpu=fpv4-sp-d16                               \
            -mfloat-abi=softfp                              \
            -std=gnu99                                      \
            -gdwarf-2                                       \
            -fno-common                                     \
            -fmessage-length=0                              \
            -Wno-unused-function                            \
            -Wno-format-overflow                            \
            -Wno-restrict                                   \
            -fno-exceptions                                 \
            -ffunction-sections                             \
            -fdata-sections                                 \
            -fomit-frame-pointer                            \
            -Wall                                           \
            -Werror                                         \
            -Wpointer-arith                                 \
            -Wno-error=unused-function                      \
            -Wno-stringop-truncation                        \
            -Wno-misleading-indentation                     \
            -Wno-address-of-packed-member                   \
            -MMD -MP -MF"$(@:%.o=%.d)" -MT"$@"              \
            -Os                                             \
            -DNDEBUG                                        \
            -include common/prj_conf_opt.h

CDEFS     = -D__CONFIG_CHIP_XR872                           \
            -D__CONFIG_CHIP_ARCH_VER=2                      \
            -D__CONFIG_ARCH_APP_CORE                        \
            -D__CONFIG_CPU_CM4F                             \
            -D__CONFIG_HOSC_TYPE=40                         \
            -D__CONFIG_LIBC_REDEFINE_GCC_INT32_TYPE         \
            -D__CONFIG_LIBC_PRINTF_FLOAT                    \
            -D__CONFIG_LIBC_SCANF_FLOAT                     \
            -D__CONFIG_LIBC_WRAP_STDIO                      \
            -D__CONFIG_MALLOC_USE_STDLIB                    \
            -D__CONFIG_OS_LITEOS                            \
            -D__CONFIG_LWIP_V1                              \
            -D__CONFIG_MBEDTLS_VER=0x02100000               \
            -D__CONFIG_MBUF_IMPL_MODE=0                     \
            -D__CONFIG_WLAN                                 \
            -D__CONFIG_WLAN_STA                             \
            -D__CONFIG_WLAN_AP                              \
            -D__CONFIG_WLAN_MONITOR                         \
            -D__CONFIG_WIFI_CERTIFIED                       \
            -D__CONFIG_XIP                                  \
            -D__CONFIG_SECTION_ATTRIBUTE_XIP                \
            -D__CONFIG_SECTION_ATTRIBUTE_NONXIP             \
            -D__CONFIG_SECTION_ATTRIBUTE_SRAM               \
            -D__CONFIG_ROM                                  \
            -D__CONFIG_ROM_XZ                               \
            -D__CONFIG_PM                                   \
            -D__CONFIG_OTA                                  \
            -D__CONFIG_OTA_POLICY=0x00                      \
            -D__CONFIG_CACHE_POLICY=0x04                    \
            -D__CONFIG_DMAHEAP_PSRAM_SIZE=0                 \
            -D__CONFIG_MBUF_HEAP_MODE=0                     \
            -D__CONFIG_MBEDTLS_HEAP_MODE=0                  \
            -D__CONFIG_HTTPC_HEAP_MODE=0                    \
            -D__CONFIG_MQTT_HEAP_MODE=0                     \
            -D__CONFIG_NOPOLL_HEAP_MODE=0                   \
            -D__CONFIG_WPA_HEAP_MODE=0                      \
            -D__CONFIG_UMAC_HEAP_MODE=0                     \
            -D__CONFIG_LMAC_HEAP_MODE=0                     \
            -D__CONFIG_CEDARX_HEAP_MODE=0                   \
            -D__CONFIG_AUDIO_HEAP_MODE=0                    \
            -D__CONFIG_CODEC_HEAP_MODE=0

C_INCS    = -I../include                                    \
            -I../include/libc                               \
            -I../include/driver/cmsis                       \
            -I../LiteOS/arch/arm/arm-m/include              \
            -I../LiteOS/kernel/include                      \
            -I../LiteOS/kernel/base/include                 \
            -I../LiteOS/utils/include                       \
            -I../LiteOS/targets/XR872/include               \
            -I../include/net                                \
            -I../include/net/lwip-1.4.1                     \
            -I../include/net/mbedtls-2.16.0                 \
            -I../include/net/lwip-1.4.1/ipv4                \
            -I.                                             \
            -I..                                            \
            -I../common/board/xr872_evb_ai

$(OUT_DIR)/%.o : %.c
	$(CC) $(CFLAGS) $(CDEFS) $(C_INCS) -c $< -o $@

###############################################################################
# rules to create output dir
###############################################################################

$(OUT_DIR) :
	-mkdir -p $@

###############################################################################
# rules to make target
###############################################################################

LD_FILE   = .project.ld

LD_FLAGS  = -mcpu=cortex-m4                                 \
            -mthumb                                         \
            -mfpu=fpv4-sp-d16                               \
            -mfloat-abi=softfp                              \
            -Wl,--gc-sections                               \
            --specs=nano.specs                              \
            -Wl,-Map=$(TARGET:%.elf=%.map),--cref           \
            -u _printf_float                                \
            -u _scanf_float                                 \
            -Wl,--wrap,main                                 \
            -Wl,--wrap,exit                                 \
            -Wl,--wrap,_malloc_r                            \
            -Wl,--wrap,_realloc_r                           \
            -Wl,--wrap,_free_r                              \
            -Wl,--wrap,gettimeofday                         \
            -Wl,--wrap,settimeofday                         \
            -Wl,--wrap,time                                 \
            -Wl,--wrap,printf                               \
            -Wl,--wrap,vprintf                              \
            -Wl,--wrap,puts                                 \
            -Wl,--wrap,fprintf                              \
            -Wl,--wrap,vfprintf                             \
            -Wl,--wrap,fputs                                \
            -Wl,--wrap,putchar                              \
            -Wl,--wrap,putc                                 \
            -Wl,--wrap,fputc                                \
            -Wl,--wrap,fflush                               \
            -Wl,--wrap,memcpy                               \
            -Wl,--wrap,memset                               \
            -Wl,--wrap,memmove                              \
            -T.project.ld                                   \
            -L../lib                                        \
            -L../lib/xradio_v2                              \
            $(OBJS)                                         \
            -Wl,--whole-archive                             \
            -lchip                                          \
            -lrom                                           \
            -lboard                                         \
            -Wl,--no-whole-archive                          \
            -lcmd                                           \
            -lframework                                     \
            -liperf                                         \
            -lstartup                                       \
            -lota                                           \
            -lwlan                                          \
            -lwpas                                          \
            -lhostapd                                       \
            -lwpa                                           \
            -lwpas                                          \
            -lhostapd                                       \
            -lwpa                                           \
            -lnet80211                                      \
            -lxrwireless                                    \
            -lnet80211                                      \
            -lmqtt                                          \
            -lnopoll                                        \
            -llibwebsockets                                 \
            -lhttpd                                         \
            -lhttpcli                                       \
            -lmbedtls                                       \
            -lsntp                                          \
            -lping                                          \
            -ludhcpd                                        \
            -lsmartlink                                     \
            -lairkiss_aes                                   \
            -lsc_assistant                                  \
            -lxrsys                                         \
            -llwip                                          \
            -lethernetif                                    \
            -llwip                                          \
            -lxrsys                                         \
            -lcjson                                         \
            -lfs                                            \
            -lconsole                                       \
            -lcomponent                                     \
            -lreverb                                        \
            -laudmgr                                        \
            -lpcm                                           \
            -ladt                                           \
            -lutil                                          \
            -ljpeg                                          \
            -lzbar                                          \
            -leq                                            \
            -lopus                                          \
            -lpm                                            \
            -limage                                         \
            -lefpg                                          \
            -los                                            \
            -lliteos                                        \
            -lxrc                                           \
            -lstdc++                                        \
            -lsupc++                                        \
            -lm                                             \
            -lc                                             \
            -lgcc                                           \
            -lxrc

$(TARGET) : $(OUT_DIR) $(OBJS)
	$(CC) $(LD_FLAGS) -o $@

###############################################################################
# rules to make bin
###############################################################################

MKIMAGE    = mkimage

ifeq ($(shell uname -o), Cygwin)
  MKIMAGE := mkimage.exe
endif
ifeq ($(shell uname -o), Msys)
  MKIMAGE := mkimage.exe
endif

IMAGE_PATH = ../image/xr872
IMAGE_TOOL = ../tools/$(MKIMAGE)

.PHONY : image

image : $(TARGET)
	$(OBJCOPY) -O binary -R .xip $^ app.bin
	$(OBJCOPY) -O binary -j .xip $^ app_xip.bin
	$(IMAGE_TOOL) -O -c .image.cfg -o xr_system.img

###############################################################################
# rules to clean
###############################################################################

.PHONY : clean

clean :
	rm $(OUT_DIR) $(TARGET) $(TARGET:%.elf=%.map) xr_system.img app.bin app_xip.bin -rf

