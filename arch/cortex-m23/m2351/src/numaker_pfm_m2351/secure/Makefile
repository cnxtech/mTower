############################################################################
# arch/cortex-m23/m2351/src/numaker_pfm_m2351/secure/Makefile
#
# Copyright (c) 2019 Samsung Electronics Co., Ltd. All Rights Reserved.
# Author: Taras Drozdovskyi t.drozdovsky@samsung.com
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
############################################################################

-include $(TOPDIR)/Make.defs
-include $(TOPDIR)/.config
-include Make.defs

CFLAGS += -I../ -I./
CFLAGS += -I$(TOPDIR)/include/mtower
CFLAGS += -I../../CMSIS/Include
CFLAGS += -I../../StdDriver/inc
CFLAGS += -I../../Device/Nuvoton/M2351/Include
CFLAGS += -I$(TOPDIR)/tee_client/public
CFLAGS += -I$(TOPDIR)/tee_client/libteec/include/freertos
CFLAGS += -I$(TOPDIR)/apps/hello_world
CFLAGS += -I$(TOPDIR)/tee_internal/include


OBJDIR = $(TOPDIR)/build/secure$(subst $(TOPDIR),,$(shell pwd))

MTOWER_S = $(TOPDIR)/mtower_s$(EXEEXT)

ASRCS_S = $(CHIP_ASRCS_S) $(CMN_ASRCS)
AOBJS_S = $(addprefix $(OBJDIR)/, $(ASRCS_S:.S=$(OBJEXT)))

CSRCS_S = $(CHIP_CSRCS_S) $(CMN_CSRCS)
COBJS_S = $(addprefix $(OBJDIR)/, $(CSRCS_S:.c=$(OBJEXT)))

OBJS_S = $(AOBJS_S) $(COBJS_S)

LIBPATHS = -L. -L"$(TOPDIR)/lib"

LIBS =  -lm -lc -lnosys -lm2351_StdDriver_s

$(AOBJS_S): $(OBJDIR)/%$(OBJEXT): %.S
	$(Q) mkdir -p $(OBJDIR)/$(dir $<)
	@echo "AS: $<"
	$(Q) $(CC) -c $(AFLAGS) -mcmse $< -o $@

$(COBJS_S): $(OBJDIR)/%$(OBJEXT): %.c
	$(Q) mkdir -p $(OBJDIR)/$(dir $<)
	@echo "CC: $<"
	$(Q) $(CC) -c $(CFLAGS) -mcmse $< -o $@

mtower_s$(EXEEXT): $(OBJS_S) 
	$(Q) $(MAKE) -C ../../StdDriver/src/ TOPDIR="$(TOPDIR)" WORLD=secure libm2351_StdDriver_s.a
	@echo "CC: mTower_s$(EXEEXT)"
	$(Q) $(CC) $(CFLAGS) -mcmse -Tsecure.ld $(OBJS_S) $(LIBPATHS) $(LIBS) -o $(MTOWER_S)
#	-$(Q) $(CC) -Wl,--section-start=.gnu.sgstubs=0x3f000 -Wl,--out-implib=libnsc$(LIBEXT) -Wl,--cmse-implib $(OBJS_S) $(LIBPATHS) $(LIBS)
	-$(Q) $(LD) --section-start=.gnu.sgstubs=0x3f000 --out-implib=libnsc$(LIBEXT) --cmse-implib $(OBJS_S) $(LIBPATHS) $(LIBS)
	@cp -f libnsc$(LIBEXT) $(TOPDIR)/lib/
	$(Q) $(OBJCOPY) -O ihex $(MTOWER_S) $(TOPDIR)/mtower_s.hex
	$(Q) $(OBJCOPY) -O binary $(MTOWER_S) $(TOPDIR)/mtower_s.bin

clean:
	$(Q) rm -f *$(OBJEXT) *$(LIBEXT)
	$(Q) rm -f ../../Device/Nuvoton/M2351/Source/*$(OBJEXT)
	$(Q) rm -f ../../Device/Nuvoton/M2351/Source/GCC/*$(OBJEXT)
	$(Q) rm -f ../../StdDriver/src/*$(OBJEXT) ../../StdDriver/src/*$(LIBEXT)

distclean: clean

