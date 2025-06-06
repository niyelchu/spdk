#  SPDX-License-Identifier: BSD-3-Clause
#  Copyright (C) 2015 Intel Corporation.
#  Copyright (c) 2024 NVIDIA CORPORATION & AFFILIATES.
#  All rights reserved.
#

SPDK_ROOT_DIR := $(abspath $(CURDIR)/..)
include $(SPDK_ROOT_DIR)/mk/spdk.common.mk
include $(SPDK_ROOT_DIR)/mk/spdk.lib_deps.mk

DIRS-y += bdev blob blobfs conf dma accel event json jsonrpc \
          log lvol rpc sock thread trace util nvme vmd scsi \
          ioat ut_mock iscsi notify init trace_parser keyring
ifeq ($(OS),Linux)
DIRS-y += nbd ftl vfio_user
ifeq ($(CONFIG_UBLK),y)
DIRS-y += ublk
endif
endif

# The ut library is used by unit tests and some functional tests, so build it only when either of
# these configs is enabled
ifeq ($(filter $(CONFIG_TESTS) $(CONFIG_UNIT_TESTS),y),y)
DIRS-y += ut
endif

DIRS-$(CONFIG_OCF) += env_ocf
DIRS-$(CONFIG_IDXD) += idxd
DIRS-$(CONFIG_VHOST) += vhost
DIRS-$(CONFIG_VIRTIO) += virtio
DIRS-$(CONFIG_VBDEV_COMPRESS) += reduce
DIRS-$(CONFIG_RDMA) += rdma_provider
DIRS-$(CONFIG_RDMA) += rdma_utils
DIRS-$(CONFIG_VFIO_USER) += vfu_tgt
DIRS-$(CONFIG_FSDEV) += fsdev fuse_dispatcher

ifeq ($(CONFIG_RDMA_PROV),mlx5_dv)
DIRS-y += mlx5
endif
# If CONFIG_ENV is pointing at a directory in lib, build it.
# Out-of-tree env implementations must be built separately by the user.
ENV_NAME := $(notdir $(CONFIG_ENV))
ifeq ($(abspath $(CONFIG_ENV)),$(SPDK_ROOT_DIR)/lib/$(ENV_NAME))
DIRS-y += $(ENV_NAME)
endif

.PHONY: all clean $(DIRS-y)

all: $(DIRS-y)
clean: $(DIRS-y)

include $(SPDK_ROOT_DIR)/mk/spdk.subdirs.mk
