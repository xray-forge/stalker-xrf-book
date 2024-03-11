# Patrols

- todo <br/>
- todo <br/>
- todo <br/>

## Related schemes

- [walker](./schemes/walker.md)
- [remark](./schemes/remark.md)
- [sleeper](./schemes/sleeper.md)
- [camper](./schemes/camper.md)
- [sniper](./schemes/sniper.md)
- [follower](./schemes/follower.md)
- [kamp](./schemes/kamp.md)
- [zoneguard](./schemes/zoneguard.md)
- [wounded](./schemes/wounded.md)
- [heli_hunter](./schemes/heli_hunter.md)
- [patrol](./schemes/patrol.md)

## ☎️ Patrol flags system

To control patrol movement / staying on some point flags system is implemented. <br/>
Following parameters can be used to affect patrolling logics.

### 🔨 path_walk

#### a=state

- body state when moving on a patrol

#### p=percent

- percent probability to stop on a patrol point (0-100), it is 100 by default

#### sig=name

- set signal on waypoint arrival (without look check and correct), for the following check with the logic system field on_signal
- -to set a signal after turning, use the corresponding flag for a path_look waypoint

### 🔨 path_look

#### a=state

- body state when stand/sit at place

#### t=msec

- time to stay idle when on point and look
- `*` means unlimited time
- valid values are in the range [1000, 30000], 5000 by default
- for terminal waypoints of path_walk that have no more than one corresponding path_look, the value of `t` is always
  considered infinite and does not have to be set

#### sig=name

- set signal on path_look waypoint with the given name after turning towards the point

#### syn

- flag to wait whole team before setting signal as complete, used to wait others and stay idle before gathering
- this flag will halt setting the signal until all characters with the given team arrive
- the team is set as a text string in customdata, the given character will be play its idle animation until the others arrive

#### sigtm=signal

- set signal on time callback from state manager
- if t=0, signal is set right after playing `init` animation

## 💊 Examples

### Button press

Play press animation and emit scheme signal:

- wp00 | a=press | t=0 | sigtm=pressed
- on_signal = pressed | another_scheme@section

### todo:

- Check docs from links and integrate more

## ⛽️ References

- http://sdk.stalker-game.com/en/index.php?title=Logic
- http://sdk.stalker-game.com/ru/index.php?title=%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%B8_%28%D1%87%D0%B0%D1%81%D1%82%D1%8C_1%29
