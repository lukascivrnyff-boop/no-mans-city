# Collision + Lane Foundation

## v12 collision foundation

- Walkable surfaces are separated from hard obstacle meshes.
- The soccer stadium collider is treated as walkable ground rather than a vertical wall.
- Broad flat meshes are treated as ground to avoid invisible-wall behavior on steps, ramps and mild terrain changes.
- Player movement follows ground height with controlled step-up / drop-down limits.
- Vehicle collision rays are raised above curb height and vehicles follow ground height separately.
- Buildings, real obstacles and street furniture remain hard collision.
- Vegetation remains vehicle-solid.

## v13 lane traffic foundation

Generated from the existing road graph:
- 538 directed lanes
- 8,078 sampled lane points
- lane width offset: 0.008 world units
- maximum lane point spacing: 0.055 world units
- no dead-end lane transitions in the generated graph

Civilian traffic now follows directed lane points rather than travelling directly between coarse road-tile centers. Initial traffic behavior includes gradual acceleration/braking, basic following distance and obstacle checks.

Initial real NPC fleet used for traffic testing:
- VW Golf R32 Mk4
- BMW M3 E36
- Chevrolet Express / GMC Savana cargo van
- Ford Mustang RTR
- Dodge Charger Hellcat

Paint variants are applied on models whose body material can be identified reliably.

## Next implementation order

1. Harden lane graph intersections / remove invalid shortcuts.
2. Full vehicle-to-vehicle avoidance and collision response.
3. Move police response and pursuit onto the same lane graph.
4. Pedestrian sidewalk / crossing navigation.
5. Stadium entrance/security cleanup.
6. Debug test scenarios for repeatable QA.
