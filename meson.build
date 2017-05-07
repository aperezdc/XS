project('xs', 'cpp')
boost_dep = dependency('boost')
compiler = meson.get_compiler('cpp')
ffi_lib = compiler.find_library('ffi')
gc_lib = compiler.find_library('gc')
readline_lib = compiler.find_library('readline')
custom_target('git_date.hxx',
	build_always: true,
	output: 'git_date.hxx',
	command: ['../git_date.sh'])
custom_target('git_hash.hxx',
	build_always: true,
	output: 'git_hash.hxx',
	command: ['../git_hash.sh'])
custom_target('git_url.hxx',
	build_always: true,
	output: 'git_url.hxx',
	command: ['../git_url.sh'])
parse_cxx = custom_target('parse.cxx',
	output: ['parse.cxx', 'parse.hxx'],
	command: ['../mkparse.sh'])
sigmsgs_cxx = custom_target('sigmsgs.cxx',
	output: 'sigmsgs.cxx',
	command: ['../mksignal.sh', '@OUTPUT@'])
xsdump = executable('xsdump', ['access.cxx', 'closure.cxx', 'conv.cxx',
		'dump.cxx', 'eval.cxx', 'fd.cxx', 'glob.cxx', 'glom.cxx',
		'heredoc.cxx', 'input.cxx', 'list.cxx', 'main.cxx',
		'match.cxx', 'opt.cxx', parse_cxx, 'prim-ctl.cxx',
		'prim.cxx', 'prim-etc.cxx', 'prim-io.cxx', 'prim-rel.cxx',
		'prim-sys.cxx', 'print.cxx', 'proc.cxx', sigmsgs_cxx,
		'signal.cxx', 'split.cxx', 'status.cxx', 'str.cxx',
		'syntax.cxx', 'term.cxx', 'token.cxx', 'tree.cxx',
		'util.cxx', 'var.cxx', 'version.cxx',],
	dependencies: [boost_dep, readline_lib, gc_lib, ffi_lib])
initial_cxx = custom_target('initial.cxx',
	input: 'initial.xs',
	output: 'initial.cxx',
	depends: xsdump,
	command: ['../initial.sh', '@INPUT@', '@OUTPUT@'])
xs = executable('xs', ['access.cxx', 'closure.cxx', 'conv.cxx', 'eval.cxx',
		'fd.cxx', 'glob.cxx', 'glom.cxx', 'heredoc.cxx',
		initial_cxx, 'input.cxx', 'list.cxx', 'main.cxx',
		'match.cxx', 'opt.cxx', parse_cxx, 'prim-ctl.cxx',
		'prim.cxx', 'prim-etc.cxx', 'prim-io.cxx', 'prim-rel.cxx',
		'prim-sys.cxx', 'print.cxx', 'proc.cxx', sigmsgs_cxx,
		'signal.cxx', 'split.cxx', 'status.cxx', 'str.cxx',
		'syntax.cxx', 'term.cxx', 'token.cxx', 'tree.cxx',
		'util.cxx', 'var.cxx', 'version.cxx',],
	dependencies: [boost_dep, readline_lib, gc_lib, ffi_lib],
	install: true)
install_man('xs.1')
install_data(['AUTHORS', 'CHANGES', 'COPYING', 'GOTCHAS', 'INSTALL',
		'NOTES', 'README', 'SYSTEMD_LOCALE'],
	install_dir: 'share/doc/xs')
run_target('check',
	command: ['sh', '-c', './test.xs'],
	depends: xs)
run_target('fuzz',
	command: ['zzuf', '-O', 'copy', '-s', '1:10000', '-r', '0.001:0.050',
		'-vic', xs, '-p', '-n', '~/.xsrc'])
