# Rig QA and Repair Loop

A separated character is not considered rig-ready until it passes motion checks.

## Test Philosophy

Test the actual failure mode: rotation around pivots. Do not judge only from a static neutral pose.

## Standard Rotation Matrix

- Shoulder: -45°, -25°, 0°, +25°, +45°
- Elbow: 0°, 30°, 60°, 90°
- Wrist: -30°, 0°, +30°
- Hip: -30°, 0°, +30°
- Knee: 0°, 30°, 60°, 90°
- Ankle: -20°, 0°, +20°

If the intended animation requires larger motion, extend the range before approval.

## Defect Types

### identity_drift
Symptoms: altered face, different skin tone, new clothing details, changed line weight or proportions.
Repair: regenerate the failing asset using the canonical image and identity-lock block.

### insufficient_overlap
Symptoms: white/transparent wedge at a joint, segment detaches when rotated, tiny connector becomes visible.
Repair: extend hidden geometry around the pivot, prefer a rounded cap, preserve the visible silhouette.

### pivot_misalignment
Symptoms: limb appears to orbit instead of bend, joint slides sideways, elbow/knee location changes unnaturally.
Repair: move pivot to anatomical/visual center and reposition child artwork relative to pivot.

### outline_break
Symptoms: doubled dark ring, black seam inside a bent elbow/knee, outline disappears at external contour.
Repair: remove concealed interior outline and keep only visible external contour.

### scale_mismatch
Symptoms: regenerated hand too large, forearm thickness differs, foot length inconsistent.
Repair: first correct scale/transform in Photoshop; regenerate only if perspective or anatomy is incompatible.

### color_drift
Symptoms: skin/clothing hue changes across parts or shadow tone differs.
Repair: sample approved colors from canonical image and recolor in Photoshop when feasible.

### occlusion_error
Symptoms: braid appears in front when it should be behind, far limb sits above torso, scarf cuts through hand.
Repair: correct layer order first; split cloth/hair only if independent motion requires it.

### bad_silhouette
Symptoms: neutral composite no longer matches canonical design, parts create bulges or dents.
Repair: realign or mask locally; regenerate only the smallest incompatible asset.

## QA Report Template

```
# Rig QA Report

Character:
Canonical view:
Date:

## Identity
- Face consistency: PASS/FAIL
- Color consistency: PASS/FAIL
- Proportion consistency: PASS/FAIL

## Joints
| Joint | Range tested | Gap | Double outline | Pivot | Result |
|---|---|---|---|---|---|
| near shoulder | -45..+45 | no | no | good | PASS |
| near elbow | 0..90 | yes | no | good | FAIL |

## Secondary Motion
- Hair/braid:
- Dupatta/scarf:
- Accessories:

## Repairs Required
1.
2.

## Final Status
- Photoshop-ready: YES/NO
- Rig-ready: YES/NO
```

## Repair Priority

Fix in this order:
1. missing/merged part
2. identity drift
3. scale mismatch
4. overlap
5. pivot
6. outline
7. color
8. cosmetic fold details

## Repair Limit

Default maximum: 3 attempts per failing part.

After 3 unsuccessful attempts, keep the best version, identify the exact defect, and recommend a manual Photoshop patch. Do not regenerate unrelated parts.

## Completion Rule

Use "rig-ready" only if the requested motion range has actually been checked.

If rotation testing was not performed, say "parts prepared for rigging; motion QA still required."
