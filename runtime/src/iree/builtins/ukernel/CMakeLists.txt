################################################################################
# Autogenerated by build_tools/bazel_to_cmake/bazel_to_cmake.py from           #
# runtime/src/iree/builtins/ukernel/BUILD.bazel                                #
#                                                                              #
# Use iree_cmake_extra_content from iree/build_defs.oss.bzl to add arbitrary   #
# CMake-only content.                                                          #
#                                                                              #
# To disable autogeneration for this file entirely, delete this header.        #
################################################################################

iree_add_all_subdirs()

iree_cc_library(
  NAME
    static_assert
  HDRS
    "static_assert.h"
  DEPS

  PUBLIC
)

iree_cc_library(
  NAME
    exported_bits
  HDRS
    "exported_bits.h"
  DEPS
    ::static_assert
  PUBLIC
)

iree_cc_library(
  NAME
    internal_headers
  HDRS
    "common.h"
    "elementwise.h"
    "mmt4d.h"
    "mmt4d_internal.h"
    "pack.h"
    "pack_internal.h"
    "query_tile_sizes.h"
    "query_tile_sizes_internal.h"
    "unpack.h"
    "unpack_internal.h"
  DEPS
    ::exported_bits
    ::static_assert
    iree::base::core_headers
    iree::builtins::ukernel::arch::config
  PUBLIC
)

iree_cc_library(
  NAME
    ukernel_noweak
  HDRS
    "api.h"
  SRCS
    "common.h"
    "elementwise.c"
    "elementwise.h"
    "mmt4d.c"
    "mmt4d.h"
    "mmt4d_internal.h"
    "mmt4d_tile.c"
    "pack.c"
    "pack.h"
    "pack_internal.h"
    "pack_tile.c"
    "query_tile_sizes.c"
    "query_tile_sizes.h"
    "query_tile_sizes_internal.h"
    "unpack.c"
    "unpack.h"
    "unpack_internal.h"
    "unpack_tile.c"
  DEPS
    ::exported_bits
    ::static_assert
    iree::base::core_headers
    iree::builtins::ukernel::arch::config
    iree::builtins::ukernel::arch::ukernel_arch
  PUBLIC
)

iree_cc_library(
  NAME
    zzz_weak_linklast
  SRCS
    "weak.c"
  DEPS
    ::internal_headers
  PUBLIC
)

iree_cc_library(
  NAME
    ukernel
  HDRS
    "api.h"
  DEPS
    ::ukernel_noweak
    ::zzz_weak_linklast
  PUBLIC
)

if(IREE_BUILD_COMPILER AND IREE_TARGET_BACKEND_LLVM_CPU)

iree_bitcode_library(
  NAME
    ukernel_bitcode_base
  SRCS
    "mmt4d.c"
    "mmt4d_tile.c"
    "pack.c"
    "pack_tile.c"
    "query_tile_sizes.c"
    "unpack_tile.c"
  HDRS
    "//runtime/src/iree/builtins/ukernel/arch/x86_64:common_x86_64.h"
    "//runtime/src/iree/builtins/ukernel:api.h"
    "//runtime/src/iree/builtins/ukernel:common.h"
    "//runtime/src/iree/builtins/ukernel:elementwise.h"
    "//runtime/src/iree/builtins/ukernel:exported_bits.h"
    "//runtime/src/iree/builtins/ukernel:mmt4d.h"
    "//runtime/src/iree/builtins/ukernel:mmt4d_internal.h"
    "//runtime/src/iree/builtins/ukernel:pack.h"
    "//runtime/src/iree/builtins/ukernel:pack_internal.h"
    "//runtime/src/iree/builtins/ukernel:query_tile_sizes.h"
    "//runtime/src/iree/builtins/ukernel:query_tile_sizes_internal.h"
    "//runtime/src/iree/builtins/ukernel:static_assert.h"
    "//runtime/src/iree/builtins/ukernel:unpack.h"
    "//runtime/src/iree/builtins/ukernel:unpack_internal.h"
    "//runtime/src/iree/schemas:cpu_data.h"
    "//runtime/src/iree/schemas:cpu_feature_bits.inl"
  COPTS
    "-std=c17"
    "-nostdinc"
    "-ffreestanding"
    "-O3"
    "-DNDEBUG"
    "-fno-ident"
    "-fdiscard-value-names"
    "-fno-short-wchar"
    "-c"
    "-emit-llvm"
    "-DIREE_UK_STANDALONE=1"
    "-target"
    "x86_64-unknown-unknown-eabi-elf"
    "-DIREE_UK_ARCH_X86_64"
    "-DIREE_UK_POINTER_SIZE=8"
    "-DIREE_UK_BUILD_X86_64_AVX2_FMA"
    "-DIREE_UK_BUILD_X86_64_AVX512_BASE"
    "-DIREE_UK_BUILD_X86_64_AVX512_VNNI"
  PUBLIC
)

iree_link_bitcode(
  NAME
    ukernel_bitcode
  SRCS
    "arch/x86_64/ukernel_bitcode_x86_64_avx2_fma.bc"
    "arch/x86_64/ukernel_bitcode_x86_64_avx512_base.bc"
    "arch/x86_64/ukernel_bitcode_x86_64_avx512_vnni.bc"
    "arch/x86_64/ukernel_bitcode_x86_64_base.bc"
    "ukernel_bitcode_base.bc"
  PUBLIC
)

iree_c_embed_data(
  NAME
    libukernel_bitcode
  GENERATED_SRCS
    "ukernel_bitcode.bc"
  DEPS

  C_FILE_OUTPUT
    "libukernel.c"
  H_FILE_OUTPUT
    "libukernel.h"
  IDENTIFIER
    "iree_builtins_libukernel"
  FLATTEN
  PUBLIC
)

endif()  # IREE_BUILD_COMPILER AND IREE_TARGET_BACKEND_LLVM_CPU

### BAZEL_TO_CMAKE_PRESERVES_ALL_CONTENT_BELOW_THIS_LINE ###
