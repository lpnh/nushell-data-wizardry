# Nushell Data

*Playing with a `dnd.xlsx` file*

```nu
♥  : ls dnd.xlsx
╭───┬──────────┬──────┬───────────┬───────────────╮
│ # │   name   │ type │   size    │   modified    │
├───┼──────────┼──────┼───────────┼───────────────┤
│ 0 │ dnd.xlsx │ file │ 210.4 KiB │ 8 minutes ago │
╰───┴──────────┴──────┴───────────┴───────────────╯
```

## Basic Commands

### Columns name

```nu
♥  : open --raw dnd.xlsx | from xlsx | columns
╭───┬─────────╮
│ 0 │ Spells  │
│ 1 │ Summons │
╰───┴─────────╯
```

### First 5 rows from Spells column

```nu
♥  : open --raw dnd.xlsx | from xlsx | get Spells | first 5
╭────┬────────────────┬──────────┬──────────────┬───────────────┬───────────────┬─────────────┬─────╮
│  # │    column0     │ column1  │   column2    │    column3    │    column4    │   column5   │ ... │
├────┼────────────────┼──────────┼──────────────┼───────────────┼───────────────┼─────────────┼─────┤
│  0 │ Name           │ Level    │ School       │ Casting Time  │ Duration      │ Range       │ ... │
│  1 │ Acid Splash    │     0.00 │ Conjuration  │ 1 Action      │ Instantaneous │ 60 ft       │ ... │
│  2 │ Blade Ward     │     0.00 │ Abjuration   │ 1 Action      │ 1 Round       │ Self        │ ... │
│  3 │ Booming Blade  │     0.00 │ Evocation    │ 1 Action      │ 1 Round       │ Self (5 ft) │ ... │
│  4 │ Chill Touch    │     0.00 │ Necromancy   │ 1 Action      │ 1 Round       │ 120 ft      │ ... │
╰────┴────────────────┴──────────┴──────────────┴───────────────┴───────────────┴─────────────┴─────╯
```

### First row of the table as column names

```nu
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | first 5
╭────┬─────────────────┬────────┬────────────────┬──────────────┬───────────────┬─────────────┬─────╮
│  # │      Name       │ Level  │     School     │ Casting Time │   Duration    │    Range    │ ... │
├────┼─────────────────┼────────┼────────────────┼──────────────┼───────────────┼─────────────┼─────┤
│  0 │ Acid Splash     │   0.00 │ Conjuration    │ 1 Action     │ Instantaneous │ 60 ft       │ ... │
│  1 │ Blade Ward      │   0.00 │ Abjuration     │ 1 Action     │ 1 Round       │ Self        │ ... │
│  2 │ Booming Blade   │   0.00 │ Evocation      │ 1 Action     │ 1 Round       │ Self (5 ft) │ ... │
│  3 │ Chill Touch     │   0.00 │ Necromancy     │ 1 Action     │ 1 Round       │ 120 ft      │ ... │
│  4 │ Control Flames  │   0.00 │ Transmutation  │ 1 Action     │ Instantaneous │ 60 ft       │ ... │
╰────┴─────────────────┴────────┴────────────────┴──────────────┴───────────────┴─────────────┴─────╯
```

### Select only these columns or rows from the input

```nu
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | select 424
╭───┬───────────┬───────┬────────────┬──────────────┬─────────────────┬───────┬──────┬────────┬─────╮
│ # │   Name    │ Level │   School   │ Casting Time │    Duration     │ Range │ Area │ Attack │ ... │
├───┼───────────┼───────┼────────────┼──────────────┼─────────────────┼───────┼──────┼────────┼─────┤
│ 0 │ Magic Jar │  6.00 │ Necromancy │ 1 Minute     │ Until Dispelled │ Self  │      │        │ ... │
╰───┴───────────┴───────┴────────────┴──────────────┴─────────────────┴───────┴──────┴────────┴─────╯
```

```nu
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | select 254 | select Name Details
╭───┬──────────────┬────────────────────────────────────────────────────────────────────────────────╮
│ # │     Name     │                                    Details                                     │
├───┼──────────────┼────────────────────────────────────────────────────────────────────────────────┤
│ 0 │ Remove Curse │ At your touch, all curses affecting one creature or object end. If the object  │
│   │              │ is a cursed magic item, its curse remains, but the spell breaks its owner's    │
│   │              │ attunement to the object so it can be removed or discarded.                    │
╰───┴──────────────┴────────────────────────────────────────────────────────────────────────────────╯
```

### Filter values based on a row condition.

```nu
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | where Name == 'Wish'
╭────┬───────┬────────┬──────────────┬───────────────┬────────────────┬───────┬──────┬────────┬─────╮
│  # │ Name  │ Level  │    School    │ Casting Time  │    Duration    │ Range │ Area │ Attack │ ... │
├────┼───────┼────────┼──────────────┼───────────────┼────────────────┼───────┼──────┼────────┼─────┤
│  0 │ Wish  │   9.00 │ Conjuration  │ 1 Action      │ Instantaneous  │ Self  │      │        │ ... │
╰────┴───────┴────────┴──────────────┴───────────────┴────────────────┴───────┴──────┴────────┴─────╯
```
