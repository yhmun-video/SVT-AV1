project('svt-av1', 'c',
    version: '3.0.2',
    license: [
        'BSD-3-clause',
        'PATENTS',
    ],
    license_files: [
        'LICENSE.md',
        'PATENTS.md',
    ],
    meson_version: '>= 1.1.0',
)

cc = meson.get_compiler('c')

project_name     = meson.project_name()
project_version  = meson.project_version()
project_libs     = []
project_c_args   = []
project_lib_args = []

install_prefix   = get_option('prefix')
install_inc_dir  = join_paths(install_prefix, get_option('includedir'), project_name)
install_lib_dir  = join_paths(install_prefix, get_option('libdir'))
install_bin_dir  = join_paths(install_prefix, get_option('bindir'))
install_data_dir = join_paths(install_prefix, get_option('datadir'), project_name)

# Only 64-bit supported
have_x86_platform = host_machine.cpu_family() == 'x86_64'
have_arm_platform = host_machine.cpu_family() == 'aarch64'

if have_arm_platform
    have_arm_platform = cc.has_header('arm_neon.h')
endif

# Compile only C code with no simds
compile_c_only = not have_x86_platform and not have_arm_platform

if not compile_c_only
    if have_x86_platform
        if host_machine.system() == 'windows'
            asm_format = 'win'
        elif host_machine.system() in ['darwin', 'ios', 'tvos']
            asm_format = 'macho'
        else
            asm_format = 'elf'
        endif

        asm_args = []
        asm_compiler = find_program('nasm', required: false, version: '>= 2.14')
        if asm_compiler.found()
            if asm_format == 'win'
                asm_args += '-gcv8'
            else
                asm_args += '-gdwarf'
            endif
            asm_args += [
                '-MQ', '@OUTPUT@',
                '-MF', '@DEPFILE@',
            ]
        else
            asm_compiler = find_program('nasm', required: yes)
            if asm_format == 'macho'
                asm_args += '-gnull'
            else
                asm_args += '-gdwarf2'
            endif
        endif

        if host_machine.cpu_family() == 'x86_64'
            asm_format += '64'
        else
            asm_format += '32'
        endif

        asm_args += [
            '-f', asm_format,
            '@EXTRA_ARGS@',
            '@INPUT@',
            '-o', '@OUTPUT@'
        ]
        foreach asm_inc : ['Source/Lib/ASM_SSE2', 'Source/Lib/ASM_AVX2']
            asm_args += ['-I', '@0@/@1@'.format(meson.current_source_dir(), asm_inc)]
        endforeach

        asm_gen = generator(asm_compiler,
               output: '@BASENAME@.obj',
              depfile: '@BASENAME@.obj.ndep',
            arguments: asm_args)

        project_c_args += '-DARCH_X86_64=1'

        enable_avx512 = get_option('ENABLE_AVX512')
        if enable_avx512
            if cc.get_argument_syntax() == 'msvc'
                enable_avx512 = cc.get_supported_arguments('/arch:AVX512').length() > 0
            else
                enable_avx512 = cc.get_supported_arguments('-mavx512f').length() > 0
            endif
        endif
        project_c_args += '-DEN_AVX512_SUPPORT=@0@'.format(enable_avx512.to_int())
    elif have_arm_platform
        arm_crc32_arg = '-march=armv8-a+crc'
        neon_prod_arg = '-march=armv8.2-a+dotprod'
        neon_i8mm_arg = '-march=armv8.2-a+dotprod+i8mm'
        sve_arg       = '-march=armv8.2-a+dotprod+i8mm+sve'
        sve2_arg      = '-march=armv9-a+i8mm+sve2'              # SVE2 is a v9-only feature

        enable_neon         = get_option('ENABLE_NEON')
        enable_arm_crc32    = get_option('ENABLE_ARM_CRC32')    and cc.get_supported_arguments(arm_crc32_arg).length() > 0
        enable_neon_dotprod = get_option('ENABLE_NEON_DOTPROD') and cc.get_supported_arguments(neon_prod_arg).length() > 0
        enable_neon_i8mm    = get_option('ENABLE_NEON_I8MM')    and cc.get_supported_arguments(neon_i8mm_arg).length() > 0
        enable_sve          = get_option('ENABLE_SVE')
        enable_sve2         = get_option('ENABLE_SVE2')

        if enable_sve or enable_sve2
            have_sve_header = cc.compiles('''
                #ifndef __ARM_NEON_SVE_BRIDGE
                #error 1
                #endif
                #include <arm_sve.h>
                #include <arm_neon_sve_bridge.h>
                ''',
                args: sve_arg,
                name: 'SVE header')

            # Check whether the compiler can compile SVE functions that require
            # backup/restore of SVE registers according to AAPCS. Clang for Windows used
            # to fail this, see https://github.com/llvm/llvm-project/issues/80009.
            can_sve_compile = cc.compiles('''
                #include <arm_sve.h>
                void other(void);
                svfloat32_t func(svfloat32_t a) {
                    other();
                    return a;
                }
                int main(void) {
                    return 0;
                }
                ''',
                args: sve_arg,
                name: 'SVE compile')

            if not have_sve_header or not can_sve_compile
                enable_sve  = false
                enable_sve2 = false
            endif
        endif

        project_c_args += [
            '-DHAVE_NEON=@0@'.format(enable_neon.to_int()),
            '-DHAVE_ARM_CRC32=@0@'.format(enable_arm_crc32.to_int()),
            '-DHAVE_NEON_DOTPROD=@0@'.format(enable_neon_dotprod.to_int()),
            '-DHAVE_NEON_I8MM=@0@'.format(enable_neon_i8mm.to_int()),
            '-DHAVE_SVE=@0@'.format(enable_sve.to_int()),
            '-DHAVE_SVE2=@0@'.format(enable_sve2.to_int()),
        ]

        project_c_args += '-DARCH_AARCH64=1'
        project_c_args += '-fno-lax-vector-conversions'
        # Prevents missmatch between arm and x86 bitstreams
        project_c_args += '-ffp-contract=off'
    endif
endif

if cc.get_argument_syntax() == 'msvc'
    project_c_args += [
        '/MP',
    ]
else
    if host_machine.system() in ['linux', 'gnu']
        project_c_args += '--std=gnu99'
    else
        project_c_args += 'c_std=c99'
    endif
    if host_machine.system() == 'windows'
        project_c_args += [
            '-Wno-attributes',
            '-Wno-implicit-function-declaration',
        ]
    endif
    if get_option('NATIVE')
        project_c_args += '-march=native'
    endif
    project_c_args += '-mno-avx'
endif

if cc.has_function('strnlen_s')
    project_c_args += '-DHAVE_STRNLEN_S=1'
endif
if cc.has_function('strncpy_s')
    project_c_args += '-DHAVE_STRNCPY_S=1'
endif
if cc.has_function('strcpy_s')
    project_c_args += '-DHAVE_STRCPY_S=1'
endif

if host_machine.system() != 'windows'
    project_lib_args += [
        '-lm',
        '-lpthread'
    ]
endif

project_c_args = cc.get_supported_arguments(project_c_args)
add_project_arguments(project_c_args, language: 'c')

project_lib_args = cc.get_supported_arguments(project_lib_args)
add_project_link_arguments(project_lib_args, language: 'c')

subdir('third_party/fastfeat')
subdir('Source/API')
subdir('Source/Lib')
if get_option('BUILD_APP')
    subdir('third_party/safestringlib')
    subdir('Source/App')
endif

install_data(
    meson.project_license_files(),
    install_dir: install_data_dir,
)
