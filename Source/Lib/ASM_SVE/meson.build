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
    sve_arg,
]

sources = files(
    'aom_sum_squares_sve.c',
    'highbd_convolve_sve.c',
    'highbd_jnt_convolve_sve.c',
    'highbd_pickrst_sve.c',
    'highbd_sse_sve.c',
    'highbd_variance_sve.c',
    'highbd_warp_plane_sve.c',
    'pickrst_sve.c',
    'warp_plane_sve.c',
    'wedge_utils_sve.c',
)

project_libs += static_library('ASM_SVE',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
