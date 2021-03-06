wayfire_sources = ['main.cpp',
                   'util.cpp',

                   'core/output-layout.cpp',
                   'core/matcher.cpp',
                   'core/object.cpp',
                   'core/opengl.cpp',
                   'core/plugin.cpp',
                   'core/core.cpp',
                   'core/img.cpp',
                   'core/wm.cpp',
                   'core/view-access-interface.cpp',

                   'core/seat/pointing-device.cpp',
                   'core/seat/input-manager.cpp',
                   'core/seat/keyboard.cpp',
                   'core/seat/pointer.cpp',
                   'core/seat/cursor.cpp',
                   'core/seat/switch.cpp',
                   'core/seat/tablet.cpp',
                   'core/seat/touch.cpp',
                   'core/seat/seat.cpp',

                   'view/surface.cpp',
                   'view/subsurface.cpp',
                   'view/view.cpp',
                   'view/view-impl.cpp',
                   'view/xdg-shell.cpp',
                   'view/xwayland.cpp',
                   'view/layer-shell.cpp',
                   'view/view-3d.cpp',
                   'view/compositor-view.cpp',

                   'output/plugin-loader.cpp',
                   'output/output.cpp',
                   'output/render-manager.cpp',
                   'output/workspace-impl.cpp',
                   'output/wayfire-shell.cpp',
                   'output/gtk-shell.cpp']

wayfire_dependencies = [wayland_server, wlroots, xkbcommon, libinput,
                       pixman, drm, egl, glesv2, glm, wf_protos,
                       wfconfig, libinotify, backtrace, wfutils, xcb]

if conf_data.get('BUILD_WITH_IMAGEIO')
    wayfire_dependencies += [jpeg, png]
endif

debug_arguments = []

addr2line = find_program('addr2line', required: false)
if addr2line.found()
  debug_arguments += ['-DHAS_ADDR2LINE=1']
else
  debug_arguments += ['-DHAS_ADDR2LINE=0']
endif

if get_option('b_sanitize').contains('address')
  debug_arguments += ['-DASAN_ENABLED']
endif

executable('wayfire', wayfire_sources,
        dependencies: wayfire_dependencies,
        include_directories: [wayfire_conf_inc, wayfire_api_inc],
        cpp_args: debug_arguments,
        link_args: '-ldl',
        install: true)

install_headers(['api/wayfire/nonstd/safe-list.hpp',
                 'api/wayfire/nonstd/noncopyable.hpp',
                 'api/wayfire/nonstd/observer_ptr.h',
                 'api/wayfire/nonstd/reverse.hpp'],
                subdir: 'wayfire/nonstd')

install_headers(['api/wayfire/compositor-surface.hpp',
                 'api/wayfire/compositor-view.hpp',
                 'api/wayfire/bindings.hpp',
                 'api/wayfire/core.hpp',
                 'api/wayfire/debug.hpp',
                 'api/wayfire/decorator.hpp',
                 'api/wayfire/img.hpp',
                 'api/wayfire/geometry.hpp',
                 'api/wayfire/object.hpp',
                 'api/wayfire/opengl.hpp',
                 'api/wayfire/option-wrapper.hpp',
                 'api/wayfire/output.hpp',
                 'api/wayfire/plugin.hpp',
                 'api/wayfire/singleton-plugin.hpp',
                 'api/wayfire/render-manager.hpp',
                 'api/wayfire/signal-definitions.hpp',
                 'api/wayfire/util.hpp',
                 'api/wayfire/surface.hpp',
                 'api/wayfire/view-transform.hpp',
                 'api/wayfire/view.hpp',
                 'api/wayfire/view-access-interface.hpp',
                 'api/wayfire/workspace-manager.hpp',
                 'api/wayfire/workspace-stream.hpp',
                 'api/wayfire/input-device.hpp',
                 'api/wayfire/output-layout.hpp',
                 'api/wayfire/matcher.hpp'],
                subdir: 'wayfire')

pkgconfig = import('pkgconfig')
pkgconfig.generate(
    version:      meson.project_version(),
    filebase:     meson.project_name(),
    name:         meson.project_name(),
    description: 'A Wayland Compositor',
    variables:    ['metadatadir=${prefix}/' + metadata_dir_suffix,
                   'sysconfdir=' + sysconfdir,
                   'plugindir=${libdir}/wayfire',
                   'icondir=${prefix}/share/wayfire/icons']
    )
