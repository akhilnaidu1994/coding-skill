# Photoshop Assembly Guide

Use this guide after the character parts are approved.

## Goal

Build a layered PSD that can be imported into After Effects and rigged with Duik or another cutout-rigging system.

## Preparation

For each asset:
1. remove the white background cleanly,
2. preserve antialiased edge pixels without a white fringe,
3. keep hidden joint extensions,
4. avoid auto-trimming the layer to the visible silhouette,
5. keep each movable part on a separate layer.

If an asset sheet contains multiple parts, extract them to individual transparent layers before assembly.

## Recommended PSD Structure

```
CHARACTER
├── 00_BACK
│   ├── hair_back
│   ├── braid
│   └── dupatta_back
├── 10_FAR_LEG
│   ├── thigh
│   ├── shin
│   └── foot
├── 20_FAR_ARM
│   ├── upper_arm
│   ├── forearm
│   └── hand
├── 30_TORSO
│   ├── torso_base
│   └── torso_clothing
├── 40_NEAR_LEG
│   ├── thigh
│   ├── shin
│   └── foot
├── 50_NEAR_ARM
│   ├── upper_arm
│   ├── forearm
│   └── hand
├── 60_HEAD
│   ├── head_base
│   ├── ears
│   ├── face
│   │   ├── brows
│   │   ├── eyes
│   │   └── mouth
│   └── hair_front
└── 70_FRONT_CLOTH_ACCESSORIES
```

Map NEAR/FAR to anatomical L/R only after confirming the canonical view.

## Layer Naming

Use short predictable names. Avoid spaces when the downstream rig tool dislikes them.

Recommended:
- `torso`
- `arm_near_upper`
- `arm_near_fore`
- `hand_near`
- `arm_far_upper`
- `arm_far_fore`
- `hand_far`
- `leg_near_thigh`
- `leg_near_shin`
- `foot_near`
- `leg_far_thigh`
- `leg_far_shin`
- `foot_far`
- `head_base`
- `hair_front`
- `hair_back`
- `braid`
- `dupatta_back`

## Assembly Order

1. Place the canonical full-body image at low opacity as a locked alignment reference.
2. Add all separated parts above it.
3. Align each asset until the combined silhouette matches the canonical image.
4. Hide the canonical reference.
5. Verify there are no seams in the neutral pose.
6. Verify hidden overlap is actually present by temporarily moving each child segment away from the parent.
7. Keep the hidden overlap; do not erase it just because it is not visible in the neutral pose.

## Pivot Marking

Create a temporary guide layer named `PIVOTS_GUIDE`.

Mark:
- shoulder
- elbow
- wrist
- hip
- knee
- ankle
- neck/head base

Do not flatten the guide into the artwork.

If exporting to After Effects, place the layer anchor point at the pivot after import. If using Duik, create bones/controllers after the visual layer alignment is final.

## Transform-Origin Rule

The pivot must sit inside valid painted pixels belonging to overlapping geometry whenever possible.

Bad:
- pivot placed at the last visible pixel of the arm,
- pivot outside the layer bounds,
- joint constructed from two flat-ended parts that merely touch.

Good:
- pivot lies inside a rounded overlap region,
- parent and child artwork cover the pivot from multiple rotation angles.

## Masks and Clipping

Use masks only when they simplify depth behavior.

Avoid a mask that permanently deletes hidden geometry required for rotation.

If a sleeve opening needs to hide an upper-arm cap:
- keep the upper-arm cap intact,
- use torso/sleeve artwork above it to provide natural occlusion.

## Facial Replacement Assets

Keep face assets registered to the same head coordinate system.

Useful groups:
```
eyes/
  open
  half
  closed

mouth/
  neutral
  smile
  worried
  A
  E
  O
  M_B_P
```

Do not resize individual mouth shapes independently after registration.

## Import to After Effects

When importing the PSD:
- import as Composition,
- retain layer sizes,
- verify each Photoshop group/layer maps as expected,
- then set anchor points before parenting or Duik rig creation.

Recommended hierarchy:
```
torso
├── head
├── upper_arm_near
│   └── forearm_near
│       └── hand_near
├── upper_arm_far
│   └── forearm_far
│       └── hand_far
├── thigh_near
│   └── shin_near
│       └── foot_near
└── thigh_far
    └── shin_far
        └── foot_far
```

Secondary cloth/hair should be parented according to its physical attachment point, not simply grouped by visual location.

## Final Photoshop Checklist

Before saving the master PSD:
- canonical reference hidden but retained,
- no accidental white background pixels,
- no clipped hidden joint extensions,
- no merged movable parts,
- all layers named,
- layer depth order correct,
- pivots documented,
- neutral pose reconstructs cleanly,
- PSD saved before AE-specific edits.
