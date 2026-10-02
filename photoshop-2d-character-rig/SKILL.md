---
name: photoshop-2d-character-rig
description: Generate readable, consistent 2D cutout characters from text or reference images and prepare them as Photoshop/After Effects/Duik-ready layered assets. Use for Indian TV/YouTube cartoon characters, moral-story/horror animation characters, 3/4-view puppets, reusable body-part kits, expression sets, and rig-friendly joint decomposition.
metadata:
  short-description: Generate Photoshop-ready 2D cutout characters with overlap-safe joints
---

# Photoshop 2D Character Rig

Use this skill when the user wants a reusable 2D character that is easy to finish in Photoshop and rig in After Effects, Duik, Character Animator, Moho, or another cutout-animation tool.

The target is NOT merely a pretty character sheet. The target is a production-oriented cutout puppet whose parts:
- preserve one character identity,
- remain readable when separated,
- overlap correctly at joints,
- can be assembled into a clean Photoshop layer stack,
- and survive basic rotation tests without white gaps.

## Core Principle

Treat character generation as a staged asset-production pipeline:

`reference -> identity lock -> canonical 3/4 puppet -> part generation -> hidden-joint reconstruction -> Photoshop assembly -> rig QA -> targeted repair`

Never ask an image model to solve the entire task in one crowded mega-sheet unless the user explicitly wants a presentation sheet only.

## Default Target

Unless the user asks otherwise:
- View: full-body three-quarter view facing right.
- Pose: neutral rig pose.
- Arms: slightly away from torso with visible background gap.
- Legs: slightly apart with visible background gap.
- Background: pure white for generation, transparent after extraction.
- Character proportions: realistic adult proportions appropriate to the supplied style.
- Output intent: Photoshop cutout puppet for After Effects + Duik.
- Resolution: generate as large and clean as the image model allows; do not intentionally downscale before separation.

## Non-Negotiables

1. **Lock identity before generating parts.**
   The face, skin tone, hair, outfit, colors, accessories, proportions, and art style must be recorded in a character bible before asset generation.

2. **Use one canonical pose as the source of truth.**
   Every part must be generated to match the canonical three-quarter character, not reimagined independently.

3. **Do not generate all rig parts in one giant sheet.**
   Split the work into focused generation passes: head/face, torso/clothing, left arm, right arm, left leg, right leg, hands, hair/secondary cloth.

4. **Every rotating joint requires hidden overlap.**
   Shoulder, elbow, wrist, hip, knee, and ankle segments must contain extra artwork beyond the visible seam.

5. **Separate foreground assets from occluding assets.**
   Hair, braid, dupatta, sleeves, hands, and body parts should be split according to how they need to move, not merely how they appear in a static drawing.

6. **Preserve outlines across joints.**
   Avoid exposed double outlines inside a bent joint. The visible contour should remain clean after rotation.

7. **Validate by rotation before calling the puppet rig-ready.**
   A sheet is not rig-ready merely because body parts are separated.

8. **Repair only failing parts.**
   If the left forearm fails, regenerate or reconstruct the left forearm. Do not regenerate the whole character unless identity has drifted globally.

## Workflow

### Stage 1 — Intake and Visual Analysis

Inspect the supplied reference image(s) and determine:
- apparent age and body proportions,
- face shape and landmark placement,
- hair construction,
- skin tone,
- outfit construction,
- accessories,
- near/far limb order,
- line weight,
- fill colors,
- shadow rule,
- camera/view angle,
- which cloth/hair pieces should move independently.

If the user supplies multiple references, classify each as one of:
- identity reference,
- style reference,
- clothing reference,
- pose reference,
- rig-layout reference.

Never copy identity from a style-only reference.

Create a compact character bible using `references/character-bible-template.md`.

### Stage 2 — Identity Lock

Generate or select one canonical full-body three-quarter character.

The canonical image must:
- show the whole body,
- keep both arms separated from the torso,
- keep both legs separated,
- avoid crossed limbs,
- keep hair/dupatta behind limbs unless deliberately separate,
- keep face neutral unless the user requests otherwise,
- preserve clear silhouette boundaries.

Reject the canonical image and retry if:
- hands merge into clothing,
- feet overlap,
- braid or scarf crosses over an arm that must rotate,
- sleeves fuse visually into torso,
- face identity is inconsistent,
- clothing pattern is too complex to separate.

Once accepted, treat it as the identity anchor for every subsequent generation.

### Stage 3 — Generate Focused Asset Passes

Generate assets in separate passes. Preferred order:

1. `head_base`
2. `hair_front`, `hair_back`, `braid`
3. facial assets: brows, eyes open/closed, mouth shapes
4. `torso` and torso clothing
5. `far_upper_arm`, `far_forearm`, `far_hand`
6. `near_upper_arm`, `near_forearm`, `near_hand`
7. `far_thigh`, `far_shin`, `far_foot`
8. `near_thigh`, `near_shin`, `near_foot`
9. dupatta/cape/scarf pieces
10. accessory pieces

For each pass, use the canonical character as a visual reference and explicitly state that:
- character identity must not change,
- view angle must not change,
- local part scale must match the canonical character,
- shading direction must not change,
- outline thickness must not change.

