load('//tools/build:sphinx.bzl', 'sphinx_build', 'sphinx_spelling_test', 'sphinx_linkcheck_test')
load('//tools/build:pkg.bzl', 'stage_files')
load('//tools/build:python.bzl', 'py_script', 'py_lint_test')
load('//tools/build:image.bzl', 'png_images')
load('//tools/krpctools:docgen.bzl', 'docgen_multiple')
load('//doc:test.bzl', 'check_documented_test')
load('//:config.bzl', 'version')
load('//doc:macros.bzl', 'csharp_binary_multiple', 'csharp_library_multiple', 'cc_binary_multiple', 'java_binary_multiple')

filegroup(
    name = 'doc',
    srcs = [
        ':html',
        ':pdf'
    ]
)

stage_files(
    name = 'srcs',
    srcs = glob([
        'src/**/*.rst',
        'src/images/**/*.png',
        'src/crafts/*.craft',
        'src/scripts/**/*.c',
        'src/scripts/**/*.ino',
        'src/scripts/**/*.cs',
        'src/scripts/**/*.cpp',
        'src/scripts/**/*.java',
        'src/scripts/**/*.lua',
        'src/scripts/**/*.py',
        'src/scripts/**/*.js'
    ]) + [
        ':conf',
        'src/_static/custom.css',
        'src/_templates/layout.html',
        'src/dictionary.txt',
        ':images',
        ':cnano-api',
        ':csharp-api',
        ':cpp-api',
        ':java-api',
        ':lua-api',
        ':python-api'
    ],
    path_map = {
        'doc/src/': ''
    }
)

py_script(
    name = 'sphinx-build',
    script = 'sphinx-build',
    pkg = '@python_sphinx//file',
    deps = [
        '@python_alabaster//file',
        '@python_babel//file',
        '@python_certifi//file',
        '@python_charset_normalizer//file',
        '@python_docutils//file',
        '@python_idna//file',
        '@python_imagesize//file',
        '@python_javalang//file',
        '@python_javasphinx//file',
        '@python_jinja2//file',
        '@python_markupsafe//file',
        '@python_packaging//file',
        '@python_pyenchant//file',
        '@python_pygments//file',
        '@python_pytz//file',
        '@python_requests//file',
        '@python_snowballstemmer//file',
        '@python_sphinx_csharp//file',
        '@python_sphinx_rtd_theme//file',
        '@python_sphinx_tabs//file',
        '@python_sphinxcontrib_applehelp//file',
        '@python_sphinxcontrib_devhelp//file',
        '@python_sphinxcontrib_htmlhelp//file',
        '@python_sphinxcontrib_jsmath//file',
        '@python_sphinxcontrib_luadomain//file',
        '@python_sphinxcontrib_qthelp//file',
        '@python_sphinxcontrib_serializinghtml//file',
        '@python_sphinxcontrib_spelling//file',
        '@python_urllib3//file'
    ],
    visibility = ['//visibility:public']
)

genrule(
    name = 'conf',
    srcs = ['conf.py.tmpl'],
    outs = ['src/conf.py'],
    cmd = 'sed \'s/%VERSION%/'+version+'/g\' "$<" > "$@"'
)

sphinx_build(
    name = 'html',
    out = 'html.zip',
    srcs = [':srcs'],
    sphinx_build = ':sphinx-build',
    builder = 'html',
    opts = {'version': version}
)

sphinx_build(
    name = 'pdf',
    out = 'kRPC.pdf',
    srcs = [':srcs'],
    sphinx_build = ':sphinx-build',
    builder = 'latex',
    opts = {'version': version},
    visibility = ['//:__pkg__']
)

png_images(
    name = 'images',
    srcs = glob(['src/images/**/*.svg'])
)

defs = [
    '//server:ServiceDefinitions',
    '//service/SpaceCenter:ServiceDefinitions',
    '//service/Drawing:ServiceDefinitions',
    '//service/InfernalRobotics:ServiceDefinitions',
    '//service/KerbalAlarmClock:ServiceDefinitions',
    '//service/RemoteTech:ServiceDefinitions',
    '//service/UI:ServiceDefinitions',
    '//service/LiDAR:ServiceDefinitions',
    '//service/DockingCamera:ServiceDefinitions'
]

