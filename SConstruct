# Copyright (c) 2013, AllSeen Alliance. All rights reserved.
#
#    Permission to use, copy, modify, and/or distribute this software for any
#    purpose with or without fee is hereby granted, provided that the above
#    copyright notice and this permission notice appear in all copies.
#
#    THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
#    WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
#    MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
#    ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
#    WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
#    ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
#    OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

import os

env = SConscript('../../core/alljoyn/build_core/SConscript')

vars = Variables()
vars.Add('BINDINGS', 'Bindings to build (comma separated list): cpp, java', 'cpp,java')
vars.Add(EnumVariable('BUILD_SERVICES_SAMPLES',
                      'Build the services samples.',
                      'on',
                      allowed_values = ['on', 'off']))
vars.Add(PathVariable('ALLJOYN_DISTDIR',
                      'Directory containing a built AllJoyn Core dist directory.',
                      os.environ.get('ALLJOYN_DISTDIR')))

vars.Add(PathVariable('NS_DISTDIR',
                      'Directory containing a built AllJoyn Notification dist directory (ALLJOYN_DISTDIR will be used if unset).',
                      os.environ.get('NS_DISTDIR',
                                     ARGUMENTS.get('ALLJOYN_DISTDIR', os.environ.get('ALLJOYN_DISTDIR')))))

vars.Add(PathVariable('APP_COMMON_DIR',
                      'Directory containing common sample application sources.',
                      os.environ.get('APP_COMMON_DIR','../sample_apps')))


vars.Update(env)
Help(vars.GenerateHelpText(env))

if env.get('ALLJOYN_DISTDIR'):
    # normalize ALLJOYN_DISTDIR first
    env['ALLJOYN_DISTDIR'] = env.Dir('$ALLJOYN_DISTDIR')
    env.Append(CPPPATH = [ env.Dir('$ALLJOYN_DISTDIR/cpp/inc'),
                           env.Dir('$ALLJOYN_DISTDIR/about/inc'),
                           env.Dir('$ALLJOYN_DISTDIR/services_common/inc') ])
    env.Append(LIBPATH = [ env.Dir('$ALLJOYN_DISTDIR/cpp/lib'),
                           env.Dir('$ALLJOYN_DISTDIR/about/lib'),
                           env.Dir('$ALLJOYN_DISTDIR/services_common/lib') ])

if env.get('NS_DISTDIR'):
    # normalize NS_DISTDIR first
    env['NS_DISTDIR'] = env.Dir('$NS_DISTDIR')
    env.Append(CPPPATH = [ env.Dir('$NS_DISTDIR/notification/inc') ])
    env.Append(LIBPATH = [ env.Dir('$NS_DISTDIR/notification/lib') ])

if env.get('APP_COMMON_DIR'):
    # normalize APP_COMMON_DIR
    env['APP_COMMON_DIR'] = env.Dir('$APP_COMMON_DIR')

env['bindings'] = set([ b.strip() for b in env['BINDINGS'].split(',') ])

env.SConscript('SConscript')
