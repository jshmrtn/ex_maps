# ExMaps

[![Build Status](https://travis-ci.org/Kociamber/ex_maps.svg?branch=master)](https://travis-ci.org/Kociamber/ex_maps)
[![Hex version badge](https://img.shields.io/hexpm/v/ex_maps.svg)](https://hex.pm/packages/ex_maps)

**Fast, industrial strength, concurrent Google Maps interface for Elixir platforms.**

## Overivew

The wrapper is concurrent, which means that every coordinates entry on param list will spawn separate Elixir process (task) to retrieve the data from Google API or the cache if query has
been already sent.

The application is using super fast generational caching lib [Nebulex](https://github.com/cabol/nebulex)
Since its latest version is rc3, this package is also treated as release candidate.

## Installation

Add ExMaps as a dependency to your `mix.exs` file:

```elixir
defp deps() do
  [{:ex_maps, "1.0.0-rc.1"}]
end
```

## Configuration

In order to be able to use Google API, you need to get free or commercial [key](https://developers.google.com/maps/documentation/directions/get-api-key).
Please then create environmental variable MAPS_API_KEY with the value of your key.

If you are going to use this application as a dependency in your own project, you will need to copy and paste below configuration to your `config/config.exs` file:

```elixir
config :ex_maps, api_key: System.get_env("MAPS_API_KEY")

config :ex_maps, ExMaps.Cache,
  adapter: Nebulex.Adapters.Local,
  n_shards: 2,
  gc_interval: 3600
```

..and you are ready to go!

## Basic Usage

Please note that free Google API key has some limitations and it will be easy to exceed query limit per second.
First param is a list of coordinates, second one is a list of additional options explained on [hexdocs](https://hexdocs.pm/ex_maps/readme.html) and  [Google Dev Guide](https://developers.google.com/maps/documentation/directions/intro).

```elixir
ExMaps.get_directions([%{origin: "Warsaw", destination: "Amsterdam"}, destination: {52.3719729, 4.8903469}}], units: :metric)
{:ok, %{WEATHER_DATA}}
```

You can of course mix coordinates types, ie. cities names, latitude / longitude, etc.

```elixir
ExMaps.get_directions([%{origin: "Warsaw", destination: "Amsterdam"}, %{origin: {52.3714894, 4.8957388}, destination: {52.3719729, 4.8903469}}], units: :metric)
{:ok, %{WEATHER_DATA}}
```

-------

## To do

*   Simplify configuration
*   Expand test and docs coverage

## License

This project is MIT licensed. Please see the [`LICENSE.md`](https://github.com/Kociamber/ex_maps/blob/master/LICENSE.md) file for more details.
