# Filtering

For more information see:
https://www.nushell.sh/commands/categories/filters.html


`columns`: Given a record or table, produce a list of its columns' names.

```
♥  : open --raw dnd.xlsx | from xlsx | columns
╭───┬─────────╮
│ 0 │ Spells  │
│ 1 │ Summons │
╰───┴─────────╯
```

`first`: Return only the first several rows of the input. Counterpart of
`last`. Opposite of `skip`.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | first 5
╭───┬───────────┬─────────┬───────────┬───────────┬────────────┬────────────┬─────╮
│ # │  column0  │ column1 │  column2  │  column3  │  column4   │  column5   │ ... │
├───┼───────────┼─────────┼───────────┼───────────┼────────────┼────────────┼─────┤
│ 0 │ Name      │ Level   │ School    │ Casting   │ Duration   │ Range      │ ... │
│   │           │         │           │ Time      │            │            │     │
│ 1 │ Acid      │    0.00 │ Conjurati │ 1 Action  │ Instantane │ 60 ft      │ ... │
│   │ Splash    │         │ on        │           │ ous        │            │     │
│ 2 │ Blade     │    0.00 │ Abjuratio │ 1 Action  │ 1 Round    │ Self       │ ... │
│   │ Ward      │         │ n         │           │            │            │     │
│ 3 │ Booming   │    0.00 │ Evocation │ 1 Action  │ 1 Round    │ Self (5    │ ... │
│   │ Blade     │         │           │           │            │ ft)        │     │
│ 4 │ Chill     │    0.00 │ Necromanc │ 1 Action  │ 1 Round    │ 120 ft     │ ... │
│   │ Touch     │         │ y         │           │            │            │     │
╰───┴───────────┴─────────┴───────────┴───────────┴────────────┴────────────┴─────╯
```

`headers`: Use the first row of the table as column names.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | first 5
╭───┬───────────┬───────┬────────────┬────────────┬────────────┬────────────┬─────╮
│ # │   Name    │ Level │   School   │ Casting    │  Duration  │   Range    │ ... │
│   │           │       │            │ Time       │            │            │     │
├───┼───────────┼───────┼────────────┼────────────┼────────────┼────────────┼─────┤
│ 0 │ Acid      │  0.00 │ Conjuratio │ 1 Action   │ Instantane │ 60 ft      │ ... │
│   │ Splash    │       │ n          │            │ ous        │            │     │
│ 1 │ Blade     │  0.00 │ Abjuration │ 1 Action   │ 1 Round    │ Self       │ ... │
│   │ Ward      │       │            │            │            │            │     │
│ 2 │ Booming   │  0.00 │ Evocation  │ 1 Action   │ 1 Round    │ Self (5    │ ... │
│   │ Blade     │       │            │            │            │ ft)        │     │
│ 3 │ Chill     │  0.00 │ Necromancy │ 1 Action   │ 1 Round    │ 120 ft     │ ... │
│   │ Touch     │       │            │            │            │            │     │
│ 4 │ Control   │  0.00 │ Transmutat │ 1 Action   │ Instantane │ 60 ft      │ ... │
│   │ Flames    │       │ ion        │            │ ous        │            │     │
╰───┴───────────┴───────┴────────────┴────────────┴────────────┴────────────┴─────╯
```

`select`: Select only these columns or rows from the input. Opposite of `reject`.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | select 424
╭───┬───────────┬───────┬────────────┬──────────────┬───────────────┬───────┬─────╮
│ # │   Name    │ Level │   School   │ Casting Time │   Duration    │ Range │ ... │
├───┼───────────┼───────┼────────────┼──────────────┼───────────────┼───────┼─────┤
│ 0 │ Magic Jar │  6.00 │ Necromancy │ 1 Minute     │ Until         │ Self  │ ... │
│   │           │       │            │              │ Dispelled     │       │     │
╰───┴───────────┴───────┴────────────┴──────────────┴───────────────┴───────┴─────╯
```

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | select 254 |
select Name Details
╭───┬──────────────┬──────────────────────────────────────────────────────────────╮
│ # │     Name     │                           Details                            │
├───┼──────────────┼──────────────────────────────────────────────────────────────┤
│ 0 │ Remove Curse │ At your touch, all curses affecting one creature or object   │
│   │              │ end. If the object is a cursed magic item, its curse         │
│   │              │ remains, but the spell breaks its owner's attunement to the  │
│   │              │ object so it can be removed or discarded.                    │
╰───┴──────────────┴──────────────────────────────────────────────────────────────╯
```

