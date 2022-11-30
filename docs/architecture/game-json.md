# Game JSON

The state of the game may be represented in a single JSON document. 

Other key entities are game _turns_ and _actions_. The actions represented the smallest transactional unit
with each action transforming the game JSON file.

## New game template

Example JSON:

```json
{
    "turntrack": {
        "players": "PLAYER_COUNT_UNKNOWN",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "players": {},
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    }
}    
```

## Initial state of the game

A new game with no players. (It isn't clear whether this would be a real game state or not. We may require any game to have at least one player.)

The game hash uniquely identifies the game.

Example JSON:

```json
{
    "creator": "username0123",
    "creation_time: "2022-11-30T00:24:08.462Z",
    "game_hash": "2e9181e8bb0f0ae82d182ead3100c16f9ce99ad3",
    "turntrack": {
        "players": "PLAYER_COUNT_UNKNOWN",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "players": {},
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    }
}    
```

Redis Transaction:
```
JSON.SET game-495ec99e5dd0ecb4b211e8357382fc087fe99777
```

## Players join the game

Here two players join the game. Note that we do not yet know which turn track to use or
how many sailors to give each player because we do not yet know how many players are in the game.

```json
{
    "turntrack": {
        "players": "PLAYER_COUNT_UNKNOWN",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "players": {},
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    },
    "players": {
        "P1": {
            "name": "player 1 name",
            "color": "Color.RED",
            "money": 5,
            "player-id": "1234500001"
        },
        "P2": {
            "name": "player 2 name",
            "color": "Color.BLUE",
            "money": 5,
            "player-id": "1234500002"
        }   
   }
}   
```

## The number of players is set

We now four players. Because we know the number of players, we can set the turn track and number 
of sailors for each player. We set a player_count because we could choose how many players in the
game even before all the players have joined.

```json
{
    "turntrack": {
        "players": "FOUR_PLAYERS",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "players": {},
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    },
    "player_count": 4,
    "players": {
        "P1": {
            "name": "player 1 name",
            "color": "Color.RED",
            "money": 5,
            "player-id": "1234500001",
            "sailors": 22
        },
        "P2": {
            "name": "player 2 name",
            "color": "Color.BLUE",
            "money": 5,
            "player-id": "1234500002"
        }
   }
}   
```

## Additional players join

Two additional players join. We now have enough players to begin the game.

```json
{
    "turntrack": {
        "players": "FOUR_PLAYERS",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "players": {},
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    },
    "player_count": 4,
    "players": {
        "P1": {
            "name": "player 1 name",
            "color": "Color.RED",
            "money": 5,
            "player-id": "1234500001",
            "sailors": 22
        },
        "P2": {
            "name": "player 2 name",
            "color": "Color.BLUE",
            "money": 5,
            "player-id": "1234500002"
        },
        "P3": {
            "name": "player 3 name",
            "color": "Color.ORANGE",
            "money": 5,
            "player-id": "1234500003"
        },
        "P4": {
            "name": "player 4 name",
            "color": "Color.BLACK",
            "money": 5,
            "player-id": "1234500004"
        }
   }
}   
```
## Players choose their initial galleys and ports

Here player 1 selects a port and galley. Three sailors move from the player's supply to his choosen port. 
Three sailors move from the player's supply into the choosen galley. 

```json
{
    "turntrack": {
        "players": "FOUR_PLAYERS",
        "space": 1
    },
    "cards": {
        "drawn": []
    },
    "active_galley": 0,
    "bank": {
        "Cargo.WOOD": 14,
        "Cargo.STONE": 12,
        "Cargo.MARBLE": 12,
        "Cargo.WINE": 12,
        "Cargo.GOLD": 10,
        "Cargo.SPICE": 10
    },
    "player_count": 4,
    "players": {
        "P1": {
            "name": "player 1 name",
            "color": "Color.RED",
            "money": 5,
            "player-id": "1234500001",
            "sailors": 16
        },
        "P2": {
            "name": "player 2 name",
            "color": "Color.BLUE",
            "money": 5,
            "player-id": "1234500002",
            "sailors": 22
        },
        "P3": {
            "name": "player 3 name",
            "color": "Color.ORANGE",
            "money": 5,
            "player-id": "1234500003",
            "sailors": 22            
        },
        "P4": {
            "name": "player 4 name",
            "color": "Color.BLACK",
            "money": 5,
            "player-id": "1234500004",
            "sailors": 22            
        }
    },
    "galleys": {
        "1": {
            "owner": "P1",
            "hold": { 
                "1": "SAILOR",
                "2": "SAILOR",
                "3": "SAILOR",
                "4": "EMPTY",
                "5": "EMPTY"
            }
          }
   },
    "ports": {
        "Venezia": {
            "owner": "P1",
            "sailors": 3,
            "cargo": []
        }
    }
}   
```

TODO we need to add location of galley on the board. That could be stored in either the galley section or the port section, if we add the single sea space to the list of ports.  We will work on transactions and then decide which makes most sense.

## An ordinary galley game turn

The order of play is determined by the galleys and galley ownership. 
Play begins with the first galley. Its owner plays the galley's turn 
and then play passes to the owner of the next galley on the galley wheel.

During each galley's turn, the owner of that galley chooses either to take
an investment round for the galley or completes a series of galley actions. Galley actions 
are grouped into three phases which must always be played in the following order:

* Phase 1 - Load
* Phase 2 - Move
* Phase 3 - Unload

### Phase 1 actions

The following _load_ actions are permitted during phase 1.

#### LOAD_CARGO 

Purchase one or more goods from the port at which the galley is moored and load them on the galley. 

Inputs:

Validation Rules:

Example JSON:

#### SWAP 

Swap goods and sailors between two galleys moored in a port owned by the galley owner.

Inputs:

Validation Rules:

Example JSON:

#### LOAD_SAILORS

Load sailors from the port into the galley if the port is owned by the galley owner.

#### UNLOAD_SAILORS

Unload sailors from the galley into the port if the port is owned by the galley owner.

#### WASTE_CARGO

Drop a good from the galley into the sea. 

### Phase 2 actions

The following _move_ actions are permitted during phase 2.

#### MOVE_GALLEY

Move the galley zero or more spaces with the maximum movement being the number of sailors present on the galley. 

### Phase 3 actions

The following _unload_ actions are permitted during phase 3.

#### DELIVER

Unload one or more goods from the galley into the destination port, provided such goods have not already been delivered to that port.

#### CLAIM_PORT

Claim an unowned port by unloading one or more sailors into the port.

#### ATTACK_PORT

Engage a port belonging to another player in battle.

#### ATTACK_GALLEY

Engage a galley belonging to another player in battle.

## Investment round 

As an alternative to an ordinary galley game turn, a player make take an invetment round. When taking an investment round, the galley stays at its current location. The player may then make any number of the following purchases.

| Item      | Cost                        | Note |
| --------- | --------------------------- | -------- |
| Sailor    | 1                           | Sailors may be placed in any port owned by the player or galley in a port owned by the player. |
| Galley    | 1 x number of galleys owned | Galleys may be placed in any port owned by the player. Only one galley may be purchased per investment round.|
| Basilica  | 2                           | Basilicas may be placed in any port owned by the player in which gold or marble are present. Only one Basilica may exist in any given port. |
| Fort      | 2                           | Forts may be placed in any port owned by the player in which stone or wood are present.  Only one fort may exist in any given port.|

### Validations that always apply

Validation Rules:
1. One can't spend more money than one has.

### Investment Round Actions

#### PURCHASE_GALLEY

Inputs for galley purchase:
1. Galley number.
2. Destination port number.

Validation Rules:
1. Only one galley may be purchased as part of this investment round.
2. One may only purchase unowned galleys.
3. A galley must be placed in one's own port.
4. A galley must have at least one sailor or it will sink.

#### PURCHASE_SAILORS

Inputs for a sailor purchase:
1. number of sailors.
2. destination galley or port

Validation Rules:
1. One can't buy more sailors than one has.
2. One can't place more sailors in a port than the spaces available for sailors.
3. Only galleys in a port one ones may accept new sailors.
4. Only ports one owns can accept new sailors.

#### PURCHASE_BASILICA

Inputs for a basilica purchase:
1. destination port

Validation Rules:
1. Basilicas must be placed in a port with gold and marble. 
2. Basilicas may not be placed in a port which already has a basilica.
3. Basilicas cannot be purchased if they cannot be placed.

#### PURCHASE_FORT

Inputs for a fort purchase:
1. destination port 

Validation Rules:
1. Forts must be placed in a port with stone and wood. 
2. Forts may not be placed in a port which already has a fort.
3. Forts cannot be purchased if they cannot be placed.

#### Other investment round actions

With an investment round some actions are allowed so that sailors may be placed in newly purchased galleys and distributed to ports.

## Drawing a card and advancing on the turn track

## The scoring round

## The end of the game




