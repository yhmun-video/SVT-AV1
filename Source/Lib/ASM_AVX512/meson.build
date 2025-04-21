includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_SSE2',
        '../ASM_SSE4_1',
        '../ASM_AVX2',
    ),
]

c_args = [
    '-mavx512f',
    '-mavx512bw',
    '-mavx512dq',
    '-mavx512vl',
]
if cc.get_id().startswith('intel')
    if host_machine.system() == 'windows'
        # Intel Windows (*Note - The Warning level /W0 should be made to /W4 at some point)
        c_args += [
            '/Qdiag-disable:10010,10148,10157',
            '/W0',
        ]
    else
        c_args += [
            '-static-intel',
            '-w',
        ]
    endif
endif
if cc.get_argument_syntax() == 'msvc'
    c_args += '/arch:AVX512'
endif
c_args = cc.get_supported_arguments(c_args)

sources = files(
    'cdef_avx512.c',
    'cdef_block_avx512.c',
    'compute_sad_intrin_avx512.c',
    'convolve_2d_avx512.c',
    'convolve_avx512.c',
    'convolve_avx512.h',
    'encodetxb_avx512.c',
    'highbd_fwd_txfm_AVX512.c',
    'highbd_intra_pred_avx512.c',
    'highbd_inv_txfm_avx512.c',
    'jnt_convolve_2d_avx512.c',
    'jnt_convolve_avx512.c',
    'pickrst_avx512.c',
    'pic_operators_intrin_avx512.c',
    'synonyms_avx512.h',
    'transpose_avx512.h',
    'transpose_encoder_avx512.h',
    'variance_avx512.c',
    'wiener_convolve_avx512.c',
)

project_libs += static_library('ASM_AVX512',
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
