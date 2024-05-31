project('tqftpserv',
        'c',
        default_options: [
            'warning_level=1',
            'buildtype=release',
        ])

prefix = get_option('prefix')

# Not required to build the executable, only to install unit file
systemd = dependency('systemd', required : false)
if systemd.found()
        systemd_system_unit_dir = get_option('systemd-unit-prefix')
        if systemd_system_unit_dir == ''
                systemd_system_unit_dir = systemd.get_variable(
                        pkgconfig : 'systemdsystemunitdir',
                        pkgconfig_define: ['prefix', prefix])
        endif
endif

inc = include_directories('include')

tqftpserv_srcs = ['translate.c',
                  'tqftpserv.c',
		'qrtr.c',
		'qmi.c',
		'logging.c'
		]
executable('tqftpserv',
           tqftpserv_srcs,
           include_directories : inc,
#           link_with : libqrtr,
           link_args: ['-static','-static-libgcc'],
           install : true)

if systemd.found()
        systemd_unit_conf = configuration_data()
        systemd_unit_conf.set('prefix', prefix)
        configure_file(
                input : 'tqftpserv.service.in',
                output : 'tqftpserv.service',
                configuration : systemd_unit_conf,
                install_dir : systemd_system_unit_dir)
endif
