#
# Copyright (C) 2015 MediaTek Inc.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#

ccflags-y += -I$(srctree)/drivers/clk/mediatek
ccflags-y += -I$(srctree)/drivers/devfreq
ccflags-y += -I$(srctree)/drivers/misc/mediatek/include/mt-plat
ccflags-y += -I$(srctree)/drivers/misc/mediatek/freqhopping/
ccflags-y += -I$(srctree)/drivers/misc/mediatek/freqhopping/$(MTK_PLATFORM)/
ccflags-y += -I$(srctree)/drivers/misc/mediatek/smi/
ccflags-y += -I$(srctree)/drivers/misc/mediatek/smi/$(MTK_PLATFORM)/


FORCE_STEP0 := true
SKIP_SMI_CONFIG := true

ifneq (,$(filter $(CONFIG_MTK_PLATFORM), "mt6771"))
ccflags-y += -DDYNAMIC_DISP_HRT
SUPPORT_USE_DDR_TYPE := true
SUPPORT_MMP := true
FORCE_STEP0 := false
else ifneq (,$(filter $(CONFIG_MTK_PLATFORM), "mt6768"))
ccflags-y += -DSMI_TAL
SUPPORT_MMP := true
FORCE_STEP0 := false
else ifneq (,$(filter $(CONFIG_MTK_PLATFORM), "mt6785"))
ccflags-y += -DSMI_KRU
HRT_MECHANISM := true
BLOCKING_MECHANISM := true
SUPPORT_MMP := true
SKIP_SMI_CONFIG := false
QOS_BOUND_DETECT := true
FORCE_STEP0 := false
ccflags-y += -DCHECK_VOLTAGE
endif

MMDVFS_PLATFORM_DIR=$(srctree)/drivers/misc/mediatek/mmdvfs/$(MTK_PLATFORM)
platformdir_exists=$(shell if [ -d $(MMDVFS_PLATFORM_DIR) ]; then echo "y"; else echo "n"; fi;)


obj-y += mmdvfs_pmqos.o

ifeq (true, $(FORCE_STEP0))
ccflags-y += -DMMDVFS_FORCE_STEP0
endif
ifeq (true, $(SKIP_SMI_CONFIG))
ccflags-y += -DMMDVFS_SKIP_SMI_CONFIG
endif
ifeq (true, $(HRT_MECHANISM))
ccflags-y += -DHRT_MECHANISM
endif
ifeq (true, $(SIMULATE_DVFSRC))
ccflags-y += -DSIMULATE_DVFSRC
endif
ifeq (true, $(BLOCKING_MECHANISM))
ccflags-y += -DBLOCKING_MECHANISM
endif
ifeq (true, $(AEE_CAM_BW_MISMATCH))
ccflags-y += -DAEE_CAM_BW_MISMATCH
endif
ifeq (true, $(MMDVFS_LIMIT))
ccflags-y += -DMMDVFS_LIMIT
endif
ifeq ($(platformdir_exists), y)
obj-y += $(subst ",,$(MTK_PLATFORM))/
else
obj-y += mmdvfs_plat_default.o
endif
ifeq (true, $(APPLY_CLK_LOG))
ccflags-y += -DAPPLY_CLK_LOG
endif

ifeq ($(CONFIG_MTK_QOS_V2),y)
ifeq (true, $(QOS_BOUND_DETECT))
ccflags-y += -DQOS_BOUND_DETECT
ccflags-y += -I$(srctree)/drivers/misc/mediatek/base/power/qos/$(MTK_PLATFORM)/
endif
endif

ifeq ($(CONFIG_MMPROFILE),y)
ifeq (true, $(SUPPORT_MMP))
ccflags-y += -I$(srctree)/drivers/misc/mediatek/mmp/
ccflags-y += -DMMDVFS_MMP
endif
endif

ifeq ($(CONFIG_MTK_DRAMC),y)
ccflags-y += -I$(srctree)/drivers/misc/mediatek/dramc/$(MTK_PLATFORM)/
ccflags-y += -DUSE_MTK_DRAMC
endif

ifeq (true, $(SUPPORT_USE_DDR_TYPE))
ccflags-y += -DUSE_DDR_TYPE
ccflags-y += -I$(srctree)/drivers/misc/mediatek/emi/$(MTK_PLATFORM)/
ccflags-y += -I$(srctree)/drivers/misc/mediatek/emi/submodule/
endif

