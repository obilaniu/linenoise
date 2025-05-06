project('linenoise', 'c',
    version:         '1.0',
    meson_version:   '>=1.3.0',
    license:         'MIT',
    default_options: [
        'warning_level=3',
        'optimization=s',
        'buildtype=minsize',
        'default_library=static',
    ],
)


linenoise_name_prefix        = get_option('name_prefix')
linenoise_name_prefix_length = linenoise_name_prefix.length()
if   linenoise_name_prefix_length >  1
  error(f'Only one name prefix can be provided, received @linenoise_name_prefix_length@! (@linenoise_name_prefix@)')
elif linenoise_name_prefix_length == 1
  linenoise_name_prefix = linenoise_name_prefix[0]
endif


linenoise_version_script_args = []
if meson.get_compiler('c').get_argument_syntax() == 'gcc'
  linenoise_version_script_args += [
    '-Wl,--version-script=' + meson.current_source_dir() / 'linenoise.lds'
  ]
endif


linenoise_inc = include_directories('.')
linenoise_lib = both_libraries(
    'linenoise', files('linenoise.c', 'encodings/utf8.c'),
    extra_files:         files('linenoise.lds'),
    include_directories: linenoise_inc,
    c_args:              ['-DLINENOISE_MAX_LINE='+get_option('max_line_size').to_string(),
                          '-DLINENOISE_COMPAT='+get_option('compat').to_int().to_string()],
    link_args:           linenoise_version_script_args,
    pic:                 true,
    name_prefix:         linenoise_name_prefix,
    install:             get_option('install_libs'),
    override_options:    ['b_staticpic=true'],
)


linenoise_dep = declare_dependency(
    include_directories: linenoise_inc,
    link_with:           linenoise_lib,
)
linenoise_sha_dep = declare_dependency(
    include_directories: linenoise_inc,
    link_with:           linenoise_lib.get_shared_lib(),
)
linenoise_sta_dep = declare_dependency(
    include_directories: linenoise_inc,
    link_with:           linenoise_lib.get_static_lib(),
)


if get_option('build_examples')
  executable('linenoise_example', files('example.c'),
             dependencies: linenoise_sta_dep,
             install:      get_option('install_examples'))
endif
