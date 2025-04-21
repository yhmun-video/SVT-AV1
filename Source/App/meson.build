includes = [
    lib_incs,
    include_directories('.'),
    include_directories('../..'),
]

sources = files(
    'app_config.c',
    'app_config.h',
    'app_context.c',
    'app_context.h',
    'app_input_y4m.c',
    'app_input_y4m.h',
    'app_main.c',
    'app_output_ivf.c',
    'app_output_ivf.h',
    'app_process_cmd.c',
    'svt_time.c',
    'svt_time.h',
)

executable('SvtAv1EncApp',
                sources: sources,
    include_directories: includes,
           dependencies: svt_av1_dep,
              link_with: safestring_lib,
            install_dir: install_bin_dir,
                install: true,
)
