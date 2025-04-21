includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
    ),
]

sources = files(
    'blend_a64_mask_c.c',
    'cfl_c.c',
    'compute_sad_c.c',
    'compute_sad_c.h',
    'encode_txb_ref_c.c',
    'encode_txb_ref_c.h',
    'filterintra_c.c',
    'inter_prediction_c.c',
    'intra_prediction_c.c',
    'pack_unpack_c.c',
    'pack_unpack_c.h',
    'picture_operators_c.c',
    'picture_operators_c.h',
    'sad_av1.c',
    'variance.c',
)

project_libs += static_library('C_DEFAULT',
                sources: sources,
    include_directories: includes,
                install: false,
)
