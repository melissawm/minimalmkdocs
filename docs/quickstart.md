# Quickstart

The Pokédex package contains basic information about the three
[starter Pokémon](https://bulbapedia.bulbagarden.net/wiki/Starter_Pok%C3%A9mon)
from the [core series](https://bulbapedia.bulbagarden.net/wiki/Core_series). <!--These are detailed in [](apidocs).-->

## About starter Pokémon

![Starter Pokémon from the core series.](starter_pokemon.png)

_Starter Pokémon from the core series._

As you can see in the image above, these starter Pokémon have
different types and abilities, and will evolve into different creatures as their
level progresses.

!!! warning

    Choosing a starter Pokémon is an important decision that will affect your journey. Choose carefully!

### Base Stats

The base stats for these Pokémon can be obtained from the general [base stats list](https://bulbapedia.bulbagarden.net/wiki/List_of_Pok%C3%A9mon_by_base_stats_(Generation_I)). If you need to compute total damage done in battle or the
current HP for a given Pokémon, you can use [NumPy array](https://numpy.org/doc/stable/reference/generated/numpy.array.html)
objects.

| Pokémon     | HP | Attack | Defense | Speed | Special | Total | Average |
|-------------|----:|-------:|--------:|------:|--------:|------:|--------:|
| Bulbasaur   |  45 |     49 |     49  |    45 |      65 |  253 |    50.6 |
| Charmander  |  39 |     52 |     43  |    65 |      50 |  249 |    49.8 |
| Squirtle    |  44 |     48 |     65  |    43 |      50 |  250 |    50.0 |

### Pokémon details

For these three Pokémon, you can check out their pokédex entries below.

=== "Bulbasaur"
    - Seed Pokémon
    - Type: `grass` `poison`
    - Abilities: Overgrow, Chlorophyll

=== "Charmander"
    - Lizard Pokémon
    - Type: `fire`
    - Abilities: Blaze, Solar power

=== "Squirtle"
    - Tiny turtle Pokémon
    - Type: `water`
    - Abilities: Torrent, Rain dish

## Usage

You can create an instance of Bulbasaur called `friend`, for example, by doing

```python
   >>> import pokedex
   >>> friend = pokedex.Bulbasaur()
```
