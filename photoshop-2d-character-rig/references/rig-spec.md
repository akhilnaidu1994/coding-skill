# Rig Geometry and Hidden-Overlap Specification

Use this reference whenever generating, extracting, reconstructing, or validating body parts.

## General Joint Rule

Every rotating joint is represented by:
1. a visible segment,
2. a concealed overlap zone,
3. a pivot positioned inside the overlap zone.

Never cut both neighboring segments exactly at the visible seam.

## Preferred End-Cap Shape

Use a rounded or elliptical cap around the pivot. Rounded concealed geometry tolerates rotation far better than a flat horizontal or diagonal cut.

For clothed joints, continue both:
- the base skin/body geometry when needed, and
- the visible clothing geometry that should cover the joint.

Avoid a hard outline across the concealed interior edge unless that line is meant to remain visible during animation.

## Suggested Hidden-Overlap Amounts

These are defaults, not absolute anatomy rules.

| Joint | Hidden overlap guideline | Notes |
|---|---:|---|
| Shoulder | 12–18% of upper-arm length | Upper arm should travel under torso/sleeve opening |
| Elbow | 10–15% of forearm length | Both neighboring segments should cover the pivot area |
| Wrist | 8–12% of hand/forearm local length | Forearm usually sits beneath hand |
| Hip | 12–18% of thigh length | Thigh extends beneath torso, skirt, kurta, salwar or shorts |
| Knee | 10–15% of shin length | Prefer rounded knee-zone coverage |
| Ankle | 8–12% of local shin length | Shin should extend into foot/sandal region |

When the costume is loose or baggy, increase overlap enough to preserve the clothing silhouette through the intended range of motion.

## Pivot Placement

Place the pivot at the anatomical rotation center, not at the visible outer edge.

Approximate landmarks:
- shoulder: center of humeral head beneath sleeve,
- elbow: center of elbow mass,
- wrist: center between radius/ulna termination and hand,
- hip: femoral head region beneath pelvis,
- knee: center of knee mass,
- ankle: center of ankle hinge above foot.

For stylized characters, visual plausibility is more important than anatomical precision, but the pivot must stay inside overlapping art.

## Outline Rules

At each joint:
- visible exterior contour may have an outline,
- hidden interior overlap should usually NOT have a strong seam outline,
- if both parts carry outlines through the overlap, rotation can reveal a doubled dark ring.

Where needed, split the outline from the fill or manually erase concealed interior outline sections in Photoshop.

## Clothing-Specific Notes

### Sleeves
If an upper arm wears a sleeve:
- sleeve artwork belongs to the upper-arm segment if it must rotate with the arm,
- the sleeve cap should extend beneath the torso/shoulder,
- do not bake the sleeve into the torso if the arm needs wide motion.

### Kurta / Tunic / Dress
For thigh movement beneath long clothing:
- keep the main tunic/skirt as a torso or cloth layer,
- extend thighs beneath it,
- do not visually cut legs at the visible hem.

### Salwar / Loose Pants
Loose pants may need:
- upper leg cloth segment,
- lower leg cloth segment,
- a deliberately hidden knee transition.
Do not rely on a tiny skin-colored connector if the visible garment is supposed to remain continuous.

### Dupatta / Scarf / Cape
Separate into the minimum number of pieces needed for animation:
- back cloth,
- front neck drape,
- optional left/right tails.
Do not allow a scarf tail to be baked onto an arm that needs independent motion.

### Hair / Braid
Separate:
- back hair mass,
- front fringe/face-framing pieces,
- braid/ponytail if it will swing.
Braid roots should overlap underneath the back-hair mass.

## Neutral Rig Pose

For asset generation:
- arms 15–30° away from torso,
- elbows only slightly bent,
- hands relaxed,
- legs separated enough to expose inner contours,
- feet fully visible,
- secondary cloth/hair positioned behind primary limbs unless the user needs a different rig structure.

The neutral pose exists to expose separable silhouettes, not to look like a natural acting pose.

## Joint QA Pass Conditions

A joint passes when:
- no transparent/white hole appears through the test angle range,
- no hard internal seam becomes visible,
- outline thickness remains visually consistent,
- the limb does not appear to detach or telescope,
- neighboring clothing still looks attached,
- pivot position does not create obvious sliding.
