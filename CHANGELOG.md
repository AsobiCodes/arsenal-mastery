# Changelog

## 0.3.3
First version published from its own repository (earlier versions came inside RAC-RS). The launcher can update it from here.
- Faster: what never changes while a moby (an object in the game) lives, its class, is read once and cached, so the hooks that run many times
  a tick no longer read the whole record each time.
- Faster: perks only run their hooks when a perk that uses the hook is on (below V3 the usual case is a straight call to the game), and the
  list of perks that are on is rebuilt only when a level, a setting or the save changes.
- Log lines are queued while the mod runs (one or two a tick), so a burst of messages (hits, level-ups, perks) no longer costs a whole tick of
  the mod's time budget.
- Ammo: Survival: kills made by the Glove of Doom no longer give a shot back (its agents kill by themselves and refilled the glove).
- A mini bomb that belongs to a cluster is never split again, and the owner of a mini bomb is looked up once per bomb.
- A hit that raises an error no longer switches XP and weapon scaling off for good.

## 0.3.2 and earlier
Shipped inside RAC-RS; see its changelog.
