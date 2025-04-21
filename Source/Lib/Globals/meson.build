includes = [
    lib_incs,
    include_directories(
        '.',
        '../Codec',
        '../C_DEFAULT',
    ),
]

sources = files(
    'enc_handle.c',
    'enc_handle.h',
    'enc_settings.c',
    'enc_settings.h',
    'metadata_handle.c',
    'metadata_handle.h',
)

project_libs += static_library('GLOBALS',
                sources: sources,
    include_directories: includes,
                install: false,
)
