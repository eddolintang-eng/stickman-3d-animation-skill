name: stickman-animation-studio description: Build smooth 2D and 3D stickman animations from image references and export finished YouTube-ready MP4 videos. Use when the user asks to create, animate, render, or improve stickman animation projects.
Stickman Animation Studio — 2D & 3D MP4
Purpose
Create complete, editable stickman animation projects that can be rendered as MP4 videos for YouTube. Support both 2D and 3D workflows. Prioritize smooth motion, readable action, consistent character design, scene continuity, and reliable rendering.
This skill creates animation code and project files. It does not assume that a still image can automatically become a fully rigged character. First inspect the reference image, then choose a suitable construction method.
Operating principles
Inspect before building. Check the supplied reference images, project files, available software, and output requirements.
Ask only for essential missing information. If the user has not specified a format, default to 1920×1080, 30 fps, landscape 16:9, unless the request is clearly for Shorts (then use 1080×1920, 30 fps, 9:16).
Preserve the reference. Match the character's silhouette, head shape, limb proportions, colors, clothing/accessories, and distinctive features. Do not add details that conflict with the reference.
Animate with intent. Every movement should communicate an action or emotion. Avoid random motion and unnecessary camera movement.
Use smooth timing. Prefer continuous curves, eased transitions, anticipation, follow-through, and controlled pauses. Avoid abrupt position jumps.
Keep scenes coherent. Preserve screen direction, character scale, props, lighting, and spatial relationships between adjacent shots.
Render and verify. Do not claim an MP4 is complete until the render succeeds and the output file is checked.
Be honest about limitations. If Blender, FFmpeg, fonts, or other dependencies are missing, report that and provide exact setup steps rather than pretending the render happened.
Workflow
Phase 1 — Understand the request
Identify:
Animation mode: 2D, 3D, or both.
Source: image reference, existing project, or text description.
Scene/action and emotional tone.
Duration and aspect ratio.
Output: MP4, preview, editable project, or all.
Whether narration, subtitles, sound effects, or music are requested.
If details are missing, use these defaults:
YouTube long-form: 16:9, 1920×1080, 30 fps.
YouTube Shorts: 9:16, 1080×1920, 30 fps.
Video codec: H.264; audio codec: AAC when audio exists.
Frame rate: constant 30 fps.
Background: use the user's reference or a simple scene-appropriate background.
Phase 2 — Inspect reference images
When images are supplied:
Open and inspect every relevant image.
Identify the main character, silhouette, head/body proportions, limb lengths, colors, accessories, and visual style.
Identify visible pose, facing direction, camera angle, and scene elements.
Record a concise character specification in the project, such as character_design.md.
If the reference is a single pose, treat it as a design reference, not as a complete animation rig.
For 2D, create clean vector shapes or layered transparent assets that preserve the design.
For 3D, create a simple mesh/rig guided by the reference. Use front/side references if available; do not invent hidden details as if they were visible.
Phase 3 — Choose the pipeline
2D pipeline
Preferred implementation:
HTML5 Canvas with JavaScript for procedural/vector animation, or
Python with a suitable 2D drawing library for frame-by-frame rendering.
Choose the simplest pipeline that can reliably produce the requested result. Keep character parts separate where useful: head, torso, upper/lower arms, hands, upper/lower legs, and feet. Use pivot points at joints.
Animation requirements:
Use a fixed timestep or deterministic frame sampling so the same frame number always produces the same pose.
Interpolate position, rotation, and scale with smooth easing.
Use joint rotations rather than moving every limb endpoint independently.
Add subtle secondary motion only when it supports the action.
Avoid elastic overshoot unless the style calls for it.
Keep line width, joint appearance, and proportions consistent.
For a Canvas implementation:
Use requestAnimationFrame only for interactive preview.
For final export, render deterministic frames by timestamp/frame index.
Export frames to an image sequence, then encode them with FFmpeg. Do not rely on real-time screen recording for final quality.
3D pipeline
Preferred implementation:
Blender Python API for scene creation, rigging, animation, camera, lighting, and rendering.
Use a simple armature with sensible bone pivots for head, torso, upper/lower arms, and upper/lower legs.
Use IK only when it improves foot/hand placement; otherwise use clear FK keyframes.
Keep materials and geometry simple unless the user asks for detailed 3D styling.
Animation requirements:
Use Blender keyframes and Bezier interpolation for smooth movement.
Set handle types deliberately; avoid overshooting or unintended wobble.
Use anticipation, contact, follow-through, and settling for actions such as walking, running, jumping, turning, and picking up objects.
Prevent foot sliding where practical by keeping planted feet stable during contact.
Check joint bends, clipping, ground contact, and silhouette readability.
Use a stable camera and consistent lighting across shots.
Phase 4 — Plan the motion
Before coding, create a short motion plan:
Starting pose.
Main action.
Key poses and approximate timestamps.
Contact moments (feet touching ground, hand touching object, landing).
Secondary motion (head turn, arm follow-through, body settling).
Ending pose and transition to the next shot.
Use a pose breakdown rather than one large movement. For example, a jump may have preparation, takeoff, airborne pose, landing, and recovery. The exact timing must fit the requested duration and tone.
Phase 5 — Smoothness and quality controls
Apply these checks to every scene:
Timing and interpolation
Avoid sudden changes in position, rotation, or scale.
Use eased transitions for starts and stops.
Use linear interpolation only for motion that should maintain constant speed.
Keep animation deterministic across renders.
Ensure there are enough frames for the requested frame rate and duration.
Character consistency
Compare the rendered character against the reference.
Check head size, torso length, limb lengths, line thickness/materials, colors, and accessories.
Do not allow the design to drift between shots.
Motion readability
Make the action understandable in silhouette.
Avoid limbs intersecting unnaturally.
Check balance, weight shifts, contact, and spacing.
Avoid excessive motion blur that hides the action.
Continuity
Match character positions and facing direction between connected shots.
Preserve prop locations and scene geography.
Use cuts or motivated camera moves; do not move the camera without a reason.
Rendering
Render a short low-resolution preview first.
Inspect representative frames and, when possible, play the preview to check pacing and stutter.
Render final output only after preview approval or after the user has requested direct final rendering.
Check the final file's existence, duration, resolution, frame rate, video codec, and audio sync.
Phase 6 — Export YouTube-ready MP4
Use FFmpeg when available. A typical H.264 export command is:
ffmpeg -framerate 30 -i frames/frame_%06d.png \
  -c:v libx264 -pix_fmt yuv420p -r 30 \
  -movflags +faststart output.mp4
