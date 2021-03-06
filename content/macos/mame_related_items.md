---
Title: macOS - MAME Related Items
Description: A cheat sheet for MAME related items.
Author: Jack Szwergold
Date: 2015-10-07
Robots: noindex,nofollow
Template: index
---

### Find all files that have not been touched in the last 3 hours.

Dry run:
    
    find /Applications/mame*-64bit/roms -maxdepth 1 -type f -name "*.zip" -mtime +3 -exec ls -latr {} \;

Actual command using `advzip`:
    
    cd /Applications/advancecomp-1.19

    find /Applications/mame*-64bit/roms -maxdepth 1 -type f -name "*.zip" -mtime +3 -exec ./advzip -z -3  {} \;
    
### Find all files that have been touched in the last hour.

Dry run:
    
    find /Applications/mame*-64bit/roms -maxdepth 1 -type f -name "*.zip" -mtime -1 -exec ls -latr {} \;

Actual command using `advzip`:
    
    cd /Applications/advancecomp-1.19

    find /Applications/mame*-64bit/roms -maxdepth 1 -type f -name "*.zip" -mtime -1 -exec ./advzip -z -3  {} \;
    
### Verify ROMs.

Get a verification of all ROM files:

    cd /Applications/mame*-64bit/

    ./mame64 -verifyroms > ./mame-roms-all.txt

Create a list of bad ROM files:

    cd /Applications/mame*-64bit/

    ./mame64 -verifyroms | grep "is bad" > ./mame-roms-bad.txt

Gather a list of bad ROM files:

    cd /Applications/mame*-64bit/

    ./mame64 -verifyroms | grep "is bad" | awk '{print $2}'
    
### Clean out bad ROMs.

First draft of a `find` script to roll through the ROMs:

    find '/Applications/mame*-64bit/roms' -type f -name '*.zip' |\
      while read FULL_FILEPATH
      do
        # Parse the directory name, extension & filename.
        DIRNAME=$(dirname "${FULL_FILEPATH}")
        BASENAME=$(basename "${FULL_FILEPATH}")
        FILENAME="${BASENAME%.*}"
        EXTENSION="${BASENAME##*.}"
        echo "${FILENAME}"
      done

Another idea for a script to get rid of bad ROMs. Dry run with `echo`:

    cd /Applications/mame*-64bit/

    ./mame64 -verifyroms | grep "is bad" | awk '{print $2}' |\
      while read ROM_NAME
      do
        ROM_FULLPATH="/Applications/mame*-64bit/roms"/"${ROM_NAME}".zip
        if [[ -f "${ROM_FULLPATH}" ]]; then
          echo "${ROM_FULLPATH}"
        fi
      done

Actual script:

    cd /Applications/mame*-64bit/

    ./mame64 -verifyroms | grep "is bad" | awk '{print $2}' |\
      while read ROM_NAME
      do
        ROM_FULLPATH="/Applications/mame*-64bit/roms"/"${ROM_NAME}".zip
        if [[ -f "${ROM_FULLPATH}" ]]; then
          rm "${ROM_FULLPATH}"
        fi
      done
    
### Using AdvanceSCAN ROMs manager stuff.

Generate MAME info in XML format:

    cd /Applications/mame*-64bit/

    ./mame64 -listxml > ./mame-info.xml

Now run AdvanceSCAN stuff to repair and clean the files:

    cd /Applications/advancescan-1.17/

    ./advscan -R < ../mame*-64bit/mame-info.xml
    
### MAME command line options.

Show them using `-showusage` like this:

    ./mame64 -showusage

