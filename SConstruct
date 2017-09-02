from os.path import abspath

env = Environment(CPPPATH=abspath("src"))
env.Append(CXXFLAGS='-g')

qdownloader = SConscript('src/SConscript', variant_dir="bin/", duplicate=0, exports=['env'])

SConscript('test/SConscript', exports=['qdownloader', 'env'])
