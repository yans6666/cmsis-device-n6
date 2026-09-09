from building import *
import os

# Import environment variables
Import('env')

# Get the current working directory
cwd = GetCurrentDir()

# Initialize include paths and source files list
path = [os.path.join(cwd, 'Include')]
src = [os.path.join(cwd, 'Source', 'Templates', 'system_stm32n6xx_s.c')]

# Map microcontroller units (MCUs) to the platform-independent startup basename.
# GCC/IAR use the assembly template; Arm Compiler uses the C template.
mcu_startup_basenames = {
    'STM32N645xx': 'startup_stm32n645xx',
    'STM32N647xx': 'startup_stm32n647xx',
    'STM32N655xx': 'startup_stm32n655xx',
    'STM32N657xx': 'startup_stm32n657xx',
}

# Check each defined MCU, match the platform and append the appropriate startup file
cpp_defines_tuple = env.get('CPPDEFINES', [])
cpp_defines_list = [item[0] if isinstance(item, tuple) else item for item in cpp_defines_tuple]
for mcu, startup_basename in mcu_startup_basenames.items():
    if mcu in cpp_defines_list:
        if rtconfig.PLATFORM in ['gcc', 'llvm-arm']:
            startup_file = startup_basename + '.s'
            src += [os.path.join(cwd, 'Source', 'Templates', 'gcc', startup_file)]
        elif rtconfig.PLATFORM in ['armcc', 'armclang']:
            startup_file = startup_basename + '.c'
            src += [os.path.join(cwd, 'Source', 'Templates', 'arm', startup_file)]
        elif rtconfig.PLATFORM in ['iccarm']:
            startup_file = startup_basename + '.s'
            src += [os.path.join(cwd, 'Source', 'Templates', 'iar', startup_file)]
        break

# Define the build group
group = DefineGroup('STM32N6-CMSIS', src, depend=['PKG_USING_STM32N6_CMSIS_DRIVER'], CPPPATH=path)

# Return the build group
Return('group')