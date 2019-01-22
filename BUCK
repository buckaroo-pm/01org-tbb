load('//:subdir_glob.bzl', 'subdir_glob')
load('//:buckaroo_macros.bzl', 'buckaroo_deps_from_package')

genrule(
  name = 'make',
  out = 'out',
  srcs = glob([
    'build/**/*',
    'include/**/*',
    'python/**/*',
    'src/**/*',
    '*',
  ]),
  cmd = 'touch $OUT && cd $SRCDIR && make',
)

linux_deps = \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-pthread') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-dl') + \
  buckaroo_deps_from_package('github.com/buckaroo-pm/host-rt')

genrule(
  name = 'version-string-linux',
  out = 'version_string.ver',
  srcs = [
    'build/version_info_linux.sh',
  ],
  cmd = 'sh $SRCS > $OUT',
)

genrule(
  name = 'version-string-macos',
  out = 'version_string.ver',
  srcs = [
    'build/version_info_macos.sh',
  ],
  cmd = 'sh $SRCS > $OUT',
)

cxx_library(
  name = 'tbbmalloc',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'tbb/**/*.h'),
    ('include', 'tbb/compat/**/*'),
  ]),
  headers = subdir_glob([
    ('src', '**/*.h'),
  ]),
  platform_headers = [
    ('linux.*', { 'version_string.ver': ':version-string-linux' }),
    ('macos.*', { 'version_string.ver': ':version-string-macos' }),
  ],
  preprocessor_flags = [
    "-D__TBBMALLOC_BUILD=1" 
  ],
  srcs = glob([
    'src/tbbmalloc/**/*.cpp',
  ]),
  platform_deps = [
    ('linux.*', linux_deps),
  ],
  visibility = [
    'PUBLIC',
  ],
)

cxx_library(
  name = 'tbbproxy',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'tbb/**/*.h'),
    ('include', 'tbb/compat/**/*'),
  ]),
  headers = subdir_glob([
    ('src', '**/*.h'),
    ('src/tbb', '**/*.h'),
  ]),
  platform_headers = [
    ('linux.*', { 'version_string.ver': ':version-string-linux' }),
    ('macos.*', { 'version_string.ver': ':version-string-macos' }),
  ],
  srcs = glob([
    'src/tbbproxy/**/*.cpp',
  ]),
  platform_deps = [
    ('linux.*', linux_deps),
  ],
  visibility = [
    'PUBLIC',
  ],
)

cxx_library(
  name = 'tbb',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'tbb/**/*.h'),
    ('include', 'tbb/compat/**/*'),
  ]),
  headers = subdir_glob([
    ('src', '**/*.h'),
    ('src/old/include', '**/*.h'),
    ('src/rml/include', '**/*.h'),
    ('src', 'tbb/*.cpp'),
  ]),
  platform_headers = [
    ('linux.*', { 'version_string.ver': ':version-string-linux' }),
    ('macos.*', { 'version_string.ver': ':version-string-macos' }),
  ],
  srcs = glob([
    'src/tbb/**/*.cpp',
    'src/old/**/*.cpp',
    'src/rml/**/*.cpp',
  ], exclude=["src/**/test/**/*.cpp", "src/old/test_*.cpp"]),
  deps = [
    ':tbbmalloc',
    ':tbbproxy',
  ],
  platform_deps = [
    ('linux.*', linux_deps),
  ],
  visibility = [
    'PUBLIC',
  ],
)
