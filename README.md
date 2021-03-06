# CSFML

[![CI](https://github.com/JuliaMultimedia/CSFML.jl/actions/workflows/ci.yml/badge.svg)](https://github.com/JuliaMultimedia/CSFML.jl/actions/workflows/ci.yml)
[![TagBot](https://github.com/JuliaMultimedia/CSFML.jl/actions/workflows/TagBot.yml/badge.svg)](https://github.com/JuliaMultimedia/CSFML.jl/actions/workflows/TagBot.yml)
[![codecov](https://codecov.io/gh/JuliaMultimedia/CSFML.jl/branch/master/graph/badge.svg)](https://codecov.io/gh/JuliaMultimedia/CSFML.jl)

Julia wrapper for [CSFML](https://github.com/SFML/CSFML), the official binding of [SFML](https://github.com/SFML/SFML) for C. SFML is a simple, fast, cross-platform and object-oriented multimedia API. It provides access to windowing, graphics, audio and network. The Julia bindings in this repo are auto-generated using [Clang.jl](https://github.com/JuliaInterop/Clang.jl).

## Installation
```julia
pkg> add CSFML
```

## Quick start

```julia
using CSFML
using CSFML.LibCSFML

mode = sfVideoMode(1280, 720, 32)

window = sfRenderWindow_create(mode, "SFML window", sfResize | sfClose, C_NULL)
@assert window != C_NULL

texture = sfTexture_createFromFile(joinpath(dirname(pathof(CSFML)), "..", "examples", "julia-tan.png"), C_NULL)
@assert texture != C_NULL

sprite = sfSprite_create()
sfSprite_setTexture(sprite, texture, sfTrue)

font = sfFont_createFromFile(joinpath(dirname(pathof(CSFML)), "..", "examples", "Roboto-Bold.ttf"))
@assert font != C_NULL

text = sfText_create()
sfText_setString(text, "Hello SFML")
sfText_setFont(text, font)
sfText_setCharacterSize(text, 50)

music = sfMusic_createFromFile(joinpath(dirname(pathof(CSFML)), "..", "examples", "Chrono_Trigger.ogg"))
@assert music != C_NULL

sfMusic_play(music)

event_ref = Ref{sfEvent}()

while Bool(sfRenderWindow_isOpen(window))
    # process events
    while Bool(sfRenderWindow_pollEvent(window, event_ref))
        # close window : exit
        event_ref.x.type == sfEvtClosed && sfRenderWindow_close(window)
    end
    # clear the screen
    sfRenderWindow_clear(window, sfColor_fromRGBA(0,0,0,1))
    # draw the sprite
    sfRenderWindow_drawSprite(window, sprite, C_NULL)
    # draw the text
    sfRenderWindow_drawText(window, text, C_NULL)
    # update the window
    sfRenderWindow_display(window)
end

sfMusic_destroy(music)
sfText_destroy(text)
sfFont_destroy(font)
sfSprite_destroy(sprite)
sfTexture_destroy(texture)
sfRenderWindow_destroy(window)
```

## License
This package is released under zlib/png license except `examples/Chrono_Trigger.ogg` which is
licensed under https://creativecommons.org/licenses/by-sa/3.0/.
