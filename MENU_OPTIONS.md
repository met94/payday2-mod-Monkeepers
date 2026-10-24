# Monkeepers - Menu Options Guide

**Where:** Mod Options (SuperBLT) -> **Monkeepers**.

Settings are saved to `...\PAYDAY 2\mods\saves\monkeepers.txt`. Only per-level
behaviours that differ from the default are stored.

## Per-level bag behaviour

The menu lists every heist. Each heist has a behaviour selector that applies
only to that level:

| Mode | Behaviour |
| --- | --- |
| **None** | Bots ignore loot bags on this heist. |
| **Same bag contents** | Routes only carry bags of the same type the player picked up (useful on maps with mixed loot, e.g. Lab Rats). |
| **Same bag weight** | Routes only carry bags with the same throw distance; gold does not use routes made for money. This is the default. |
| **All** | Routes carry any bag type. |

Pick the mode with left/right. The selection is saved when you leave the menu.
The level list is sorted by mode and then name, so levels you changed stand out
from the default.

## Controls

### Delete all routes

Keybind, host only. Deletes every transport rule and cancels outstanding carry
objectives immediately. Useful when a route points somewhere wrong or the heist
changes phase.

### Prevent bots to catch thrown bags

Disables bots catching bags players throw at them. When off, thrown bags are
caught as before; when on, bots ignore thrown bags and only use established
routes.

## How routes appear

1. Pick up a bag. The mod remembers that position.
2. Drop or throw the bag at least 7 m away from it. A route is created between
   where you took the bag and where it landed.
3. Any bag within 6 m of the source point is routed to the destination.
4. The route disappears after 90 seconds without a bag near the source.

You do not need to configure routes manually; Keepers' put-back-in-place order
and custom waypoints feed the same system.

In stealth, wake bots first with Keepers (secondary "Stop AI" control) so they
can start carrying.
