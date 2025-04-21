includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
        '../ASM_SSE2',
    ),
]

c_args = [
    '-mssse3',
]
if cc.get_id().startswith('intel') and cc.get_argument_syntax() != 'msvc'
    c_args += [
        '-static-intel',
        '-w',
    ]
endif
c_args = cc.get_supported_arguments(c_args)

sources = files(
    'aom_subpixel_8t_intrin_ssse3.c',
    'av1_inv_txfm_ssse3.c',
    'av1_inv_txfm_ssse3.h',
    'highbd_convolve_2d_ssse3.c',
    'highbd_convolve_ssse3.c',
    'highbd_intrapred_intrin_ssse3.c',
    'highbd_wiener_convolve_ssse3.c',
    'intrapred_ssse3.c',
    'jnt_convolve_ssse3.c',
    'reconinter_ssse3.c',
    'variance_impl_ssse3.c',
)

objects = []
if not compile_c_only and have_x86_platform
    objects = asm_gen.process([
        'aom_subpixel_bilinear_ssse3.asm',
        'aom_subpixel_8t_ssse3.asm',
        'subpel_variance_ssse3.asm',
    ])
endif

project_libs += static_library('ASM_SSSE3',
                objects: objects,
                sources: sources,
                 c_args: c_args,
    include_directories: includes,
                install: false,
)
