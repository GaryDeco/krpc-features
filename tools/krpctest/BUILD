load('//tools/build:python.bzl', 'py_sdist', 'py_test', 'py_lint_test')
load('//:config.bzl', 'version', 'python_version')

py_sdist(
    name = 'krpctest',
    out = 'krpctest-%s.zip' % version,
    files = [
        'README.txt', '//:version', 'CHANGES.txt',
        'LICENSE', '//:COPYING',
        'setup.py', 'MANIFEST.in', '//:python_version',
        'krpctest/krpctest.sfs',
        'krpctest/krpctest_career.sfs',
    ] + glob(['krpctest/**/*.py']),
    path_map = {
        'version.py': 'krpctest/version.py',
        'tools/krpctest/': ''
    },
    visibility = ['//visibility:public']
)

test_suite(
    name = 'test',
    tests = [':krpctesttest', ':lint']
)

test_suite(
    name = 'ci-test',
    tests = [':krpctesttest']
)

deps = [
    '//client/python',
    '@python_protobuf//file'
]

py_test(
    name = 'krpctesttest',
    src = ':krpctest',
    pkg = 'krpctest-'+python_version,
    deps = deps,
    size = 'small'
)

py_lint_test(
    name = 'lint',
    pkg = ':krpctest',
    pkg_name = 'krpctest',
    deps = deps,
    pylint_config = 'pylint.rc',
    size = 'small'
)
