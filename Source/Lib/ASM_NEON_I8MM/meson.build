includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_NEON',
        '../ASM_NEON_DOTPROD',
    ),
]

c_args = [
    neon_i8mm_arg,
]

sources = files(
    'aom_convolve8_neon_i8mm.c',
    'convolve_neon_i8mm.c',
    'convolve_scale_neon_i8mm.c',
    'jnt_convolve_neon_i8mm.c',
    'warp_plane_neon_i8mm.c',
)

project_libs += static_library('ASM_NEON_I8MM',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