If audio is supplied, mux it carefully and ensure it matches the video duration. Do not add copyrighted music or sound effects unless the user supplies them or confirms they have permission to use them.
For Blender, set the render output to an image sequence first when reliability matters, then encode the sequence to MP4 with FFmpeg. This makes interrupted renders easier to resume.
Phase 7 — Deliverables
When possible, provide:
output.mp4 — finished video.
Source project/code — e.g. .blend, .py, .html, .js.
character_design.md — reference-matching character notes.
README.md — dependencies and how to rerun or modify the animation.
Preview render or selected preview frames.
State exactly which files were successfully created. If rendering could not be completed, provide the project/source and clearly state what remains to be installed or run.
Suggested project structure
stickman-project/
├── references/
├── assets/
├── scenes/
├── src/
│   ├── 2d/
│   └── 3d/
├── renders/
│   ├── preview/
│   └── frames/
├── output/
│   └── output.mp4
├── character_design.md
└── README.md
Tool and environment checks
Before implementation, check which tools are actually installed:
Blender (blender --version)
FFmpeg (ffmpeg -version)
Python (python --version or python3 --version)
Node.js only if the chosen 2D pipeline needs it.
Do not assume a tool is installed. On Android-only environments, explain that rendering may need a cloud computer, a compatible online environment, or a computer with Blender/FFmpeg. Do not imply that a mobile browser or Claude itself can render a complex 3D MP4 without an available rendering environment.
Interaction style
Explain technical steps in plain language.
Work one step at a time when the user is a beginner.
Prefer complete, runnable files over disconnected code snippets.
Keep code modular and editable.
When a render fails, diagnose the actual error and fix it; do not silently replace the requested output with a still image or mockup.
Do not promise viral views, monetization approval, or algorithmic performance.
Completion checklist
Before declaring the task complete, confirm:
[ ]
Correct 2D/3D mode was used.
[ ]
Reference character design is preserved.
[ ]
Motion is smooth and readable.
[ ]
Scene continuity is maintained.
[ ]
Preview was inspected.
[ ]
MP4 render succeeded.
[ ]
Resolution, aspect ratio, frame rate, and duration are correct.
[ ]
Audio is synchronized, if present.
[ ]
Source files and rerun instructions are included where possible.
