includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_SSE2',
        '../ASM_SSSE3',
    ),
]

c_args = [
    '-msse4.1',
]
if cc.get_id().startswith('intel') and cc.get_argument_syntax() != 'msvc'
    c_args += [
        '-static-intel',
        '-w',
    ]
endif
c_args = cc.get_supported_arguments(c_args)

sources = files(
    'av1_convolve_scale_sse4.c',
    'av1_quantize_sse4_1.c',
    'av1_txfm_sse4.h',
    'av1_txfm1d_sse4.h',
    'blend_a64_mask_sse4.c',
    'blend_sse4.h',
    'cdef_block_sse4_1.c',
    'cdef_sse4.c',
    'compute_sad_intrin_sse4_1.c',
    'corner_match_sse4.c',
    'encodetxb_sse4.c',
    'filterintra_sse4.c',
    'highbd_convolve_2d_sse4.c',
    'highbd_fwd_txfm_sse4.c',
    'highbd_inv_txfm_sse4.c',
    'highbd_jnt_convolve_sse4.c',
    'highbd_txfm_utility_sse4.h',
    'intrapred_16bit_intrin_sse4_1.c',
    'memory_sse4_1.h',
    'obmc_variance_sse4.c',
    'pickrst_sse4.c',
    'pic_operators_intrin_sse4_1.c',
    'reconinter_sse4.c',
    'selfguided_sse4.c',
    'temporal_filtering_constants.h',
    'temporal_filtering_sse4_1.c',
    'warp_plane_sse4.c',
)

project_libs += static_library('ASM_SSE4_1',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
