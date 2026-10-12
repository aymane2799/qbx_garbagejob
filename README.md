# Garbage Job V2 For QBOX

# Rework Update
- Job now pays per bag delivered on a configured scale.
- Job Randomises the amount of stops and what stops the user goes to
- Job traks rewards on the server rather than client
- Optional Cryptostick find per stop
- Highly Configurable.

## Atlas fork

This is a fork of [Qbox-project/qbx_garbagejob](https://github.com/Qbox-project/qbx_garbagejob) at commit
`0702c5d`, kept for the Atlas server. One change: **the length of a route can be capped.**

Upstream, a route has between `minStops` and as many stops as there are locations (19), and every stop is drawn
independently, so the same location can come twice in a row. In this fork:

| | Upstream | Fork |
|---|---|---|
| Stops per route | `minStops` to the number of locations | `minStops` to `maxStops` (`config/server.lua`). The default of 19 is upstream's range |
| Locations of a route | Drawn independently, repeats possible | Drawn without repetition, as long as the route is not longer than the list |

Nothing else changes: the bags per stop, the pay per bag, the time gate and every check are upstream's.

Rebase on a new upstream commit: `git fetch upstream && git rebase upstream/main`. The patch touches the route
draw in the `garbagejob:server:newShift` callback of `server/main.lua` and one key in `config/server.lua`.
