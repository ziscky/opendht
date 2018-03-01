project('opendht', 'c', 'cpp', default_options : ['cpp_std=c++11'])
add_global_arguments('--std=c++11', language : 'cpp')

gnutls = dependency('gnutls')
nettle = dependency('nettle')
msgpack = dependency('msgpack')

argon2 = dependency('argon2', required : false)
if not argon2.found()
    argon2_inc = include_directories('argon2/include')
    argon2_src = [
        'argon2/src/argon2.c',
        'argon2/src/core.c',
        'argon2/src/blake2/blake2b.c',
        'argon2/src/thread.c',
        'argon2/src/ref.c',
        'argon2/src/encoding.c'
    ]
    argon2_lib = static_library('argon2', argon2_src)
    argon2 = declare_dependency(link_with : argon2_lib, include_directories : argon2_inc)
endif

opendht_inc = include_directories('include/opendht')
opendht_src = [
    'src/utils.cpp',
    'src/infohash.cpp',
    'src/crypto.cpp',
    'src/default_types.cpp',
    'src/node.cpp',
    'src/value.cpp',
    'src/dht.cpp',
    'src/callbacks.cpp',
    'src/routing_table.cpp',
    'src/node_cache.cpp',
    'src/network_engine.cpp',
    'src/securedht.cpp',
    'src/dhtrunner.cpp',
    'src/log.cpp'
]
opendht = shared_library('opendht', opendht_src, include_directories : opendht_inc, dependencies : [gnutls, nettle, msgpack, argon2])

readline = meson.get_compiler('c').find_library('readline')
executable('dhtnode', 'tools/dhtnode.cpp', link_with : opendht, dependencies : [readline])
