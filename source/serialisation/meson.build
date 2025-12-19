# UDA-Serialisation Library

if not has_capnproto
  subdir_done()
endif

# Generate schema.capnp.h
capnp = find_program('capnp')
run_command(
  capnp,
  'compile',
  '-oc++', 'schema.capnp',
  check: true,
)

# Source files
serial_sources = files(
  'capnp_serialisation.cpp',
  'schema.capnp.c++',
)

# Dependencies
serial_deps = [
  capnp_dep,
]

# Compile flags
serial_args = global_defs + warning_flags

# Include directories
serial_inc = [
  include_directories(
    '.',
    '../clientserver/',
    '../../extlib/GSL-4.0.0/include',
  ),
  config_inc,
]

# Build library
uda_serial_lib = library(
  'uda_serialisation',
  serial_sources,
  cpp_args: serial_args,
  include_directories: serial_inc,
  dependencies: serial_deps,
  version: version,
  install: true,
)

# Declare dependency
uda_serial_dep = declare_dependency(
  link_with: uda_serial_lib,
  include_directories: serial_inc,
  dependencies: serial_deps,
)

# Install headers
install_headers(
  'capnp_serialisation.h',
  'schema.capnp.h',
  subdir: 'uda/serialisation',
)

# Generate pkg-config file
pkg = import('pkgconfig')
pkg.generate(
  uda_serial_lib,
  name: 'uda-serialisation',
  description: 'UDA Serialisation Library',
  version: version,
)