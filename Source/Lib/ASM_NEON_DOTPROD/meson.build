includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_NEON',
    ),
]

c_args = [
    neon_prod_arg,
]

sources = files(
    'aom_convolve8_neon_dotprod.c',
    'convolve_neon_dotprod.c',
    'convolve_scale_neon_dotprod.c',
    'highbd_variance_neon_dotprod.c',
    'jnt_convolve_neon_dotprod.c',
    'pic_analysis_neon_dotprod.c',
    'sse_neon_dotprod.c',
    'variance_neon_dotprod.c',
)

project_libs += static_library('ASM_NEON_DOTPROD',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
