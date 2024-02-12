## byond-tracy
byond-tracy glues together a byond server with the tracy profiler allowing you to analyze and visualize proc calls

NOTE: This branch supports only 1630 and further versions due to proc structure changes in 1624.

## supported byond versions
| windows  | linux    |
| -------- | -------- |
| 515.1630 | 515.1630 |


## supported tracy versions
`0.8.1` `0.8.2` `0.9.0` `0.9.1` `0.10.0`

## usage
simply call `init` from `prof.dll` to begin collecting profile data and connect using [tracy-server](https://github.com/wolfpld/tracy/releases) `Tracy.exe`
```dm
/proc/prof_init()
	var/lib

	switch(world.system_type)
		if(MS_WINDOWS) lib = "prof.dll"
		if(UNIX) lib = "libprof.so"
		else CRASH("unsupported platform")

	var/init = call_ext(lib, "init")()
	if("0" != init) CRASH("[lib] init error: [init]")

/world/New()
	prof_init()
	. = ..()
```

## env vars
set these env vars before launching dreamdaemon to control which node and service to bind
```console
UTRACY_BIND_ADDRESS
```

```console
UTRACY_BIND_PORT
```

## building
no build system included, simply invoke your preferred c11 compiler.
examples:
```console
cl.exe /nologo /std:c11 /O2 /LD /DNDEBUG prof.c ws2_32.lib /Fe:prof.dll
```

```console
clang.exe -std=c11 -m32 -shared -Ofast3 -DNDEBUG -fuse-ld=lld-link prof.c -lws2_32 -o prof.dll
```

```console
gcc -std=c11 -m32 -shared -fPIC -Ofast -s -DNDEBUG prof.c -pthread -o libprof.so
```

## remarks
byond-tracy is in its infancy and is not production ready for live servers.
