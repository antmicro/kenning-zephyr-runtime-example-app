# Copyright (c) 2025 Antmicro <www.antmicro.com>
#
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

project(app)

kenning_gen_model_header("${CMAKE_CURRENT_BINARY_DIR}/src/model_data.h")
kenning_add_board_repl_target()

include_directories("${CMAKE_CURRENT_BINARY_DIR}/src")
target_sources(app PRIVATE src/main.c)