Read `references/generation-prompts.md` for reusable prompt blocks.

### Stage 4 — Joint-Safe Reconstruction

Read `references/rig-spec.md`.

Every rigid limb segment must include hidden geometry:

- shoulder: upper arm extends under torso/sleeve,
- elbow: upper arm and forearm overlap around the elbow pivot,
- wrist: forearm extends beneath the hand,
- hip: thigh extends under torso/salwar/skirt,
- knee: thigh and shin overlap,
- ankle: shin extends beneath foot/sandal.

Do not leave a literal visible gap between parts in the final assembled puppet. White space belongs between limbs and torso in the neutral pose, not inside a joint.

Prefer rounded or elliptical concealed end-caps rather than flat-cut rectangles.

### Stage 5 — Photoshop Assembly

Read `references/photoshop-assembly.md`.

Create or instruct the user to create one layer/group per independent moving part.

The layer stack must reflect depth in the canonical view. Keep anatomical naming and depth naming together when helpful, for example:
- `L_ARM_NEAR/upper_arm`
- `R_ARM_FAR/upper_arm`

For every movable part:
- keep a transparent canvas large enough to preserve hidden overlap,
- place the transform origin at the intended pivot,
- avoid trimming the layer so tightly that the hidden cap is removed.

If automatic PSD creation is unavailable, produce a precise layer manifest and assembly guide rather than pretending a PNG sheet is a layered PSD.

### Stage 6 — Rig QA

Read `references/qa-repair-loop.md`.

At minimum test:
- shoulders: -45°, -25°, 0°, +25°, +45°
- elbows: 0°, 30°, 60°, 90°
- wrists: -30°, 0°, +30°
- hips: -30°, 0°, +30°
- knees: 0°, 30°, 60°, 90°
- ankles: -20°, 0°, +20°

Check for:
- exposed background holes,
- double outlines,
- detached hands/feet,
- color mismatch,
- wrong part scale,
- incorrect pivot placement,
- sliding joint illusion,
- duplicated cloth folds at seams,
- occlusion-order errors.

### Stage 7 — Targeted Repair Loop

Classify each failure:
- `identity_drift`
- `insufficient_overlap`
- `pivot_misalignment`
- `outline_break`
- `scale_mismatch`
- `color_drift`
- `occlusion_error`
- `bad_silhouette`

Then repair only the smallest affected asset.

Maximum default repair attempts per part: 3.
If a part still fails after 3 attempts, report the issue and recommend manual Photoshop reconstruction for that part.

## Canonical Output Structure

```
character-name/
├── 00_reference/
│   ├── original.*
│   └── style_reference.*
├── 01_identity/
│   ├── character_bible.md
│   ├── character_bible.json
│   └── canonical_3q.png
├── 02_parts/
│   ├── head/
│   ├── torso/
│   ├── arms/
│   ├── legs/
│   ├── hands/
│   ├── hair/
│   ├── cloth/
│   └── accessories/
├── 03_psd/
│   ├── layer_manifest.md
│   └── character.psd
├── 04_qa/
│   ├── shoulder_test.png
│   ├── elbow_test.png
│   ├── knee_test.png
│   └── qa_report.md
└── 05_exports/
    ├── ae/
    └── png/
```

## Minimum Photoshop Layer Manifest

```
CHARACTER
├── HAIR_BACK
├── DUPATTA_BACK / CAPE_BACK
├── FAR_ARM
│   ├── upper_arm
│   ├── forearm
│   └── hand
├── FAR_LEG
│   ├── thigh
│   ├── shin
│   └── foot
├── TORSO
├── NEAR_LEG
│   ├── thigh
│   ├── shin
│   └── foot
├── NEAR_ARM
│   ├── upper_arm
│   ├── forearm
│   └── hand
├── HEAD_BASE
├── FACE
│   ├── eyes
│   ├── brows
│   └── mouth
├── HAIR_FRONT
└── FRONT_ACCESSORIES
```

The exact near/far ordering may vary with pose and costume. Preserve the canonical image's actual occlusion.

## Expression Set

Do not generate a large expression library until the base puppet passes rig QA.

Default useful set:
- neutral
- blink
- worried
- angry
- smile
- open speaking mouth
- closed speaking mouth
- O mouth
- wide vowel mouth

For lip-sync-heavy characters, generate mouth shapes only after face identity is locked.

## Success Criteria

A character is "Photoshop-ready" when:
- all required independent parts exist,
- identity is visually consistent,
- part scale is consistent,
- hidden overlap exists at every rotating joint,
- canonical composite matches the approved character,
- layer order is documented,
- pivots are documented,
- no required part is fused with another movable part.

A character is "rig-ready" only when:
- joint rotation tests pass without visible background gaps,
- pivot locations behave plausibly,
- no major double-outline artifacts appear through the tested range,
- and the assembled puppet can reproduce the canonical neutral pose.

## Transparency

Never claim:
- a flat contact sheet is already a layered PSD,
- a visually separated sheet is automatically rig-ready,
- AI-generated joints are correct without testing.

Say exactly what has been generated, what remains to be separated or reconstructed, and what has passed QA.
