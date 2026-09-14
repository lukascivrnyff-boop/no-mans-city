# NO MAN'S CITY — Living City Systems

This document is the implementation contract for the city simulation rewrite.

## 1. Core rule
Every moving entity must use the same physical world. No teleport shortcuts, no passing through buildings, no special police-only movement magic.

## 2. Road and lane graph
- Replace coarse road-center graph with lane-level paths.
- Curves and intersections get dense waypoints.
- Each lane has direction, speed target and allowed connections.
- Civilian traffic, police, service vehicles and player navigation all reference the same road data.
- Spawns must be on valid lane segments and outside visible camera range when possible.

## 3. Civilian traffic
- Cars accelerate and brake progressively.
- Keep safe distance from the vehicle ahead.
- Stop or slow for blocked lanes.
- Do not drive through buildings, trees, parked cars or police cars.
- At intersections choose only legal outgoing lane connections.
- Despawn only far away from the player and respawn on another valid lane.
- Vehicle variety comes from interchangeable GLB models with one normalized size/orientation contract.

## 4. Pedestrians
- Walk on pedestrian paths / sidewalk zones where available.
- Do not walk through buildings or parked vehicles.
- Idle, walk, cross and react states.
- React to nearby vehicle danger.
- Can be struck by vehicles and enter an incapacitated/dead state.
- Witnessed serious incidents can raise police response.

## 5. Player vehicle
- Progressive throttle and braking, not instant speed.
- Forward/reverse steering behaves correctly.
- Collision with world, trees, civilian cars, police cars and pedestrians.
- Impacts reduce speed and can damage/disable NPCs.

## 6. Police response state machine
1. Dispatch created.
2. Patrol cars spawn on reachable lane routes.
3. Cars physically drive to the incident with emergency lights.
4. Cars park at valid approach/parking points.
5. Officers exit only after the car stops.
6. Officers approach the player on foot.
7. Dialogue starts only at conversation range.
8. Compliant resolution -> officers return to cars -> local patrol resumes.
9. Player runs on foot -> officers pursue on foot.
10. Player escapes in a car -> officers return to vehicles and begin vehicle pursuit.
11. Serious injury/death of officer/civilian -> backup wave dispatched.

Police may damage the player only when the officer is actually facing/aiming at the player and line of sight is clear.

## 7. Security at stadium
- Guards stand inside the actual entrance, not at player spawn and not at the mirrored side.
- They react only while the player is on restricted pitch/grass.
- First warning only after a guard physically approaches conversation range.
- If player leaves restricted area, guards stop pursuit and return to posts.
- Escalation: warning -> final warning -> call police.

## 8. Vehicle pursuit
- Police pursuit uses the same lane graph as civilian traffic.
- Dynamic re-routing to the player's nearest reachable lane.
- Gradual acceleration/braking.
- Police keep spacing from one another and do not stack.
- If target leaves road temporarily, police route to the closest reachable interception point rather than cutting through scenery.

## 9. Collision layers
- World solids: buildings, walls, obstacles, large street furniture.
- Vehicle solids: world solids + trees/vegetation trunks + vehicles.
- Character solids: world solids + vehicles.
- Dynamic collisions are resolved before position commit.

## 10. Debug / QA mode
A deterministic test scenario must exist:
1. Spawn at stadium road.
2. Enter restricted pitch.
3. Security approaches and warns.
4. Ignore warnings.
5. Police dispatch.
6. At least two patrol cars visibly arrive by road.
7. Officers approach and dialogue appears with free mouse cursor.
8. Choose flee.
9. Enter player car.
10. Officers return to patrol cars.
11. Police vehicle pursuit begins.

Debug HUD should show entity state, route index, speed and police state so broken behavior is diagnosable instead of guessed.

## 11. Performance
- Target desktop browser / MacBook Air class hardware.
- Reuse geometries/materials where possible.
- NPC counts scale with distance.
- Avoid expensive per-frame raycasts against thousands of meshes; use spatially reduced collision sets where possible.

## 12. Asset contract for new NPC car models
Preferred input: `.glb`.
- One vehicle per file.
- Reasonable polygon count and texture size.
- Wheels do not need to be rigged for the first pass.
- Model should be upright and centered.
- Front direction can be arbitrary; importer will normalize orientation.
- Prefer roughly 2–15 MB per vehicle if possible.
- Civilian vehicles should have no baked police lights/logos unless intended as service vehicles.
