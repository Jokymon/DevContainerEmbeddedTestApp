cmake_minimum_required(VERSION 3.27)

SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_VERSION 1)

# Enable logging messages
#set(CMAKE_VERBOSE_MAKEFILE ON)

# Project name
set(PROJECT_NAME EmbeddedApplication)
PROJECT(${PROJECT_NAME} C CXX ASM)
SET(CMAKE_CXX_STANDARD 11)

###################### CHIP CONFIGURATION ##########################
SET(ROOT_PROJ_DIR   ${CMAKE_CURRENT_SOURCE_DIR})
SET(CMAKE_INSTALL_INCLUDEDIR "${ROOT_PROJ_DIR}/${CHIP}/install")
SET(CPU         "cortex-m4")
SET(ARCH_NAME   "arm")
SET(ARCH_VER    "v7e-m")
SET(FAMILY      "stm32f3")
SET(CHIP        "STM32F303xC")
SET(ARCH        "${ARCH_NAME}${ARCH_VER}")
####################################################################

# MCU Config
set(FPU         "-mfpu=fpv4-sp-d16")
set(FLOAT_ABI   "-mfloat-abi=hard")

# Toolchain path
set(TOOLCHAIN_PATH  "/usr/local/bin")
# set(ARM_LIB         "/usr/lib/arm-none-eabi/lib/${ARCH}")s
# Specify C, C++ and ASM compilers
SET(CMAKE_C_COMPILER    ${TOOLCHAIN_PATH}/arm-none-eabi-gcc)
SET(CMAKE_CXX_COMPILER  ${TOOLCHAIN_PATH}/arm-none-eabi-g++)
set(AS                  ${TOOLCHAIN_PATH}/arm-none-eabi-as)
set(AR                  ${TOOLCHAIN_PATH}/arm-none-eabi-ar)
set(OBJCOPY             ${TOOLCHAIN_PATH}/arm-none-eabi-objcopy)
set(OBJDUMP             ${TOOLCHAIN_PATH}/arm-none-eabi-objdump)
set(SIZE                ${TOOLCHAIN_PATH}/arm-none-eabi-size)
set(GDB                 ${TOOLCHAIN_PATH}/arm-none-eabi-gdb)
set(SIZE                ${TOOLCHAIN_PATH}/arm-none-eabi-size)

# Definitions passed at compile time (#defines)
add_definitions(-DFAMILY=${FAMILY})
add_definitions(-DCHIP=${CHIP})
add_definitions(-D${CHIP})
add_definitions(-DUSE_FULL_LL_DRIVER)
add_definitions(-USE_HAL_DRIVER)
add_definitions(-DHSE_VALUE=8000000)
add_definitions(-DHSE_STARTUP_TIMEOUT=100)
add_definitions(-DLSE_STARTUP_TIMEOUT=5000)
add_definitions(-DLSE_VALUE=32768)
add_definitions(-DHSI_VALUE=8000000)
add_definitions(-DLSI_VALUE=40000)
add_definitions(-DDD_VALUE=3300)
add_definitions(-DPREFETCH_ENABLE=1)

# Compilation flags
add_compile_options(-mcpu=${CPU})
add_compile_options(-march=${ARCH})
add_compile_options(-mthumb)
add_compile_options(${FPU})
add_compile_options(${FLOAT_ABI})
add_compile_options(-Og)
add_compile_options(-Wall)
add_compile_options(-fdata-sections)
add_compile_options(-ffunction-sections)
# Only for debugging
add_compile_options(-g -gdwarf-2)


# Linker script path
file(GLOB_RECURSE LINKER_SCRIPT ${ROOT_PROJ_DIR}/Debug/*.ld)

# Variables initialized first time
SET(CMAKE_CXX_FLAGS_INIT "-std=c++11")
SET(CMAKE_C_FLAGS_INIT "-std=gnu99")


# link_directories(${ARM_LIB})

# source code
set(SOURCES
  ${ROOT_PROJ_DIR}/Core/Src/main.c
  ${ROOT_PROJ_DIR}/Core/Src/stm32f3xx_hal_msp.c
  ${ROOT_PROJ_DIR}/Core/Src/stm32f3xx_it.c
  ${ROOT_PROJ_DIR}/Core/Src/syscalls.c
  ${ROOT_PROJ_DIR}/Core/Src/sysmem.c
  ${ROOT_PROJ_DIR}/Core/Src/system_stm32f3xx.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_cortex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_dma.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_exti.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_gpio.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_i2c.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_i2c_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pcd.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pcd_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_spi.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_spi_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim_ex.c
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_ll_usb.c
)

set(INCLUDE_DIRS
  ${ROOT_PROJ_DIR}/Core/Inc
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Inc
  ${ROOT_PROJ_DIR}/Drivers/STM32F3xx_HAL_Driver/Inc/Legacy
  ${ROOT_PROJ_DIR}/Drivers/CMSIS/Device/ST/STM32F3xx/Include 
  ${ROOT_PROJ_DIR}/Drivers/CMSIS/Include  
)

set(EXE_NAME "${PROJECT_NAME}")
include_directories(${INCLUDE_DIRS})
add_executable(${EXE_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})
set(CMAKE_EXE_LINKER_FLAGS "-mcpu=${CPU} -mthumb ${FPU} ${FLOAT_ABI} --specs=nano.specs -T${LINKER_SCRIPT} -Wl,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map,--cref -Wl,--gc-sections")

# Libs and external dependencies
target_link_libraries(${EXE_NAME}.elf 
  PRIVATE  
  -lc -lm -lnosys
  ${ROOT_PROJ_DIR}/Core/Startup/startup_stm32f303vctx.s)