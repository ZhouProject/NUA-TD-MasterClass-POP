# TD Master Class POP

Jojo - z.zhou@norwichuni.ac.uk

**What we will do:** In this second master class we move from 2D image instancing to TouchDesigner’s new GPU-based Point Operators (POPs), using them to build generative, body-responsive point, particle and line systems, then apply interaction-design methods to develop your own interactive prototype in a guided studio sprint.

## Before the session

Allow some time to do the four items in order: the first two are the POP primer, the last two are about designing interaction.

### Reading task

| # | Item | Type | Why |
| --- | --- | --- | --- |
| 1 | Derivative, [Learning About POPs](https://docs.derivative.ca/Learning_About_POPs) | Official primer | Points, primitives, vertices and attributes: the vocabulary for the whole day |
| 2 | Derivative Curriculum, [Lesson 109: POPs, Working with Points](https://learn.derivative.ca/courses/100-fundamentals/lessons/109-pops-working-with-points/topic/fundamentals-of-pops/) | Short videos + example files | Watch “Fundamentals of POPs” and “Using POP Attributes”; the rest is optional |
| 3 | Tom Igoe, [Making Interactive Art: Set the Stage, Then Shut Up and Listen](https://www.tigoe.com/blog/category/physicalcomputing/405/) | Short essay | Why an interactive piece should invite action rather than explain itself |
| 4 | Bill Verplank, [Interaction Design Sketchbook](https://ccrma.stanford.edu/courses/250a-fall-2014/lectures/IDSketchbok.pdf) | Course notes | Read the opening on Do, Feel and Know; we use it in the afternoon |

### Bring to class

- One interaction idea, written as three sentences: what a person **does**, what they **feel** (see or hear), and what they come to **know** about the system.
- One reference: an artwork or installation that uses points, particles or lines, with a link.

### Setup

- [ ] Bring your Master Class 1 files, including the MediaPipe component, and a laptop with a webcam.


## Lab tasks

Each lab is 40 minutes. Use OP Snippets (Help menu, or right-click an operator) whenever you meet a POP you have not used: every POP has a small working example you can copy.

### Lab 1 · Image to points, rebuilt

**Goal:** rebuild your Master Class 1 image-to-instances patch with POPs, and compare the two.

1. Bring in a camera (Video Device In TOP) or a movie (Movie File In TOP). Reduce it to about 256 × 256 with a Fit or Resolution TOP.
2. Add a TOP to POP. Set it to use brightness as height, and adjust the displacement scale until the image has depth.
3. Carry the image’s colour onto the points as a `Color` attribute (TOP to POP’s Color or Custom mapping, or a Lookup Texture POP).
4. Add a Noise POP on `P` and animate its translate over time (for example with `absTime.seconds`).
5. Render it: Geometry COMP, Camera COMP, Light COMP if needed, Render TOP. Set point size with the `PointScale` attribute.

**Check:** open your Master Class 1 patch and compare cook times in the Performance Monitor.

**Stretch:** change TOP to POP’s Connectivity from points to triangles. The same data now renders as a surface.

**If your points are invisible:** check that they have a point primitive, and that `PointScale` is not tiny.

### Lab 2 · Living points

**Goal:** give the points memory and a zone of influence.

1. Feed your Lab 1 points into a Particle POP as the emitter.
2. Set a life span and a little drag so particles fade out rather than pile up.
3. Add one force inside the particle loop: Force Radial POP (attract, repel or spiral) or a Noise POP.
4. Add a Field POP (a sphere is fine). Use its `Weight` to limit an effect to the inside of the field, for example on the Noise POP’s Map page, so Amplitude follows Weight.
5. Draw with the particles: a Trail POP for trails, or a Proximity POP to join nearby particles with lines.

**Check:** move the field’s centre with an LFO CHOP. The LFO stands in for a hand; in Lab 3 you replace it.

**Stretch:** colour particles by age, mapping `PartAge` to `Color` (Math POP, or Lookup Texture POP against a Ramp TOP).

**If the simulation misbehaves:** press Initialize, then Start, on the Particle POP.

### Lab 3 · Body to points

**Goal:** replace the placeholder input with a body, and test whether a stranger can read the interaction.

1. Open `lab2.toe` and add the MediaPipe component from Master Class 1.
2. **Route A, the hand as a control:** select the index fingertip channels, remap them from MediaPipe’s 0–1 range to your scene, and export them to the Field POP’s centre (or the Force Radial POP’s Position) in place of the LFO.
3. **Route B, the hand as geometry:** use a Shuffle CHOP so the 21 hand landmarks become x, y, z channels with one sample per landmark, then a CHOP to POP to turn each sample into a point.
4. Join the landmark points with a Proximity POP, or give them a Trail POP.
5. Write your Do / Feel / Know in three sentences.

**Check, the 30-second test:** swap seats with a partner and say nothing. Can they find the rule within 30 seconds? Write down what they tried first.

**If nothing moves:** check the channel names in your MediaPipe component’s output, and that the remapped values land inside your camera’s view.

## Studio sprint: project briefs

Choose one brief, or propose your own that follows the same rule: use at least **one** technique from Master Class 1 (communication between software or devices, image instancing, MediaPipe) and at least **two** POP verbs from today (make, shape, move, connect). Start from your Lab 3 file.

| Brief | The experience | From Master Class 1 | POPs to start with | Stretch |
| --- | --- | --- | --- | --- |
| A · The mirror that remembers | A body in front of the camera leaves traces that fade slowly; standing still lets the image settle | MediaPipe or camera input | Trail, Feedback, Particle | Traces that respond to how fast someone moves |
| B · Sound you can see | Sound from Ableton Live or Max/MSP, sent over OSC, shapes a particle field; a performer controls both | OSC between software | Field, Force Radial, Noise | Map several sound features to one force (many to one) |
| C · The hidden portrait | An image becomes a point cloud that only resolves where a hand passes, and scatters again behind it | Image to points, MediaPipe | TOP to POP, Field, Noise | Two images, one revealed by each hand |
| D · Out of the screen | The point system drives something physical or another app | Communication with external devices | DMX Fixture and DMX Out POPs, or POP to DAT for OSC out | A physical light that follows a virtual particle |

### What to show

Two minutes each:

- A live demo, or a 30-second screen recording.
- Your mapping, as one sketch: inputs on the left, POP effects on the right, lines between.
- Your Do / Feel / Know, in three sentences.
- One thing you would change after the 30-second test.

### After the session

Upload to the relevant files to GitHub.

## Further resources

Optional, for after the session or for students who want to go further.

### TouchDesigner POPs

| Resource | What it covers | Access |
| --- | --- | --- |
| Derivative, [POP operator reference](https://docs.derivative.ca/Category:POPs) | Every POP, with parameters | Free |
| Derivative, [Write GLSL POPs](https://docs.derivative.ca/Write_GLSL_POPs) | Custom compute shaders for points | Free, advanced |
| Derivative, [POP Topology](https://docs.derivative.ca/POP_Topology) | Why some POP counts stall the CPU, and how to avoid it | Free, advanced |
| Interactive & Immersive HQ, [SOPs to POPs: Reimagining Geometry Workflows](https://interactiveimmersive.io/blog/touchdesigner-operators-tricks/sops-to-pops-reimagining-geometry-workflows/) (Vernetti, 2025) | The same patch built with SOPs and with POPs, side by side | Free article |
| Interactive & Immersive HQ, [POPs in TouchDesigner FAQ](https://interactiveimmersive.io/blog/touchdesigner-3d/pops-in-touchdesigner-faq/) | Common questions; instancing from POP attributes | Free article (written during the alpha) |
| Dean Cheesman, [Create Point Typography](https://derivative.ca/community-post/tutorial/create-point-typography/73967) (2026) | Text and images as points without instancing | Free video |
| Bileam Tschepe (elekktronaut), [tutorials on derivative.ca](https://derivative.ca/tags/elekktronaut) | Includes POP experiments and blob tracking with POPs | Free videos |
| Darien Brito, [POPs Foundations](https://derivative.ca/workshop/pops-foundations-darien/74370) | Three-hour structured introduction, recorded | Paid workshop |
| Derivative, [Optimize](https://derivative.ca/UserGuide/Optimize) | Finding and fixing slow operators | Free |

### Designing interactive work

| Reading | Why it is useful here | Access |
| --- | --- | --- |
| Golan Levin, [Computer Vision for Artists and Designers](https://flong.com/archive/texts/essays/essay_cvad/index.html) (2006) | Camera-based interaction from first principles, including how lighting and the physical set-up make tracking reliable | Free essay |
| Andy Hunt and Marcelo Wanderley, “Mapping performer parameters to synthesis engines”, *Organised Sound* 7(2), 2002 | One-to-one, one-to-many and many-to-one mappings, and how they feel to perform | Library |
| Dan Saffer, [*Microinteractions*](https://www.oreilly.com/library/view/microinteractions/9781449342760/) (O’Reilly, 2013) | Trigger, rules, feedback, loops: a checklist for a single interaction | Book; O’Reilly via library |
| Kristina Höök, *Designing with the Body: Somaesthetic Interaction Design* (MIT Press, 2018) | Designing from felt, bodily experience rather than screens | Book |
