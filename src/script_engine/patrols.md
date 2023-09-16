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

- percent probability to stop on a patrol point

#### sig=name

- set signal on waypoint reach (without look check and correct turn)

### 🔨 path_look

#### a=state

- body state when stand/sit at place

#### t=msec

- time to stay idle when on point

#### sig=name

- set signal on path_look waypoint tuned/looking

#### syn

- flag to wait whole team before setting signal as complete, used to wait others and stay idle before gathering

#### sigtm=signal

- set signal on time callback from state manager (animation finish as example)
- if t=0, signal is set right after initial animation

### 💊 Example

- wp00 | a=press | t=0 | sigtm=pressed
- on_signal = pressed | another_scheme@section

## ⛽️ References

- http://sdk.stalker-game.com/ru/index.php?title=%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0_%D0%BB%D0%BE%D0%B3%D0%B8%D0%BA%D0%B8_%28%D1%87%D0%B0%D1%81%D1%82%D1%8C_1%29