docgen_multiple(
    name = 'cnano-api',
    outdir = 'src/cnano',
    language = 'cnano',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

docgen_multiple(
    name = 'csharp-api',
    outdir = 'src/csharp',
    language = 'csharp',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

docgen_multiple(
    name = 'cpp-api',
    outdir = 'src/cpp',
    language = 'cpp',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

docgen_multiple(
    name = 'java-api',
    outdir = 'src/java',
    language = 'java',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

docgen_multiple(
    name = 'lua-api',
    outdir = 'src/lua',
    language = 'lua',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

docgen_multiple(
    name = 'python-api',
    outdir = 'src/python',
    language = 'python',
    srcs = glob(['api/**/*.tmpl']),
    defs = defs
)

test_suite(
    name = 'test',
    tests = [':spelling', ':check-documented', ':lint'] # ':linkcheck'
)

test_suite(
    name = 'ci-test',
    tests = [':spelling', ':check-documented']
)

sphinx_spelling_test(
    name = 'spelling',
    srcs = [':srcs'],
    sphinx_build = ':sphinx-build',
    opts = {'version': version},
    size = 'medium'
)

sphinx_linkcheck_test(
    name = 'linkcheck',
    srcs = [':srcs'],
    sphinx_build = ':sphinx-build',
    opts = {'version': version},
    tags = ['requires-network']
)

test_suite(
    name = 'check-documented',
    tests = [
        ':check-documented-cnano',
        ':check-documented-csharp',
        ':check-documented-cpp',
        ':check-documented-java',
        ':check-documented-lua',
        ':check-documented-python'
    ]
)

check_documented_test(
    name = 'check-documented-cnano',
    srcs = [':cnano-api'],
    members = 'order.txt',
    size = 'small'
)

check_documented_test(
    name = 'check-documented-csharp',
    srcs = [':csharp-api'],
    members = 'order.txt',
    size = 'small'
)

check_documented_test(
    name = 'check-documented-cpp',
    srcs = [':cpp-api'],
    members = 'order.txt',
    size = 'small'
)

check_documented_test(
    name = 'check-documented-java',
    srcs = [':java-api'],
    members = 'order.txt',
    size = 'small'
)

check_documented_test(
    name = 'check-documented-lua',
    srcs = [':lua-api'],
    members = 'order.txt',
    size = 'small'
)

check_documented_test(
    name = 'check-documented-python',
    srcs = [':python-api'],
    members = 'order.txt',
    size = 'small'
)

filegroup(
    name = 'compile-scripts',
    srcs = [
        ':compile-scripts-cnano',
        ':compile-scripts-csharp',
        ':compile-scripts-cpp',
        ':compile-scripts-java'
    ]
)

filegroup(
    name = 'compile-scripts-csharp',
    srcs = [
        ':compile-scripts-csharp-exes',
        ':compile-scripts-csharp-libs'
    ]
)

cc_binary_multiple(
    name = 'compile-scripts-cnano',
    srcs = glob(['src/scripts/**/*.c']),
    deps = ['//client/cnano:krpc_cnano']
)

csharp_binary_multiple(
    name = 'compile-scripts-csharp-exes',
    srcs = glob(['src/scripts/**/*.cs'], exclude=['src/scripts/**/*.lib.cs']),
    deps = [
        '//client/csharp:KRPC.Client',
        '//tools/build/mono-4.5:Google.Protobuf',
        '//tools/build/mono-4.5:mscorlib',
        '//tools/build/mono-4.5:System',
        '//tools/build/mono-4.5:System.Core',
        '//tools/build/mono-4.5:System.Runtime',
        '//tools/build/mono-4.5:System.IO',
        '//tools/build/mono-4.5:System.Xml'
    ]
)

csharp_library_multiple(
    name = 'compile-scripts-csharp-libs',
    srcs = glob(['src/scripts/**/*.lib.cs']),
    deps = [
        '//server:KRPC',
        '//tools/build/ksp:Google.Protobuf',
        '//tools/build/ksp:Assembly-CSharp',
        '//tools/build/ksp:Assembly-CSharp-firstpass',
        '//tools/build/ksp:UnityEngine',
        '//tools/build/ksp:UnityEngine.AnimationModule',
        '//tools/build/ksp:UnityEngine.CoreModule',
        '//tools/build/ksp:UnityEngine.IMGUIModule',
        '//tools/build/ksp:UnityEngine.InputLegacyModule',
        '//tools/build/ksp:UnityEngine.UI',
        '//tools/build/ksp:mscorlib',
        '//tools/build/ksp:System',
        '//tools/build/ksp:System.Core',
        '//tools/build/ksp:System.Xml'
    ]
)

cc_binary_multiple(
    name = 'compile-scripts-cpp',
    srcs = glob(['src/scripts/**/*.cpp']),
    deps = ['//client/cpp:krpc']
)

java_binary_multiple(
    name = 'compile-scripts-java',
    srcs = glob(['src/scripts/**/*.java']),
    deps = [
        '//client/java:krpc-jar',
        '@java_protobuf//jar',
        '@java_javatuples//jar'
    ]
)

test_suite(
    name = 'lint',
    tests = [
        ':lint-python'
    ]
)

py_lint_test(
    name = 'lint-python',
    srcs = glob(['src/scripts/*.py'], exclude=['src/scripts/communication-protocol.py']),
    pycodestyle_config = 'src/scripts/pycodestyle.ini',
    pylint_config = 'src/scripts/pylint.rc',
    size = 'small'
)
