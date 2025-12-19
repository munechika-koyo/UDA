project(
  'uda',
  'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.1.0',
  version: '2.9.3',
  default_options: [
    'cpp_std=c++23',
    'buildtype=debugoptimized',
  ],
)

# =============================================================================
# Project Information
# =============================================================================
version = meson.project_version()

# =============================================================================
# Compiler and System Setup
# =============================================================================
cxx = meson.get_compiler('cpp')
host_system = host_machine.system()
host_cpu = host_machine.cpu_family()

# Global defines
global_defs = [
  '-DUDA_EXPORT',
  '-DSSLAUTHENTICATION',
]

# =============================================================================
# Warning Flags
# =============================================================================

warning_flags = []

if host_system == 'windows'
  warning_flags += [
    '/W2',
  ]
else
  warning_flags += [
    '-Wall',
    '-Werror',
    '-Wextra',
    '-Wno-unused-parameter',
    '-Wno-missing-field-initializers',
  ]
  if host_system == 'gnu' and cxx.version().version_compare('>5.0')
    warning_flags += [
      '-Wno-format-truncation',
      '-Wno-format-overflow',
    ]
    if cxx.get_id() != 'intel' or cxx.get_id() != 'intel-llvm'
      warning_flags += [
        '-Wno-use-after-free',
      ]
    endif
  endif
  if host_system == 'darwin'
    warning_flags += [
      '-Wno-cast-function-type',
    ]
    if cxx.version().version_compare('>=17.0')
      warning_flags += [
        '-Wno-cast-function-type-mismatch',
      ]
    endif
  endif
endif

# =============================================================================
# Source Directories
# =============================================================================
subdir('source')

# Tests
# Only build tests when not used as a subproject
if not meson.is_subproject()
  if (get_option('buildtype').startswith('debug') or get_option('tests'))
    subdir('test')
  endif
endif

# =============================================================================
# Summary
# =============================================================================
summary(
  {
    'Client Only': get_option('client_only'),
    'Server Only': get_option('server_only'),
  },
  section: 'Core libraries to build',
)

summary(
  {
    'C++': get_option('wrapper_cxx'),
    'Python': get_option('wrapper_python'),
    'Java': get_option('wrapper_java'),
    'IDL': get_option('wrapper_idl'),
  },
  section: 'Wrappers to build',
)