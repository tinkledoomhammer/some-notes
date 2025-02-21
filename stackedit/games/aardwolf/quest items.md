## Qeust Items

### Weapons  
`(Cost: 1000qp  Resell: 500qp  Weight: Varies  Worn: wield)
Variable:   Avg Damage: 2.5 * level if 1-79, 3 * level if 80 or higher.
            Hitroll/damroll: Level / 10 , minimum of 5.
`
   Each weapon uses a different weapon skill and has its own default
   damage, weight, and weapon flag:

 ```
        Weapon    Skill     Damage Type    Flag       Wght
        Axe       Axe       Cleave         Vorpal      20
        Bow       Bow       Thwack         Changing    20
        Dagger    Dagger    Pierce         Sharp       10
        Flail     Flail     Beating        Vampiric    25
        Halberd   Polearm   Drain          Changing    25
        Mace      Mace      Pound          Frost       20
        Staff     Spear     Thwack         Shocking    15
        Sword     Sword     Slice          Flaming     35
        Whip      Whip      Shocking Bite  Vampiric    15
```


#### Shield  
```
(Cost: 750qp  Resell: 500qp  Weight: 25  Worn: shield)
Static:     +5 Strength.
Variable:   Hitroll/Damroll: Level / 5, minimum 5.
            Resistances: All physical and all magical, variable by
            level.  (Use 'quest appraise' for values at your level.)
```
-------------------------------------------------------------------------   
### Aura of Sanctuary  
```
(Cost: 2500qp  Resell: 2000qp  Weight: 1  Worn: float)
Static:     +100 hp, +50 mana, +200 moves.
            +3 Dexterity (on top of level-based stats).
            Permanent, undispellable sanctuary effect when worn.
Variable:   Bonus to all six stats, 1 + (level / 50, rounded down)
```
### Amulet of Aardwolf  
```
(Cost: 750qp  Weight: 4  Worn: hold (portal with wish))
Static:     +2 to all six stats.
            Portal to the Aardwolf Hotel, with enhanced healing, questor,
            and area exits.            
```
### Bracers of Iron Grip  
```
(Cost: 1750qp  Weight: 5  Worn: arms- NOT wrist)
Static:      Cannot be disarmed while bracers are worn.
Variable:    Hitroll/Damroll: Level / 5, minimum 5.
             HP/mana: Equal to level of purchasing player, minimum 50.
```
### Gloves of Dexterity  
```
(Cost: 2250qp  Weight: 10  Worn: hands)
Static:      +6 Dexterity.
             100% Dual Wield skill, regardless of class.  All other
               restrictions apply; see 'help dual wield'.
Variable:    HP/mana/moves:  Level of purchasing player, minimum 50.
             Hitroll/Damroll: Level / 5, minimum 5.
```
### Boots of Speed  
```
(Cost: 1300qp  Weight: 2  Worn: feet)
Static:      Hitroll/Damroll: +10.
             HP: +75.
             +3 Dexterity, +2 Luck.
             Permanent haste affect when worn.
Variable:    Resistances: Varies by level.  Use 'quest appraise' to see
             resists at current level.
```
### Wings of Aardwolf  
```(Cost: 800qp  Weight: 1  Worn: back)
Static:      +2 Wisdom, +2 Constitution, +3 Dexterity.
             Permanent flying affect when worn.
```
### Ring of Invisibility  
``` (Cost: 500qp  Weight: 1  Worn: finger)
Static:      HP/mana/moves: +30.
             Adds invisibility affect when worn.  Remove and re-equip to
             reset effect (after going visible or starting a fight, etc.)
```
### Ring of Regeneration  
``` (Cost: 1000qp  Resell: 700qp  Weight: 1  Worn: finger)
Static:      Hitroll/Damroll: +12
             Enhanced healing when worn.
Variable:    Stats: 1 + (Level / 30, rounded down) points in random stats.
```
### Helm of True Sight  
```(Cost: 1100qp  Resell: 700qp  Weight: 5  Worn: head)
Static:      HP: +45.
             Hitroll/Damroll: +8.
             Permanent detect good/evil/hidden/invis/magic while worn.
Variable:    Luck: 1 + (level / 20, rounded down).
             Resistances: Varies by level.  Use 'quest appraise' to see
             resists at current level.
```
### Breastplate of Magic Resistance  
```
(Cost: 2000qp  Weight: 10  Worn: torso)
Static:      HP/mana/moves: +125.
             +2 to all six stats.
             Resists: +100 to all magic except disease/poison.
Variable:    Hitroll/Damroll: Level / 5, minimum 5.
             Resistances: Physical resistances vary by level.  Use 'quest
             appraise' to see resists at current level.
```
### Decanter of Endless Water  
`(Cost: 550qp  Weight: 5  Worn: none)`

  Once filled, the Decanter will almost never run dry (but can be poured
  out if you want to fill it with something different). Decanters are
  always level 1, regardless of level when purchasing it.

### Bag of Aardwolf  
```
(Cost: 1500qp  Resell: 1000qp  Weight: Varies  Worn: none)
Static:      Heaviest item: 100.
             Weight multiplier:  20%.
Variable:    Weight: -50 - (current carrier level * 2).
             Capacity: 1000 + (20 * current carrier level).
```
  The Bag of Aardwolf changes stats based upon the person carrying it.  It
  automatically changes to your level.  A bag with higher stats bought off
  rauction will be calculated to your level; it will not retain the level
  201 stats shown on 'rbid'.

  Once you have reached level 201 and remorted, your aard bags will
  stay at level 201 but Tiers won't get the full benefit of bag over level
  201 until they reach the appropriate level.


### ObjLower  
`(Cost: 500qp per use)`

  See 'help objlower'.

### ObjStat:  
`(Cost: 500qp per use)`

  See 'help objstat'.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNDg2NDgyNDBdfQ==
-->