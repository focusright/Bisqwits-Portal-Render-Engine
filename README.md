
# Ported To Web Assembly
Compiled on macOS Catalina 10.15.3 (19D76). The original compilation for the native binary should still work.

# Demo
[Link](https://mclee.com/code/graphics/museum/bisqwit/ds-engine/)

# Movement Inside The Engine
wasd + mouse

# WASM Build Commands

###Optimized
`emcc prender.c -s WASM=1 -s USE_SDL=2 -O3 -o index.js -v --preload-file map-clear.txt`

###Unoptimized (in order to read index.js)
`emcc prender.c -s WASM=1 -s USE_SDL=2 -O0 -g -o index.js -v --preload-file map-clear.txt`

<br/>
<br/>

# Previous README.md starts here

# Bisqwit's Portal Rendering Engine
Bisqwit's Portal Rendering Engine, written in C, updated to use SDL 2 instead of SDL 1.2 and C99 standard.
The code is featured and talked about in [his video on making a Doom style pseudo-3D engine](https://www.youtube.com/watch?v=HQYsFshbkYw)
All files and credit goes to [Bisqwit](https://www.youtube.com/user/Bisqwit)

# Modifications
* Included `inttypes.h` header file.
```c
#include <inttypes.h>
```
* Included `SDL2/SDL.h` header file instead of `SDL.h`
```c
#include <SDL2/SDL.h> // #include <SDL.h>
```
* Changed `unsigned` integer types to `uint32_t`.
```c
//static unsigned NumSectors = 0;
static uint32_t NumSectors = 0;
```
* Changed `int` integers to `int32_t`.
```c
// int n, m, NumVertices = 0;
int32_t n, m, NumVertices = 0;
```
* Enforced const-correctness where possible **and** necessary.
* Applied the `restrict` keyword to pointers that didn't alias similar data.
```c
const struct sector *const restrict sect = &sectors[player.sector];
const struct xy *const restrict vert = sect->vertex;
```
* Changed certain SDL-oriented functions to use their SDL2 counterparts.
* Changed certain SDL code to SDL2 equivalent code.
```c
//SDL_Flip(surface); 
SDL_UpdateTexture(texture, NULL, surface->pixels, surface->pitch);
SDL_RenderClear(renderer);
SDL_RenderCopy(renderer, texture, NULL, NULL);
SDL_RenderPresent(renderer);
```

**This repository is only for the updated source code. The map files and textures are available at [Bisqwit's personal site](https://bisqwit.iki.fi/jutut/kuvat/programming_examples/portalrendering.html)**

### How to Build
To build, you need the SDL2 sdk, a C99 or C11 compliant compiler, and to include the C standard library math library.

the build code I used for quick build:
`gcc -Wall -Wextra -std=c99 -s -O2 prender.c -o bisqwitRT_engine -lSDL2 -lm`
