############################################################################
# arch/cortex-m23/m2351/src/numaker_pfm_m2351/Makefile
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

MTOWER_S = $(TOPDIR)/mtower_s$(EXEEXT)
MTOWER_NS = $(TOPDIR)/mtower_ns$(EXEEXT)

mtower$(EXEEXT):
	$(Q) $(MAKE) -C secure TOPDIR="$(TOPDIR)" mtower_s$(EXEEXT)
	$(Q) $(MAKE) -C nonsecure TOPDIR="$(TOPDIR)" mtower_ns$(EXEEXT)

clean:
	$(Q) $(MAKE) -C secure TOPDIR="$(TOPDIR)" clean
	$(Q) $(MAKE) -C nonsecure TOPDIR="$(TOPDIR)" clean
	$(Q) rm -rf $(TOPDIR)/build
	$(Q) rm -f $(TOPDIR)/mtower_s.bin
	$(Q) rm -f $(TOPDIR)/mtower_s.hex
	$(Q) rm -f $(TOPDIR)/mtower_ns.bin
	$(Q) rm -f $(TOPDIR)/mtower_ns.hex

distclean: clean

flash:
	$(Q)$(ECHO) -e "$(BLUE)Not supported yet!$(NORMAL)"
