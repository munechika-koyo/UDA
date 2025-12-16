project(
  'portablexdr',
  'c',
  license: 'LGPL-2.0-or-later',
  meson_version: '>=1.1.0',
  version: '4.9.1',
  default_options: [
    'c_std=c11',
    'buildtype=debugoptimized',
  ],
)

# ============================================================================
# Compiler and System Setup
# ============================================================================
cc = meson.get_compiler('c')
host_system = host_machine.system()

if cc.get_id() == 'gnu' or cc.get_id() == 'clang'
  add_project_arguments('-Wno-incompatible-pointer-types', language: 'c')
endif

if host_system != 'windows'
  add_project_arguments(
    '-fno-strict-aliasing',
    '-Wno-old-style-definition',
    '-Wno-strict-prototypes',
    language: 'c',
  )
endif

# =============================================================================
# Header Setup
# =============================================================================
conf_data = configuration_data()
conf_h = configure_file(
  input: 'config.h.in',
  output: 'config.h',
  configuration: conf_data,
)

public_headers = files(
  'rpc/rpc.h',
  'rpc/types.h',
  'rpc/xdr.h',
) + conf_h
public_includes = include_directories('rpc')
private_includes = include_directories('.')
install_headers(public_headers, subdir: 'rpc')

# =============================================================================
# Library Setup
# =============================================================================
sources = files(
  'xdr.c',
  'xdr_array.c',
  'xdr_float.c',
  'xdr_intXX_t.c',
  'xdr_mem.c',
  'xdr_rec.c',
  'xdr_reference.c',
  'xdr_stdio.c',
)

portablexdr_lib = library(
  'xdr',
  sources,
  include_directories: [private_includes, public_includes],
  install: true,
)

# =============================================================================
# Pkg-config generation
# =============================================================================
pkg = import('pkgconfig')
pkg.generate(
  portablexdr_lib,
  name: 'portablexdr',
  description: 'Portable XDR library',
  version: meson.project_version(),
)