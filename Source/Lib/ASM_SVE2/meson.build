includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_NEON',
        '../ASM_SVE',
    ),
]

c_args = [
    sve2_arg,
]

sources = files(
    'highbd_convolve_sve2.c',
    'highbd_jnt_convolve_sve2.c',
)

project_libs += static_library('ASM_SVE2',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
