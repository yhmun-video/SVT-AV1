subdir('C_DEFAULT')
subdir('Globals')

if not compile_c_only
    if have_x86_platform
        subdir('ASM_SSE2')
        subdir('ASM_SSSE3')
        subdir('ASM_SSE4_1')
        subdir('ASM_AVX2')
        if enable_avx512
            subdir('ASM_AVX512')
        endif
    elif have_arm_platform
        subdir('ASM_NEON')
        if enable_neon_dotprod
            subdir('ASM_NEON_DOTPROD')
        endif
        if enable_neon_i8mm
            subdir('ASM_NEON_I8MM')
        endif
        if enable_sve
            subdir('ASM_SVE')
        endif
        if enable_sve2
            subdir('ASM_SVE2')
        endif
    endif
endif

subdir('Codec')

pkg_cfg = import('pkgconfig')
pkg_cfg.generate(
           name: lib_name,
       filebase: lib_name,
        version: project_version,
        subdirs: project_name,
      libraries: project_libs,
    description: 'SVT (Scalable Video Technology) for AV1 encoder library',
)
