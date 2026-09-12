# Anatomy visual quality and interaction standard

Working standard, September 11, 2026. The user selected [Hannah Newey’s *Cardiac Anatomy: External view of human heart*](https://sketchfab.com/3d-models/cardiac-anatomy-external-view-of-human-heart-a3f0ea2030214a6bbaa97e7357eebd58) as the visual benchmark for the nursing anatomy and physiology POC. Its browser rendering was visually inspected. This establishes an appearance and interaction target; it does not establish that its downloadable asset reproduces the presentation in A-Frame or runs acceptably on a headset.

## Reusable description

> Our visual standard is detailed medical illustration in interactive 3D: anatomically credible proportions and spatial relationships, natural tissue coloration, visible surface texture, and fine vascular detail. Deliberate color coding makes structures easy to distinguish while preserving a convincing organic appearance. Lighting should reveal shape, depth, and material differences clearly. Students should be able to examine the organ closely and rotate it without losing the sense that they are studying a carefully crafted anatomical specimen. Precise annotations and purposeful viewing angles should make the anatomy easier to understand.

Hannah’s heart supplies the reference for this standard: asymmetric organic form, subtle pink and red tissue variation, pale surface fat, fine surface markings, and branching vessels. The stronger red and blue vessel colors support identification. The desired style is realistic medical illustration; photographic appearance is not a requirement. Anatomical correctness and coverage of a lesson’s structures require separate faculty review.

## Visual acceptance criteria

| Area | What we should see in the running POC |
| --- | --- |
| Form | Convincing organ proportions, contours, grooves, and relationships between the structures used in the lesson. |
| Surface detail | Tissue texture and relevant small vessels remain legible at the intended study distance. |
| Materials and color | Tissue has subtle variation; vessel colors are consistent and explained. Surfaces retain their detail as the organ turns. |
| Lighting | Soft, controlled lighting reveals depth and does not wash out detail or make tissue look uniformly plastic. |
| Composition | The organ is prominent against a quiet background, with adequate space for labels. |
| Motion and scale | Rotation and zoom remain responsive at a useful viewing size on the actual target device. |
| Review evidence | Compare the POC and reference at matching views and approximate magnification. Record browser, device, load time, and observed responsiveness; obtain faculty review of the lesson’s anatomy. |

## Annotation and orientation standard

Preserve the strengths of the reference’s presentation: numbered markers anchored to structures, information available on selection, free inspection, and a useful view associated with each annotation. Sketchfab’s documented annotation selection can animate the camera to a stored view. [Viewer API functions](https://sketchfab.com/developers/viewer/functions)

For our learning tool, add these explicit requirements:

- Each marker identifies the intended structure precisely. Labels remain readable, minimize overlap, and do not cover the feature being taught.
- Selecting a structure brings it into a clear view. Students can return to an overview, move between lesson structures, or resume free inspection.
- Provide named anatomical views and identify the patient’s left and right. These are proposed POC controls, not a claim that Hannah’s current viewer already supplies them.
- Support a study mode with labels and a recall mode with labels hidden. Connect each prompt to an explicit learning objective and store the learner’s response separately from navigation events.
- On desktop, use smooth camera transitions. In immersive VR, keep head tracking under the learner’s control and offer deliberate organ reorientation instead of automatically rotating the learner’s view. The headset supplies the view direction in WebXR. [Three.js WebXR documentation](https://threejs.org/manual/en/webxr-basics.html)

## What renders the reference

The page uses Sketchfab’s hosted 3D viewer. Sketchfab documents WebGL as its browser rendering technology. The final appearance combines the artist’s model and surface data with the viewer’s materials, lighting, and rendering settings. The exact material setup of Hannah’s download has not been inspected. [Sketchfab rendering explanation](https://sketchfab.com/blogs/enterprise/3d-product-configurators/), [viewer features](https://sketchfab.com/features)

Sketchfab exposes a JavaScript Viewer API through an embedded iframe. Its documented integration is a hosted service dependency, rather than a locally controlled A-Frame renderer. [Viewer API introduction](https://sketchfab.com/developers/viewer)

## Two implementation paths

| Path | What it gives us | Boundary |
| --- | --- | --- |
| Embed Sketchfab and use its Viewer API | Reuses the published presentation and annotations; our page can add prompts, view buttons, and response capture. | Depends on Sketchfab hosting and terms. The embedded heart remains inside Sketchfab’s viewer; it does not become an object in our A-Frame scene. Headset behavior still needs testing. |
| Import a licensed model into A-Frame | Our own scene, interactions, organ placement, and WebXR experience. | Requires a suitable download and verification of geometry, textures, materials, performance, and reuse terms. Plan to recreate annotation controls and saved views; loading geometry does not reproduce Sketchfab’s application UI. |

A-Frame is built on three.js and provides a `gltf-model` component for glTF/GLB assets. Sketchfab offers model downloads in compatible formats, subject to availability and authentication. These capabilities establish a general import route; Hannah’s asset has not yet been imported or tested. [A-Frame architecture](https://aframe.io/aframe/), [model loading](https://aframe.io/docs/1.8.0/components/gltf-model.html), [Sketchfab download documentation](https://sketchfab.com/developers/download-api/downloading-models)

## Automation and learning objectives

Proposed application commands can sit behind a future MCP adapter:

| Our proposed command | Sketchfab implementation |
| --- | --- |
| `focus_structure(structure_id)` | Resolve an explicit structure-to-annotation mapping, then call `gotoAnnotation`. |
| `set_view(view_id)` | Resolve a saved camera position and target, then call `setCameraLookAt`. |
| `set_label_visibility(visible)` | Use the annotation visibility functions. |
| `get_learning_evidence()` | Read our application’s response and interaction log; this is not a Sketchfab competency assessment. |

The API also provides annotation selection/focus events. This is a feasible integration design based on documentation, not an implemented MCP server. Clicking or viewing a structure is interaction evidence, not evidence of mastery. [Viewer API functions](https://sketchfab.com/developers/viewer/functions)

A small lesson could ask the learner to locate a coronary artery, explain its function, and apply that explanation to the faculty-authored case. Store the prompt’s objective identifier, response, hints used, and faculty rubric result. We would define learning content and scoring in our application.

## First experiment

Use an embedded Sketchfab lesson to test whether three guided annotations and one explanation prompt help faculty and students engage with the case. Separately, test a permitted asset import into A-Frame to establish visual fidelity and headset interaction. An embed result answers the lesson-design question; it does not complete the A-Frame/WebXR experiment.

Hannah’s model page lists roughly 3 million triangles and a CC BY-NC-SA 4.0 license. Treat it as the agreed visual reference; incorporation of the actual asset must fit those terms or separate permission. No download, material audit, A-Frame import, or Quest/Vision Pro performance validation has been completed. [Model and license listing](https://sketchfab.com/3d-models/cardiac-anatomy-external-view-of-human-heart-a3f0ea2030214a6bbaa97e7357eebd58)
