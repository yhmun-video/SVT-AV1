includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
    ),
]

c_args = [
    '-msse2',
]
if cc.get_id().startswith('intel') and cc.get_argument_syntax() != 'msvc'
    c_args += [
        '-static-intel',
        '-w',
    ]
endif
c_args = cc.get_supported_arguments(c_args)

sources = files(
    'av1_txfm_sse2.h',
    'avc_style_mcp_intrin_sse.c',
    'compute_mean_intrin_sse2.c',
    'compute_mean_sse2.h',
    'convolve_2d_sse2.c',
    'convolve_sse2.c',
    'dlf_intrin_sse2.c',
    'dlf_sse2.h',
    'encodetxb_sse2.c',
    'fft_sse2.c',
    'highbd_intrapred_sse2.h',
    'highbd_intrapred_sse2.c',
    'highbd_subtract_sse2.c',
    'highbd_variance_sse2.c',
    'intra_pred_av1_intrin_sse2.c',
    'jnt_convolve_2d_sse2.c',
    'jnt_convolve_sse2.c',
    'lpf_common_sse2.h',
    'mcp_sse2.h',
    'me_sad_calc_intrin_sse2.c',
    'pack_unpack_intrin_sse2.c',
    'pic_operators_intrin_sse2.c',
    'picture_operators_sse2.h',
    'synonyms.h',
    'transpose_sse2.h',
    'variance_sse2.c',
    'variance_sse2.h',
    'wiener_convolve_sse2.c',
)

objects = []
if not compile_c_only and have_x86_platform
    objects = asm_gen.process([
        'aom_subpixel_8t_sse2.asm',
        'highbd_intrapred_sse2_.asm',
        'highbd_variance_impl_sse2.asm',
        'intrapred_sse2.asm',
        'picture_operators_sse2.asm',
        'subpel_variance_sse2.asm',
        'subtract_sse2.asm',
    ])
endif

project_libs += static_library('ASM_SSE2',
                objects: objects,
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
