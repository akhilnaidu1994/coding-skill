# Generation Prompt Library

These are reusable prompt blocks. Adapt the style and character details from the character bible.

## Global Identity Lock Block

Use this block in every generation after the canonical image is approved:

> Use the attached canonical character as the identity source of truth. Preserve exactly the same apparent age, face shape, eye shape, skin tone, hairstyle, hairline, body proportions, outfit construction, colors, accessories, line weight, shading rule, and three-quarter camera angle. Do not redesign, beautify, simplify, age-shift, change ethnicity, change clothing, or change proportions. This is asset generation for the SAME character, not a new interpretation.

## Global Art-Cleanup Block

> Produce clean readable 2D cutout-animation artwork with crisp closed silhouettes, consistent outline thickness, flat controlled fills, and only the shading style present in the reference. No gradients unless the reference uses gradients. No texture unless the reference uses texture. No background objects, text, labels, watermark, cast shadow, or scenery.

## Canonical 3/4 Puppet Prompt

> Draw one full-body three-quarter-view character facing right in a neutral cutout-rig pose. Keep the entire body visible. Arms must hang naturally but remain approximately 20–25 degrees away from the torso so there is a clear background gap between arm and body. Keep the legs slightly apart with a visible gap. Do not cross limbs. Keep hands open and relaxed. Keep secondary hair, braid, dupatta, scarf, cape, or long cloth behind the arms unless the character design requires another layer order. Preserve the approved character identity exactly.

## Head Asset Prompt

> Generate only the head assets for the approved canonical character in the exact same three-quarter view and scale. Include: clean head base without front hair, front hair, back hair, optional braid/ponytail, left/right visible ears as appropriate. Preserve the facial proportions and neck angle. Assets must be isolated and non-overlapping on plain white. Do not generate alternate characters or extra poses.

## Face Asset Prompt

> Generate facial replacement assets matching the approved canonical head exactly: eyebrows, eyes open, eyes half-closed, eyes closed, neutral mouth, small smile, worried mouth, open talking mouth, O mouth, wide vowel mouth. Keep identical placement geometry, line weight, iris color, lip color, and three-quarter perspective. No full alternate heads unless requested.

## Torso Prompt

> Generate the torso/clothing asset for the approved character in the canonical three-quarter view. Remove movable arms, head, and legs while preserving concealed shoulder sockets, neck base, and hip coverage needed for assembly. Keep costume seams and shading aligned with the canonical character. Add hidden shoulder/hip artwork under areas that will be occluded in the final puppet.

## Upper Arm Prompt

> Generate only the [near/far] upper-arm asset for the approved character. Match the canonical three-quarter perspective, skin/clothing color, sleeve shape, outline thickness, and shading. At the shoulder, include a rounded concealed cap extending approximately 12–18% of the segment length beyond the visible seam so it can rotate underneath the torso/sleeve. At the elbow, include enough concealed continuation to overlap the forearm around the elbow pivot. Do not flatten-cut either joint.

## Forearm Prompt

> Generate only the [near/far] forearm asset for the approved character. Preserve exact scale and perspective. Include a rounded proximal elbow overlap zone and a concealed distal wrist extension. The elbow and wrist ends must be suitable for rotation without exposing background. Do not add strong interior outlines across hidden overlap zones.

## Hand Prompt

> Generate the approved character's [near/far] hand in the canonical perspective with a concealed wrist socket. Provide a neutral relaxed hand first. Optional additional poses may include open palm, pointing, fist, holding, and conversational gesture, but preserve the same hand size and wrist alignment in every pose.

## Thigh Prompt

> Generate only the [near/far] upper-leg/thigh asset in the exact canonical view. Preserve the garment shape. Extend the top of the thigh underneath the torso/tunic/skirt/pants by approximately 12–18% of thigh length. Include a rounded concealed knee continuation at the lower end. Do not cut at the visible clothing hem.

## Shin Prompt

> Generate only the [near/far] lower-leg/shin asset in the exact canonical view. Include a concealed rounded knee overlap at the top and an ankle extension at the bottom. Preserve garment folds and shading while avoiding hard seams inside hidden overlap regions.

## Foot Prompt

> Generate only the [near/far] foot/sandal asset matching the canonical perspective and scale. Include enough ankle socket artwork to overlap the shin. Preserve sandal construction, toe direction, line weight, and shadow style.

## Secondary Cloth Prompt

> Generate the character's [dupatta/scarf/cape/coat-tail] as independent cutout layers matching the canonical character. Separate only pieces that need independent motion. Ensure attachment roots extend beneath the torso/neck/shoulder region so motion does not expose gaps.

## Negative Constraints Block

Append when the model tends to improvise:

> Do not change face identity. Do not change view angle. Do not change hairstyle. Do not change garment design. Do not change color palette. Do not merge neighboring parts. Do not crop joints. Do not add extra fingers. Do not add labels or arrows. Do not create a crowded presentation sheet. Do not add scenery. Do not add ground shadow. Do not put visible gaps inside joints.

## Regeneration Prompt for Insufficient Overlap

> Repair only this asset. Keep its visible shape identical to the approved version. Extend the hidden joint artwork behind the neighboring part around the pivot. The added geometry must be concealed in the neutral assembled pose but remain visible when the joint rotates. Do not alter the character identity, colors, line weight, or external silhouette.

## Regeneration Prompt for Identity Drift

> The asset does not match the canonical character. Regenerate the same requested part using the canonical character as the strict identity source. Match proportions, color, line weight, perspective, garment construction, and shading exactly. Do not introduce a new face/body interpretation.
