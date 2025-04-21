includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_SSE2',
        '../ASM_SSSE3',
        '../ASM_SSE4_1',
    ),
]

c_args = [
    '-mavx2',
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
    c_args += '/arch:AVX2'
endif
c_args = cc.get_supported_arguments(c_args)

sources = files(
    'aom_subpixel_8t_intrin_avx2.c',
    'av1_inv_txfm_avx2.c',
    'av1_inv_txfm_avx2.h',
    'av1_quantize_avx2.c',
    'blend_a64_mask_avx2.c',
    'cdef_avx2.c',
    'cdef_block_avx2.c',
    'cfl_avx2.c',
    'comb_avg_sad_intrin_avx2.c',
    'compute_sad_avx2.h',
    'compute_sad_intrin_avx2.c',
    'convolve_2d_avx2.c',
    'convolve_avx2.c',
    'convolve_avx2.h',
    'corner_match_avx2.c',
    'dwt_avx2.c',
    'encodetxb_avx2.c',
    'fft_avx2.c',
    'highbd_convolve_2d_avx2.c',
    'highbd_convolve_avx2.c',
    'highbd_fwd_txfm_avx2.c',
    'highbd_intra_pred_avx2.c',
    'highbd_inv_txfm_avx2.c',
    'highbd_jnt_convolve_avx2.c',
    'highbd_quantize_intrin_avx2.c',
    'highbd_variance_avx2.c',
    'highbd_warp_affine_avx2.c',
    'inter_prediction_com_avx2.c',
    'intra_pred_intrin_avx2.c',
    'itx_hbd.h',
    'itx_lbd.h',
    'jnt_convolve_2d_avx2.c',
    'jnt_convolve_avx2.c',
    'mc.h',
    'memory_avx2.h',
    'noise_model_avx2.c',
    'obmc_sad_avx2.c',
    'obmc_variance_avx2.c',
    'pack_unpack_intrinsic_avx2.c',
    'palette_avx2.c',
    'pickrst_avx2.c',
    'pickrst_avx2.h',
    'pic_operators_inline_avx2.h',
    'pic_operators_intrin_avx2.c',
    'resize_avx2.c',
    'restoration_pick_avx2.c',
    'selfguided_avx2.c',
    'sse_avx2.c',
    'ssim_avx2.c',
    'synonyms_avx2.h',
    'temporal_filtering_avx2.c',
    'transforms_intrin_avx2.c',
    'transpose_avx2.h',
    'txfm_common_avx2.h',
    'variance_avx2.c',
    'variance_impl_avx2.c',
    'warp_plane_avx2.c',
    'wiener_convolve_avx2.c',
    'wiener_convolve_avx2.h',
)

objects = []
if not compile_c_only and have_x86_platform
    objects = asm_gen.process([
        'itx_avx2.asm',
        'itx16_avx2.asm',
        'mc_avx2.asm',
        'mc16_avx2.asm',
    ])
endif

project_libs += static_library('ASM_AVX2',
                objects: objects,
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