And this is the output:

    Usage:  mame64 [machine] [media] [software] [options]
    
    Options:
    
    #
    # CORE CONFIGURATION OPTIONS
    #
    -readconfig          enable loading of configuration files
    -writeconfig         writes configuration to (driver).ini on exit
    
    #
    # CORE SEARCH PATH OPTIONS
    #
    -rompath             path to ROMsets and hard disk images
    -hashpath            path to hash files
    -samplepath          path to samplesets
    -artpath             path to artwork files
    -ctrlrpath           path to controller definitions
    -inipath             path to ini files
    -fontpath            path to font files
    -cheatpath           path to cheat files
    -crosshairpath       path to crosshair files
    
    #
    # CORE OUTPUT DIRECTORY OPTIONS
    #
    -cfg_directory       directory to save configurations
    -nvram_directory     directory to save nvram contents
    -input_directory     directory to save input device logs
    -state_directory     directory to save states
    -snapshot_directory  directory to save screenshots
    -diff_directory      directory to save hard drive image difference files
    -comment_directory   directory to save debugger comments
    
    #
    # CORE STATE/PLAYBACK OPTIONS
    #
    -state               saved state to load
    -autosave            enable automatic restore at startup, and automatic save at exit time
    -playback            playback an input file
    -record              record an input file
    -mngwrite            optional filename to write a MNG movie of the current session
    -aviwrite            optional filename to write an AVI movie of the current session
    -wavwrite            optional filename to write a WAV file of the current session
    -snapname            override of the default snapshot/movie naming; %g == gamename, %i == index
    -snapsize            specify snapshot/movie resolution (<width>x<height>) or 'auto' to use minimal size
    -snapview            specify snapshot/movie view or 'internal' to use internal pixel-aspect views
    -snapbilinear        specify if the snapshot/movie should have bilinear filtering applied
    -statename           override of the default state subfolder naming; %g == gamename
    -burnin              create burn-in snapshots for each screen
    
    #
    # CORE PERFORMANCE OPTIONS
    #
    -autoframeskip       enable automatic frameskip selection
    -frameskip           set frameskip to fixed value, 0-10 (autoframeskip must be disabled)
    -seconds_to_run      number of emulated seconds to run before automatically exiting
    -throttle            enable throttling to keep game running in sync with real time
    -sleep               enable sleeping, which gives time back to other applications when idle
    -speed               controls the speed of gameplay, relative to realtime; smaller numbers are slower
    -refreshspeed        automatically adjusts the speed of gameplay to keep the refresh rate lower than the screen
    
    #
    # CORE ROTATION OPTIONS
    #
    -rotate              rotate the game screen according to the game's orientation needs it
    -ror                 rotate screen clockwise 90 degrees
    -rol                 rotate screen counterclockwise 90 degrees
    -autoror             automatically rotate screen clockwise 90 degrees if vertical
    -autorol             automatically rotate screen counterclockwise 90 degrees if vertical
    -flipx               flip screen left-right
    -flipy               flip screen upside-down
    
    #
    # CORE ARTWORK OPTIONS
    #
    -artwork_crop        crop artwork to game screen size
    -use_backdrops       enable backdrops if artwork is enabled and available
    -use_overlays        enable overlays if artwork is enabled and available
    -use_bezels          enable bezels if artwork is enabled and available
    -use_cpanels         enable cpanels if artwork is enabled and available
    -use_marquees        enable marquees if artwork is enabled and available
    
    #
    # CORE SCREEN OPTIONS
    #
    -brightness          default game screen brightness correction
    -contrast            default game screen contrast correction
    -gamma               default game screen gamma correction
    -pause_brightness    amount to scale the screen brightness when paused
    -effect              name of a PNG file to use for visual effects, or 'none'
    
    #
    # CORE VECTOR OPTIONS
    #
    -antialias           use antialiasing when drawing vectors
    -beam                set vector beam width
    -flicker             set vector flicker effect
    
    #
    # CORE SOUND OPTIONS
    #
    -samplerate          set sound output sample rate
    -samples             enable the use of external samples if available
    -volume              sound volume in decibels (-32 min, 0 max)
    
    #
    # CORE INPUT OPTIONS
    #
    -coin_lockout        enable coin lockouts to actually lock out coins
    -ctrlr               preconfigure for specified controller
    -mouse               enable mouse input
    -joystick            enable joystick input
    -lightgun            enable lightgun input
    -multikeyboard       enable separate input from each keyboard device (if present)
    -multimouse          enable separate input from each mouse device (if present)
    -steadykey           enable steadykey support
    -ui_active           enable user interface on top of emulated keyboard (if present)
    -offscreen_reload    convert lightgun button 2 into offscreen reload
    -joystick_map        explicit joystick map, or auto to auto-select
    -joystick_deadzone   center deadzone range for joystick where change is ignored (0.0 center, 1.0 end)
    -joystick_saturation end of axis saturation range for joystick where change is ignored (0.0 center, 1.0 end)
    -natural             specifies whether to use a natural keyboard or not
    -joystick_contradictoryenable contradictory direction digital joystick input at the same time
    -coin_impulse        set coin impulse time (n<0 disable impulse, n==0 obey driver, 0<n set time n)
    
    #
    # CORE INPUT AUTOMATIC ENABLE OPTIONS
    #
    -paddle_device       enable (none|keyboard|mouse|lightgun|joystick) if a paddle control is present
    -adstick_device      enable (none|keyboard|mouse|lightgun|joystick) if an analog joystick control is present
    -pedal_device        enable (none|keyboard|mouse|lightgun|joystick) if a pedal control is present
    -dial_device         enable (none|keyboard|mouse|lightgun|joystick) if a dial control is present
    -trackball_device    enable (none|keyboard|mouse|lightgun|joystick) if a trackball control is present
    -lightgun_device     enable (none|keyboard|mouse|lightgun|joystick) if a lightgun control is present
    -positional_device   enable (none|keyboard|mouse|lightgun|joystick) if a positional control is present
    -mouse_device        enable (none|keyboard|mouse|lightgun|joystick) if a mouse control is present
    
    #
    # CORE DEBUGGING OPTIONS
    #
    -verbose             display additional diagnostic information
    -log                 generate an error.log file
    -oslog               output error.log data to the system debugger
    -debug               enable/disable debugger
    -update_in_pause     keep calling video updates while in pause
    -debugscript         script for debugger
    
    #
    # CORE COMM OPTIONS
    #
    -comm_localhost      local address to bind to
    -comm_localport      local port to bind to
    -comm_remotehost     remote address to connect to
    -comm_remoteport     remote port to connect to
    
    #
    # CORE MISC OPTIONS
    #
    -drc                 enable DRC cpu core if available
    -drc_use_c           force DRC use C backend
    -drc_log_uml         write DRC UML disassembly log
    -drc_log_native      write DRC native disassembly log
    -bios                select the system BIOS to use
    -cheat               enable cheat subsystem
    -skip_gameinfo       skip displaying the information screen at startup
    -uifont              specify a font to use
    -ramsize             size of RAM (if supported by driver)
    -confirm_quit        display confirm quit screen on exit
    -ui_mouse            display ui mouse cursor
    -autoboot_command    command to execute after machine boot
    -autoboot_delay      timer delay in sec to trigger command execution on autoboot
    -autoboot_script     lua script to execute after machine boot
    -http                enable local http server
    -http_port           http server listener port
    -http_path           path to web files
    -console             enable emulator LUA console
    
    #
    # CORE COMMANDS
    #
    -help                show help message
    -validate            perform driver validation on all game drivers
    
    #
    # CONFIGURATION COMMANDS
    #
    -createconfig        create the default configuration file
    -showconfig          display running parameters
    -showusage           show this help
    
    #
    # FRONTEND COMMANDS
    #
    -listxml             all available info on driver in XML format
    -listfull            short name, full name
    -listsource          driver sourcefile
    -listclones          show clones
    -listbrothers        show "brothers", or other drivers from same sourcefile
    -listcrc             CRC-32s
    -listroms            list required roms for a driver
    -listsamples         list optional samples for a driver
    -verifyroms          report romsets that have problems
    -verifysamples       report samplesets that have problems
    -romident            compare files with known MAME roms
    -listdevices         list available devices
    -listslots           list available slots and slot devices
    -listmedia           list available media for the system
    -listsoftware        list known software for the system
    -verifysoftware      verify known software for the system
    -getsoftlist         retrieve software list by name
    -verifysoftlist      verify software list by name
    
    #
    # OSD KEYBOARD MAPPING OPTIONS
    #
    -uimodekey           Key to toggle keyboard mode
    
    #
    # OSD FONT OPTIONS
    #
    -uifontprovider      provider for ui font: osx or none
    
    #
    # OSD CLI OPTIONS
    #
    -listmidi            list available MIDI I/O devices
    -listnetwork         list available network adapters
    
    #
    # OSD DEBUGGING OPTIONS
    #
    -debugger            debugger used: osx, internal or none
    -debugger_font       specifies the font to use for debugging
    -debugger_font_size  specifies the font size to use for debugging
    -watchdog            force the program to terminate if no updates within specified number of seconds
    
    #
    # OSD PERFORMANCE OPTIONS
    #
    -multithreading      enable multithreading; this enables rendering and blitting on a separate thread
    -numprocessors       number of processors; this overrides the number the system reports
    -bench               benchmark for the given number of emulated seconds; implies -video none -sound none -nothrottle
    
    #
    # OSD VIDEO OPTIONS
    #
    -video               video output method: none, soft, opengl or bgfx
    -numscreens          number of screens to create; usually, you want just one
    -window              enable window mode; otherwise, full screen mode is assumed
    -maximize            default to maximized windows; otherwise, windows will be minimized
    -keepaspect          constrain to the proper aspect ratio
    -unevenstretch       allow non-integer stretch factors
    -waitvsync           enable waiting for the start of VBLANK before flipping screens; reduces tearing effects
    -syncrefresh         enable using the start of VBLANK for throttling instead of the game time
    
    #
    # OSD PER-WINDOW VIDEO OPTIONS
    #
    -screen              explicit name of the first screen; 'auto' here will try to make a best guess
    -aspect              aspect ratio for all screens; 'auto' here will try to make a best guess
    -resolution          preferred resolution for all screens; format is <width>x<height>[@<refreshrate>] or 'auto'
    -view                preferred view for all screens
    -screen0             explicit name of the first screen; 'auto' here will try to make a best guess
    -aspect0             aspect ratio of the first screen; 'auto' here will try to make a best guess
    -resolution0         preferred resolution of the first screen; format is <width>x<height>[@<refreshrate>] or 'auto'
    -view0               preferred view for the first screen
    -screen1             explicit name of the second screen; 'auto' here will try to make a best guess
    -aspect1             aspect ratio of the second screen; 'auto' here will try to make a best guess
    -resolution1         preferred resolution of the second screen; format is <width>x<height>[@<refreshrate>] or 'auto'
    -view1               preferred view for the second screen
    -screen2             explicit name of the third screen; 'auto' here will try to make a best guess
    -aspect2             aspect ratio of the third screen; 'auto' here will try to make a best guess
    -resolution2         preferred resolution of the third screen; format is <width>x<height>[@<refreshrate>] or 'auto'
    -view2               preferred view for the third screen
    -screen3             explicit name of the fourth screen; 'auto' here will try to make a best guess
    -aspect3             aspect ratio of the fourth screen; 'auto' here will try to make a best guess
    -resolution3         preferred resolution of the fourth screen; format is <width>x<height>[@<refreshrate>] or 'auto'
    -view3               preferred view for the fourth screen
    
    #
    # OSD FULL SCREEN OPTIONS
    #
    -switchres           enable resolution switching
    
    #
    # OSD ACCELERATED VIDEO OPTIONS
    #
    -filter              enable bilinear filtering on screen output
    -prescale            scale screen rendering by this amount in software
    
    #
    # OpenGL-SPECIFIC OPTIONS
    #
    -gl_forcepow2texture force power of two textures  (default no)
    -gl_notexturerect    don't use OpenGL GL_ARB_texture_rectangle (default on)
    -gl_vbo              enable OpenGL VBO,  if available (default on)
    -gl_pbo              enable OpenGL PBO,  if available (default on)
    -gl_glsl             enable OpenGL GLSL, if available (default off)
    -gl_glsl_filter      enable OpenGL GLSL filtering instead of FF filtering 0-plain, 1-bilinear (default)
    -glsl_shader_mame0   custom OpenGL GLSL shader set mame bitmap 0
    -glsl_shader_mame1   custom OpenGL GLSL shader set mame bitmap 1
    -glsl_shader_mame2   custom OpenGL GLSL shader set mame bitmap 2
    -glsl_shader_mame3   custom OpenGL GLSL shader set mame bitmap 3
    -glsl_shader_mame4   custom OpenGL GLSL shader set mame bitmap 4
    -glsl_shader_mame5   custom OpenGL GLSL shader set mame bitmap 5
    -glsl_shader_mame6   custom OpenGL GLSL shader set mame bitmap 6
    -glsl_shader_mame7   custom OpenGL GLSL shader set mame bitmap 7
    -glsl_shader_mame8   custom OpenGL GLSL shader set mame bitmap 8
    -glsl_shader_mame9   custom OpenGL GLSL shader set mame bitmap 9
    -glsl_shader_screen0 custom OpenGL GLSL shader screen bitmap 0
    -glsl_shader_screen1 custom OpenGL GLSL shader screen bitmap 1
    -glsl_shader_screen2 custom OpenGL GLSL shader screen bitmap 2
    -glsl_shader_screen3 custom OpenGL GLSL shader screen bitmap 3
    -glsl_shader_screen4 custom OpenGL GLSL shader screen bitmap 4
    -glsl_shader_screen5 custom OpenGL GLSL shader screen bitmap 5
    -glsl_shader_screen6 custom OpenGL GLSL shader screen bitmap 6
    -glsl_shader_screen7 custom OpenGL GLSL shader screen bitmap 7
    -glsl_shader_screen8 custom OpenGL GLSL shader screen bitmap 8
    -glsl_shader_screen9 custom OpenGL GLSL shader screen bitmap 9
    
    #
    # OSD SOUND OPTIONS
    #
    -sound               sound output method: coreaudio, sdl or none
    -audio_latency       set audio latency (increase to reduce glitches, decrease for responsiveness)
    
    #
    # CoreAudio-SPECIFIC OPTIONS
    #
    -audio_output        Audio output device
    -audio_effect0       AudioUnit effect 0
    -audio_effect1       AudioUnit effect 1
    -audio_effect2       AudioUnit effect 2
    -audio_effect3       AudioUnit effect 3
    -audio_effect4       AudioUnit effect 4
    -audio_effect5       AudioUnit effect 5
    -audio_effect6       AudioUnit effect 6
    -audio_effect7       AudioUnit effect 7
    -audio_effect8       AudioUnit effect 8
    -audio_effect9       AudioUnit effect 9
    
    #
    # SDL PERFORMANCE OPTIONS
    #
    -sdlvideofps         show sdl video performance
    
    #
    # SDL VIDEO OPTIONS
    #
    -centerh             center horizontally within the view area
    -centerv             center vertically within the view area
    -scalemode           Scale mode: none, hwblit, hwbest, yv12, yuy2, yv12x2, yuy2x2 (-video soft only)
    
    #
    # SDL KEYBOARD MAPPING
    #
    -keymap              enable keymap
    -keymap_file         keymap filename
    
    #
    # SDL JOYSTICK MAPPING
    #
    -joy_idx1            name of joystick mapped to joystick #1
    -joy_idx2            name of joystick mapped to joystick #2
    -joy_idx3            name of joystick mapped to joystick #3
    -joy_idx4            name of joystick mapped to joystick #4
    -joy_idx5            name of joystick mapped to joystick #5
    -joy_idx6            name of joystick mapped to joystick #6
    -joy_idx7            name of joystick mapped to joystick #7
    -joy_idx8            name of joystick mapped to joystick #8
    -sixaxis             Use special handling for PS3 Sixaxis controllers
    
    #
    # SDL MOUSE MAPPING
    #
    -mouse_index1        name of mouse mapped to mouse #1
    -mouse_index2        name of mouse mapped to mouse #2
    -mouse_index3        name of mouse mapped to mouse #3
    -mouse_index4        name of mouse mapped to mouse #4
    -mouse_index5        name of mouse mapped to mouse #5
    -mouse_index6        name of mouse mapped to mouse #6
    -mouse_index7        name of mouse mapped to mouse #7
    -mouse_index8        name of mouse mapped to mouse #8
    
    #
    # SDL KEYBOARD MAPPING
    #
    -keyb_idx1           name of keyboard mapped to keyboard #1
    -keyb_idx2           name of keyboard mapped to keyboard #2
    -keyb_idx3           name of keyboard mapped to keyboard #3
    -keyb_idx4           name of keyboard mapped to keyboard #4
    -keyb_idx5           name of keyboard mapped to keyboard #5
    -keyb_idx6           name of keyboard mapped to keyboard #6
    -keyb_idx7           name of keyboard mapped to keyboard #7
    -keyb_idx8           name of keyboard mapped to keyboard #8
    
    #
    # SDL LOWLEVEL DRIVER OPTIONS
    #
    -videodriver         sdl video driver to use ('x11', 'directfb', ... or 'auto' for SDL default
    -renderdriver        sdl render driver to use ('software', 'opengl', 'directfb' ... or 'auto' for SDL default
    -audiodriver         sdl audio driver to use ('alsa', 'arts', ... or 'auto' for SDL default
    -gl_lib              alternative libGL.so to use; 'auto' for system default
