# sr_teleport

Scheme is implementing teleportation when reaching specific restrictor. <br/>
Example: teleports in stalker ShoC last zone, teleports at Zaton burning village, escape bridge teleport is CS.

Supports up to 10 different teleport locations (`i = 0...10`).

## ini parameters

### ❄️ timeout

Timeout before teleporting.

- Type: `number`, `milliseconds`
- Default: `900`
- Example: `0`, `1000`, `3000`

### ❄️ point[i], look[i], prob[i]

List of possible teleportation destinations. <br/>
Just fields with `i` prefix, where `i` is number from 1 to 10.

#### point

Name of teleport patrol to teleport actor on timeout. <br/>
Requires patrol with at least one point.

- Type: `string`
- Default: `none`
- Example: `zat_b20_quest_teleport_walk`

#### look

Name of teleport patrol to set actor look after teleporting. <br/>
Requires patrol with at least one point.

- Type: `string`
- Default: `none`
- Example: `zat_b20_quest_teleport_look`

#### prob

Probability of point `i` usage when teleport timeout activates.

- Type: `number`
- Default: `100`
- Example: `25`, `50`, `100`

## Usage

todo; <br/>
todo; <br/>
todo; <br/>

## Examples

### zat_b20_teleport_logic.ltx

```ini
[logic]
active = sr_teleport

[sr_teleport]
point1 = zat_b20_quest_teleport_walk
look1 = zat_b20_quest_teleport_look
```


### example.ltx

```ini
[logic]
active = sr_teleport

[sr_teleport]
point1 = example_teleport1_walk
look1 = example_teleport1_look
prob1 = 25
point2 = example_teleport2_walk
look2 = example_teleport2_look
prob2 = 25
point3 = example_teleport3_walk
look3 = example_teleport3_look
prob3 = 50
```
