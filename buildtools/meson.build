# SPDX-License-Identifier: BSD-3-Clause
# Copyright(c) 2017-2019 Intel Corporation

pkgconf = find_program('pkg-config', 'pkgconf', required: false)
pmdinfo = find_program('gen-pmdinfo-cfile.sh')
list_dir_globs = find_program('list-dir-globs.py')
check_symbols = find_program('check-symbols.sh')
ldflags_ibverbs_static = find_program('options-ibverbs-static.sh')
binutils_avx512_check = find_program('binutils-avx512-check.sh')

# set up map-to-win script using python, either built-in or external
python3 = import('python').find_installation(required: false)
if python3.found()
	py3 = [python3]
else
	py3 = ['meson', 'runpython']
endif
map_to_win_cmd = py3 + files('map_to_win.py')
sphinx_wrapper = py3 + files('call-sphinx-build.py')
pmdinfogen = py3 + files('pmdinfogen.py')

# TODO: starting from Meson 0.51.0 use
# 	python3 = import('python').find_installation('python',
#		modules : python3_required_modules)
python3_required_modules = []
if host_machine.system() != 'windows'
	python3_required_modules = ['elftools']
endif
foreach module : python3_required_modules
	script = 'import importlib.util; import sys; exit(importlib.util.find_spec("@0@") is None)'
	if run_command(py3, '-c', script.format(module)).returncode() != 0
		error('missing python module: @0@'.format(module))
	endif
endforeach