`where`: Filter values based on a row condition.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | where Name == 'Wish'
╭───┬──────┬───────┬─────────────┬─────────────┬─────────────┬───────┬──────┬─────╮
│ # │ Name │ Level │   School    │ Casting     │  Duration   │ Range │ Area │ ... │
│   │      │       │             │ Time        │             │       │      │     │
├───┼──────┼───────┼─────────────┼─────────────┼─────────────┼───────┼──────┼─────┤
│ 0 │ Wish │  9.00 │ Conjuration │ 1 Action    │ Instantaneo │ Self  │      │ ... │
│   │      │       │             │             │ us          │       │      │     │
╰───┴──────┴───────┴─────────────┴─────────────┴─────────────┴───────┴──────┴─────╯
```

`values`: Given a record or table, produce a list of its columns' values.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | get 0 | values
╭────┬───────────────╮
│  0 │ Name          │
│  1 │ Level         │
│  2 │ School        │
│  3 │ Casting Time  │
│  4 │ Duration      │
│  5 │ Range         │
│  6 │ Area          │
│  7 │ Attack        │
│  8 │ Save          │
│  9 │ Damage/Effect │
│ 10 │ Ritual        │
│ 11 │ Concentration │
│ 12 │ Verbal        │
│ 13 │ Somatic       │
│ 14 │ Material      │
│ 15 │ Material*     │
│ 16 │ Source        │
│ 17 │ Details       │
│ 18 │ link          │
│ 19 │               │
╰────┴───────────────╯
```

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers | 
where Name == 'Wish' | select Details |  get 0 | values
╭───┬─────────────────────────────────────────────────────────────────────────────╮
│ 0 │ Wish is the mightiest spell a mortal creature can cast. By simply speaking  │
│   │ aloud, you can alter the very foundations of reality in accord with your    │
│   │ desires.                                                                    │
│   │                                                                             │
│   │  The basic use of this spell is to duplicate any other spell of             │
│   │ 8th level or lower. You don't need to meet any requirements in that spell,  │
│   │ including costly components. The spell simply takes effect.                 │
│   │                                                                             │
│   │  Alternatively,                                                             │
│   │  you can create one of the following effects of your choice:                │
│   │                                                                             │
│   │   You create                                                                │
│   │ one object of up to 25,000 gp in value that isn't a magic item. The object  │
│   │ can be no more than 300 feet in any dimension, and it appears in an         │
│   │ unoccupied space you can see on the ground.                                 │
│   │   You allow up to twenty                                                    │
│   │ creatures that you can see to regain all hit points, and you end all        │
│   │ effects on them described in the greater restoration spell.                 │
│   │   You grant up                                                              │
│   │ to ten creatures that you can see resistance to a damage type you choose.   │
│   │                                                                             │
│   │ You grant up to ten creatures you can see immunity to a single spell or     │
│   │ other magical effect for 8 hours. For instance, you could make yourself and │
│   │  all your companions immune to a lich's life drain attack.                  │
│   │   You undo a                                                                │
│   │ single recent event by forcing a reroll of any roll made within the last    │
│   │ round (including your last turn). Reality reshapes itself to accommodate    │
│   │ the new result. For example, a wish spell could undo an opponent's          │
│   │ successful save, a foe's critical hit, or a friend's failed save. You can   │
│   │ force the reroll to be made with advantage or disadvantage, and you can     │
│   │ choose whether to use the reroll or the original roll.                      │
│   │                                                                             │
│   │                                                                             │
│   │  You might be able                                                          │
│   │  to achieve something beyond the scope of the above examples. State your    │
│   │ wish to the GM as precisely as possible. The GM has great latitude in       │
│   │ ruling what occurs in such an instance; the greater the wish, the greater   │
│   │ the likelihood that something goes wrong. This spell might simply fail, the │
│   │  effect you desire might only be partly achieved, or you might suffer some  │
│   │ unforeseen consequence as a result of how you worded the wish. For example, │
│   │  wishing that a villain were dead might propel you forward in time to a     │
│   │ period when that villain is no longer alive, effectively removing you from  │
│   │ the game. Similarly, wishing for a legendary magic item or artifact might   │
│   │ instantly transport you to the presence of the item's current owner.        │
│   │                                                                             │
│   │  The                                                                        │
│   │ stress of casting this spell to produce any effect other than duplicating   │
│   │ another spell weakens you. After enduring that stress, each time you cast a │
│   │  spell until you finish a long rest, you take 1d10 necrotic damage per      │
│   │ level of that spell. This damage can't be reduced or prevented in any way.  │
│   │ In addition, your Strength drops to 3, if it isn't 3 or lower already, for  │
│   │ 2d4 days. For each of those days that you spend resting and doing nothing   │
│   │ more than light activity, your remaining recovery time decreases by 2 days. │
│   │  Finally, there is a 33 percent chance that you are unable to cast wish     │
│   │ ever again if you suffer this stress.                                       │
╰───┴─────────────────────────────────────────────────────────────────────────────╯
```

`split`: Split a string into multiple rows using a separator.

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers |
where Name == 'Wish' | select Details |  get 0 | values | split row "\n"
╭────┬────────────────────────────────────────────────────────────────────────────╮
│  0 │ Wish is the mightiest spell a mortal creature can cast. By simply speaking │
│    │  aloud, you can alter the very foundations of reality in accord with your  │
│    │ desires.                                                                   │
│  1 │                                                                            │
│  2 │  The basic use of this spell is to duplicate any other spell of 8th level  │
│    │ or lower. You don't need to meet any requirements in that spell, including │
│    │  costly components. The spell simply takes effect.                         │
│  3 │                                                                            │
│  4 │  Alternatively, you can create one of the following effects of your        │
│    │ choice:                                                                    │
│  5 │                                                                            │
│  6 │   You create one object of up to 25,000 gp in value that isn't a magic     │
│    │ item. The object can be no more than 300 feet in any dimension, and it     │
│    │ appears in an unoccupied space you can see on the ground.                  │
│  7 │   You allow up to twenty creatures that you can see to regain all hit      │
│    │ points, and you end all effects on them described in the greater           │
│    │ restoration spell.                                                         │
│  8 │   You grant up to ten creatures that you can see resistance to a damage    │
│    │ type you choose.                                                           │
│  9 │   You grant up to ten creatures you can see immunity to a single spell or  │
│    │ other magical effect for 8 hours. For instance, you could make yourself    │
│    │ and all your companions immune to a lich's life drain attack.              │
│ 10 │   You undo a single recent event by forcing a reroll of any roll made      │
│    │ within the last round (including your last turn). Reality reshapes itself  │
│    │ to accommodate the new result. For example, a wish spell could undo an     │
│    │ opponent's successful save, a foe's critical hit, or a friend's failed     │
│    │ save. You can force the reroll to be made with advantage or disadvantage,  │
│    │ and you can choose whether to use the reroll or the original roll.         │
│ 11 │                                                                            │
│ 12 │                                                                            │
│ 13 │  You might be able to achieve something beyond the scope of the above      │
│    │ examples. State your wish to the GM as precisely as possible. The GM has   │
│    │ great latitude in ruling what occurs in such an instance; the greater the  │
│    │ wish, the greater the likelihood that something goes wrong. This spell     │
│    │ might simply fail, the effect you desire might only be partly achieved, or │
│    │  you might suffer some unforeseen consequence as a result of how you       │
│    │ worded the wish. For example, wishing that a villain were dead might       │
│    │ propel you forward in time to a period when that villain is no longer      │
│    │ alive, effectively removing you from the game. Similarly, wishing for a    │
│    │ legendary magic item or artifact might instantly transport you to the      │
│    │ presence of the item's current owner.                                      │
│ 14 │                                                                            │
│ 15 │  The stress of casting this spell to produce any effect other than         │
│    │ duplicating another spell weakens you. After enduring that stress, each    │
│    │ time you cast a spell until you finish a long rest, you take 1d10 necrotic │
│    │  damage per level of that spell. This damage can't be reduced or prevented │
│    │  in any way. In addition, your Strength drops to 3, if it isn't 3 or lower │
│    │  already, for 2d4 days. For each of those days that you spend resting and  │
│    │ doing nothing more than light activity, your remaining recovery time       │
│    │ decreases by 2 days. Finally, there is a 33 percent chance that you are    │
│    │ unable to cast wish ever again if you suffer this stress.                  │
╰────┴────────────────────────────────────────────────────────────────────────────╯
```

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers |
where Name == 'Wish' | select Details |  get 0 | values |
split row "\n" | get 0
Wish is the mightiest spell a mortal creature can cast. By simply speaking aloud, you can alter the very foundations of reality in accord with your desires.
```

`every`: Show (or skip) every n-th row, starting from the first one:

```
♥  : open --raw dnd.xlsx | from xlsx | get Spells | headers |
where Name == 'Wish' | select Details |  get 0 | values |
split row "\n" | first 5 | every 2
╭───┬─────────────────────────────────────────────────────────────────────────────╮
│ 0 │ Wish is the mightiest spell a mortal creature can cast. By simply speaking  │
│   │ aloud, you can alter the very foundations of reality in accord with your    │
│   │ desires.                                                                    │
│ 1 │  The basic use of this spell is to duplicate any other spell of 8th level   │
│   │ or lower. You don't need to meet any requirements in that spell, including  │
│   │ costly components. The spell simply takes effect.                           │
│ 2 │  Alternatively, you can create one of the following effects of your choice: │
╰───┴─────────────────────────────────────────────────────────────────────────────╯
```

---

Data source:
https://www.reddit.com/r/DnD5e/comments/k3zv2l/dnd_5e_spells_and_summons_block_google_sheets_and/

