# FEYNMAN

A browser-based toolkit for drawing Feynman diagrams and computing scattering amplitudes step by step. No installation required — open either HTML file directly in a browser.

---

## Quick Start — The Full Pipeline

This toolkit has two files that work together. Here is the complete workflow from drawing to final result.

### Step 1 — Open the editor

Open `feynman_editor.html` in any modern browser (Chrome, Firefox, Edge). No server needed — it runs entirely locally.

You will see:
- A dark canvas in the centre for drawing
- A sidebar on the left with drawing tools and vertex templates
- A properties panel on the right that updates when you select something
- A Feynman Rules panel at the bottom (hidden by default)

### Step 2 — Draw your diagram

**Place vertices** using the **Vertex** tool (or press `V`). Click anywhere on the canvas.

**Connect them with particle lines** by selecting a line tool from the sidebar (or keyboard shortcuts below), then clicking the first vertex and then the second. The available line types are:

| Tool | Key | Looks like | Used for |
|---|---|---|---|
| Fermion | `F` | Solid line with arrow | Quarks, leptons, neutrinos |
| Photon | `P` | Wavy line | Photons, W/Z bosons |
| Gluon | `G` | Curly line | Gluons |
| Scalar | `S` | Dashed line | Higgs, scalar particles |
| Ghost | `H` | Dotted line with arrow | Faddeev-Popov ghosts |

**Use the built-in vertex templates** in the left sidebar under *Theory Vertices*. Click a template name to activate it, then click the canvas to stamp it. Includes QED vertex, QCD vertices, weak interaction vertices (W, Z, H), and more.

**Label vertices and lines** by double-clicking them. This sets the momentum label (e.g. `p₁`, `k`) or vertex name (e.g. `e`, `Zf`, `lW`).

**Mark antiparticle lines** by selecting a fermion line and checking *Reverse arrow* in the properties panel. This is what makes the editor produce `v̄` (antiparticle) spinors instead of `u` (particle) spinors in the amplitude.

**External legs** (the stubs at the edges of your diagram) should have their vertex style set to **Invisible**. The editor automatically identifies them as incoming or outgoing particles.

> **Tip:** The properties panel on the right shows exactly what will happen to each selected element — which Feynman rule will apply to a vertex, what spinor a fermion leg will produce, what propagator an internal line carries.

### Step 3 — Configure Feynman rules (optional)

Click **Feynman Rules ▾** in the top bar. This opens a panel where you can:
- View and edit propagator formulas for each particle type
- View and edit vertex rules (matched to vertices by label)
- Add custom vertex rules for non-standard interactions

The built-in rules cover QED, QCD, and standard weak interactions. For a new theory, add your own vertex rule with a label and expression.

### Step 4 — Build the amplitude

Click **iM ▶** in the top bar. The editor reads your diagram, applies the Feynman rules, and generates the full amplitude expression in the panel at the bottom, for example:

```
iM =
    iQeγ^μ             ← vertex (e)
  × −ig_μν/s₁₂         ← photon prop (p₁+p₂)
  × iQeγ^ν             ← vertex (e)
  × u(p₁,s)            ← fermion ext in (p₁)
  × v̄(p₂,s)            ← fermion ext out (p₂)
  × ū(p₃,s)            ← fermion ext out (p₃)
  × v(p₄,s)            ← fermion ext in (p₄)
```

Each factor shows the mathematical expression and a label describing what it is. The **Copy LaTeX** button copies the amplitude in LaTeX format.

### Step 5 — Send to the stepper

Click **→ Stepper** in the top bar. This exports the amplitude to `amplitude_stepper.html` via your browser's local storage.

Switch to `amplitude_stepper.html` (open it in another tab or window first).

### Step 6 — Compute |M̄|²

In `amplitude_stepper.html`, click **Import from Editor**. The stepper:

1. Reads the amplitude from the editor
2. Identifies the topology (two-current, Compton, decay, loop, etc.)
3. Squares the amplitude: |M|² = iM × (iM)*
4. Applies spin sums: Σ u ū = p̸ + m
5. Applies photon polarisation sums: Σ ε ε* = −g_{μν}
6. Evaluates the resulting traces using γ-matrix identities
7. Substitutes Mandelstam variables s, t, u
8. Gives the final spin-averaged result |M̄|²

Every step is shown on screen. You can scroll through the full derivation.

### Step 7 — Two-diagram processes

For processes with two contributing diagrams (e.g. Bhabha scattering has t-channel and s-channel), click **+ Two Diagrams** in the stepper. Enter the second diagram's trace expression and the stepper automatically computes the cross-term interference 2Re(M₁M₂\*) using the 8-gamma trace identity.

---

## Editor Controls

### Mouse

| Action | Result |
|---|---|
| Click canvas (with line tool active) | Start drawing a line from a vertex |
| Click second vertex | Complete the line |
| Click same vertex twice (with line tool) | Draw a self-loop |
| Click vertex or line (Select tool) | Select it — properties appear on right |
| Click and drag vertex | Move it |
| Click and drag canvas (no vertex) | Box-select multiple vertices |
| Drag inside selection box | Move the whole group |
| Drag the control point on a line | Curve the line |
| Double-click vertex or line | Set its label |
| Double-click empty canvas | Place a new vertex |
| Right-click drag | Pan the canvas (any tool) |
| Scroll wheel | Zoom |

### Keyboard

| Key | Action |
|---|---|
| `Q` | Select / Move tool |
| `M` | Pan tool |
| `V` | Vertex tool |
| `F` | Fermion line |
| `P` | Photon line |
| `G` | Gluon line |
| `S` | Scalar line |
| `H` | Ghost line |
| `X` | Delete tool |
| `Del` | Delete selected vertex or line |
| `Ctrl+Z` | Undo (up to 80 steps) |
| `Ctrl+C` | Copy selected |
| `Ctrl+V` | Paste |
| `Space` | Rotate stamp 90° (when placing a template) |
| `Esc` | Cancel current action |
| `0` | Reset zoom and pan |

### Top bar buttons

| Button | Action |
|---|---|
| **Export PNG** | Save diagram as an image |
| **Clear** | Erase everything (asks for confirmation) |
| **Undo** | Undo last action |
| **Grid** | Toggle grid snapping |
| **Labels** | Toggle momentum labels on canvas |
| **Snap** | Toggle snap-to-grid |
| **Feynman Rules ▾** | Open/close the rules and amplitude panel |
| **iM ▶** | Build the amplitude from the current diagram |
| **Copy LaTeX** | Copy the amplitude as LaTeX |
| **→ Stepper** | Export the amplitude to the step solver |

### Properties panel (right side)

Click any vertex or line to see its full readout:

**Vertex selected:**
- *Label* — the name used to look up the Feynman rule (e.g. `e` for QED, `Zf` for Z-fermion, `lW` for W-lepton)
- *Style* — how the vertex is drawn; set to **Invisible** for external leg endpoints
- *Feynman rule* — shows exactly which factor will appear in the amplitude and how it was matched
- *Connected lines* — lists every line attached, with type, label, and whether it is an external leg or internal propagator

**Line selected:**
- *Momentum label* — e.g. `p₁`, `k`, `q`
- *Particle type* — Fermion / Photon / Gluon / Scalar / Ghost
- *Reverse arrow* — check this for antiparticle lines; determines whether the amplitude uses `u/ū` (particle) or `v/v̄` (antiparticle) spinors
- *Status* — External leg or Internal propagator
- *Amplitude factor* — the exact factor this line will contribute: spinor formula for external legs, propagator formula for internal lines
- *Connects* — which two vertices, with arrow direction
- *Reset curve* — straightens a curved line

---

## Stepper Controls

### Input field

Type or paste any of the following:

```
(e4/s2) * Tr[p1 gm p2 gn] * Tr[p3 gm p4 gn]   ← trace expression
iM = iQeγ^μ × −ig_μν/s × iQeγ^ν × u(p₁) × …   ← full iM from editor
← vertex (e [auto])                               ← comment-line format
```

Unicode is accepted: `γ^μ`, `×`, `p̸`, subscript numbers `₁₂₃₄`.

### Buttons

| Button | Action |
|---|---|
| **Solve ▶** | Compute and display all steps |
| **+ Two Diagrams** | Toggle two-input mode for multi-diagram interference |
| **Import from Editor** | Load the last amplitude exported via **→ Stepper** |
| **Copy LaTeX** | Copy the final result as LaTeX |
| **Syntax ?** | Show the input syntax reference |
| **Ctrl+Enter** | Solve (keyboard shortcut) |

### Example buttons

The row of grey buttons loads pre-built examples:

| Button | What it loads |
|---|---|
| **From Editor (iM)** | Simulates a full editor import (e⁺e⁻→μ⁺μ⁻) |
| **e⁺e⁻→μ⁺μ⁻** | s-channel two-current trace |
| **e⁺e⁻→μ⁺μ⁻ (massive)** | Same with (p+m) mass corrections |
| **e⁻μ⁻→e⁻μ⁻** | t-channel trace |
| **Compton (s-ch)** | Single s-channel Compton diagram |
| **Bhabha (t+s)** | Opens two-diagram mode, Bhabha interference |
| **Møller (t+u)** | Opens two-diagram mode, Møller interference |
| **Tr[p1 p2]** | Simple two-momentum trace |
| **Tr[p1 gm p2 gm]** | Internal index contraction |
| **Spinor bilinear** | ubar(p3) gm u(p1) — shows squaring steps |
| **QCD t-channel** | QCD with colour factor |

### Helicity tab

Click the **Helicity (n-gluon)** tab for massless pure-gauge amplitudes. Set the number of gluons (3–8) and assign `+` or `−` helicity to each. The stepper applies Parke-Taylor (MHV) or BCFW recursion (NMHV and beyond).

---

## Worked Example — e⁺e⁻ → μ⁺μ⁻

This is the simplest nontrivial QED process: an electron and positron annihilate via a virtual photon and produce a muon pair.

**1. Draw the diagram**

Open `feynman_editor.html`. Stamp the **QED vertex** template twice from the sidebar. Connect the two photon stubs with a photon line. You now have two QED vertices connected by a virtual photon, with four external fermion stubs.

Label the vertices `e` (they already are by default). The four external stubs get labels p₁, p₂, p₃, p₄ automatically when you build the amplitude.

Mark the two positron/antimuon lines as **Reverse arrow** in the properties panel (the lines whose arrows should point away from the vertex).

**2. Build the amplitude**

Click **iM ▶**. The panel shows:
```
iM = iQeγ^μ × −ig_μν/s₁₂ × iQeγ^ν × u(p₁,s) × v̄(p₂,s) × ū(p₃,s) × v(p₄,s)
```

**3. Export and import**

Click **→ Stepper**. Switch to `amplitude_stepper.html`. Click **Import from Editor**.

**4. Read the result**

The stepper shows 9 steps:

1. Input amplitude iM
2. Write in factored form: J₁^μ × (−ig_{μν}/s) × J₂_μ
3. Square: |M|² = iM × (iM)*
4. Apply spin sums → two traces
5. Expand each trace using Tr[p̸ γ^μ p̸ γ^ν] = 4(…)
6. Contract using master formula: 32[(A·C)(B·D)+(A·D)(B·C)]
7. Substitute Mandelstam variables
8. Evaluate: 8(t²+u²)
9. Final result: **2e⁴(t²+u²)/s²**

This matches the textbook result (Griffiths §9.2 ✓).

---

## Customisation

The toolkit is designed to be extended. Nothing in the computation is hardcoded to a specific theory — the stepper derives results from trace identities applied to whatever diagram you provide.

### Adding new vertex rules

Open the **Feynman Rules** panel (top bar) and scroll to the **Vertex rules** section. Click **+ Add vertex rule** and fill in:

- **Label** — the string you will type when labelling a vertex on the canvas (e.g. `myV`)
- **Display text** — the factor expression shown in the amplitude (e.g. `ig_new γ^μ`)
- **LaTeX** — the LaTeX version for copy-to-clipboard

Click **Save rules**. Now any vertex labelled `myV` will use that factor when you click **iM ▶**.

The label always takes priority over signature matching, so the same line configuration (e.g. two fermions + one photon) can produce different factors depending on the vertex label.

### Customising propagators

In the **Feynman Rules** panel, each particle type (Fermion, Photon, Gluon, Scalar, Ghost) has a propagator expression with two placeholder variables:

| Placeholder | Replaced with |
|---|---|
| `{p}` | The momentum label of that line |
| `{p2}` | The squared momentum label |

Example: the default fermion propagator is `i(/{p}+m)/({p2}−m²)`. If a line is labelled `k`, this becomes `i(/k+m)/(k²−m²)`.

To model a massive gauge boson (W, Z), draw the propagator as a photon-type line and change the photon propagator in the panel to `−ig_μν/({p2}−m_W²)` for that session.

### Customising external leg spinors

The **External legs** section of the Rules panel controls what factor appears for each type of external particle. The default entries cover all standard QED/QCD cases. You can change them for any theory — for example, to use a different spinor normalisation convention or to add a coupling factor to the external state.

The arrow direction on fermion lines (set via *Reverse arrow* in the properties panel) determines which rule is used:

| Arrow direction | Rule used | Default factor |
|---|---|---|
| Into diagram (normal) | `fermion_in` | `u(p, s)` |
| Out of diagram (normal) | `fermion_out` | `ū(p, s)` |
| Into diagram (reversed) | `fermion_anti_in` | `v(p, s)` |
| Out of diagram (reversed) | `fermion_anti_out` | `v̄(p, s)` |

### Saving vertex templates

Draw any subgraph (e.g. a new exotic vertex with its stub legs), select the central vertex, and click **Save as Template** in the properties panel. Give it a name. It then appears in the left sidebar under *Theory Vertices* and can be stamped onto any future diagram.

Templates store the full edge structure including particle types and arrow directions, so a W-lepton vertex template will always produce the correct fermion line configuration.

### Using the stepper for new theories

The stepper does not need to know which theory you are computing in. It works on any amplitude that can be expressed as:

- A product of two traces — `(coupling/denom) * Tr[...] * Tr[...]`
- A single trace with internal contractions
- A Compton-type amplitude (one fermion chain + photon legs)

To compute a squared amplitude for a new interaction:

1. Draw the diagram in the editor with your custom vertex labels and rules
2. Export via **→ Stepper**
3. The stepper reads the actual momenta from the diagram and derives the result using γ-matrix identities — it does not look up the answer

For the trace-expression input mode, you can type any expression directly and the stepper will evaluate it. This allows computing amplitudes for theories with no editor support yet, as long as you can write the squared amplitude as a trace.

### Extending the Feynman rules defaults

The default rules are defined at the top of `feynman_editor.html` in the `FR_DEFAULTS` object. To permanently add a new particle type, propagator, or vertex rule, edit that object directly. The structure is:

```javascript
FR_DEFAULTS.propagators['myParticle'] = {
  text:  '−iΔ(p)/{p2}',
  latex: '\\frac{-i\\Delta(p)}{{p2}}'
};

FR_DEFAULTS.vertices.push({
  label: 'myV',
  sig:   {fermion: 2, myParticle: 1},
  text:  'ig_new γ^μ',
  latex: 'ig_{\\rm new}\\gamma^{\\mu}'
});
```

Add the corresponding external leg rules if your new particle type appears as an external state. The editor will automatically use these rules for any diagram containing that vertex or propagator.

---

## Files

### `feynman_editor.html` — Diagram Editor

An interactive canvas for drawing Feynman diagrams from scratch.

**Drawing tools**
- Place vertices (dot, cross, circle, blob, or invisible styles)
- Draw particle lines: Fermion, Photon, Gluon, Scalar, Ghost
- Each line type renders with its correct physics notation (wavy photon, curly gluon, dashed scalar, dotted ghost, arrowed fermion)
- Self-loops supported on any line type

**Editing**
- Select, move, and drag individual vertices or groups (box-select)
- Drag the bezier control point on any line to curve it
- Double-click a vertex or line to set its momentum/particle label
- Reverse arrow direction on fermion/ghost lines (marks antiparticle lines — used to determine correct v/v̄ spinors)
- Undo (Ctrl+Z), up to 80 steps
- Pan and zoom the canvas

**Theory vertex library**

Built-in templates (sidebar → Theory Vertices):

| Template | Vertex factor |
|---|---|
| QED vertex | iQeγ^μ |
| QCD q-g vertex | ig_s T^a γ^μ |
| QCD 3g vertex | g_s f^{abc}[…] |
| QCD 4g vertex | −ig_s²[…] |
| φ⁴ vertex | −iλ |
| W lepton vertex | ig/(2√2) γ^μ(1−γ⁵) |
| W quark vertex | iV_CKM g/(2√2) γ^μ(1−γ⁵) |
| Z vertex | ig/(2cosθ_W) γ^μ(g_V−g_Aγ⁵) |
| H–fermion vertex | −im_f/v (Yukawa) |
| WWγ / WWZ | triple gauge boson |
| HWW / HZZ | Higgs–gauge |

Save any connected component as a reusable template. Stamp templates with 90° rotation support.

**Feynman Rules panel**

- Configure propagator expressions for all particle types
  - Photon/gluon propagators use correct lower-index notation: −ig_{μν}/q²
- Configure external leg spinors — antiparticle lines (reversed arrows) automatically produce v̄/v spinors instead of ū/u
- Named vertex rules matched by vertex label (label takes priority over signature matching)
- Loop measure configurable
- Click **iM ▶** to auto-generate the full amplitude from the current diagram
- **→ Stepper** exports the amplitude to the Step Solver

**Export**
- Export diagram as PNG
- **→ Stepper** exports amplitude to `amplitude_stepper.html` via localStorage

---

### `amplitude_stepper.html` — Step-by-Step Amplitude Solver

Takes a QFT scattering amplitude and walks through the full computation step by step — squaring, spin/polarisation/colour sums, trace algebra, Mandelstam substitution — to a final symbolic result. Designed to derive results from first principles so it works for new or unknown interactions.

**Topology detection**

The stepper reads the amplitude structure and routes to the appropriate handler:

| Topology | Example | What it computes |
|---|---|---|
| Two-current (QED) | e⁺e⁻→μ⁺μ⁻, Bhabha | Full squaring → traces → Mandelstam → result |
| Two-current (QCD) | qq→qq | Same + colour factor N_c²/[2(N_c²−1)] |
| Single Compton-type | γγ→e⁺e⁻ (one diagram) | Derives result from trace identities for that diagram; notes what is missing |
| 1→2 vector decay | Z→ff̄ | Polarisation sum, trace with γ⁵, decay width Γ formula |
| 1→2 scalar decay | H→ff̄ | Yukawa trace, helicity suppression, Γ formula |
| 2→3 bremsstrahlung | e⁻γ radiation | Eikonal factorisation, IR divergence, Bloch-Nordsieck |
| Loop | Any 1-loop diagram | Full dim-reg procedure: PV reduction, Feynman params, Wick rotation, B₀/C₀/D₀ |
| γγ→γγ / gg→gg box | Fermion box | Why squaring before integrating is wrong; 3 topologies; colour trace |
| Pure gauge | gg→gg | → Helicity tab (Parke-Taylor / BCFW) |

**Trace computation — not hardcoded**

For single Compton-type diagrams the stepper derives the result from scratch:

1. Reads propagator momentum from the factor list
2. Computes all dot products from the Mandelstam DP table
3. Applies the three trace contraction identities step by step:
   - `γ^μ p̸ γ_μ = −2p̸`
   - `q̸ p̸ q̸ = 2(q·p)q̸ − q²p̸`
   - `γ^ε A̸ γ_ε = −2A̸`
4. Collapses the result using s+t+u=0

**Two-diagram interference (automated)**

For processes with two diagrams at the same order (Bhabha, Møller, etc.) use **+ Two Diagrams** mode. The cross-term 2Re(M₁M₂\*) is derived from the 8-gamma trace identity:

```
Tr[γ^μ p̸_A γ^ν p̸_B γ_μ p̸_C γ_ν p̸_D] = −32(A·C)(B·D)
```

**General formula:** interference = 4e⁴ × (missing Mandelstam)² / (denom₁ × denom₂)

| Process | Denominators | Interference |
|---|---|---|
| Bhabha e⁺e⁻→e⁺e⁻ | t + s | 4e⁴u²/(ts) |
| Møller e⁻e⁻→e⁻e⁻ | t + u | 4e⁴s²/(tu) |

**Physics implemented**

- 4-element trace identity and double-trace contraction master formula
- 8-gamma trace for interference terms
- Massive spin sums with (p̸+m) corrections
- Photon polarisation sum: Σ εε\* = −g_{μν}
- QCD colour algebra and averaging factors
- Mandelstam substitution (4-point 2→2 massless)
- Z→ff̄ decay: axial trace with γ⁵, partial width
- H→ff̄: Yukawa coupling, helicity suppression
- 2→3 bremsstrahlung: Low's theorem, IR divergence
- Loop integrals: dim-reg, Feynman parametrisation, B₀/C₀/D₀
- γγ→γγ / gg→gg box: why pre-integration squaring fails, colour trace

**Input syntax**

```
Traces:       Tr[p1 gm p2 gn]
Momenta:      p1…p8  (always slashed inside Tr)
Gammas:       gm gn gr gs ga gb  (matching labels are contracted)
Scalars:      e2  e4  gs2  gs4  m2
Denominators: /s  /t  /u  /s2  /t2  /u2
Mass terms:   (p1+m)  (p1-m)  inside Tr
Spinors:      ubar(p1) gm u(p3)
Unicode:      γ^μ  ×  p̸  ε^μ  subscripts — all accepted
```

---

## Verified Results

| Process | Result | Reference |
|---|---|---|
| e⁻μ⁻→e⁻μ⁻ | 2e⁴(s²+u²)/t² | Griffiths §9.2 ✓ |
| e⁺e⁻→μ⁺μ⁻ | 2e⁴(t²+u²)/s² | Griffiths ✓ |
| Compton e⁻γ→e⁻γ | −2e⁴(s/u+u/s) | Griffiths §9.5 ✓ |
| e⁺e⁻→γγ | −2e⁴(t/u+u/t) | Griffiths §9.7 ✓ |
| Bhabha (full) | 2e⁴[(s²+u²)/t²+(t²+u²)/s²+2u²/(st)] | Griffiths §9.4 ✓ |
| Møller (full) | 2e⁴[(s²+u²)/t²+(s²+t²)/u²+2s²/(tu)] | Griffiths §9.3 ✓ |
| γγ→e⁺e⁻ (t-ch) | 2e⁴u/t | Derived ✓ |
| gg→gg (tree) | (9/2)g_s⁴(3−tu/s²−su/t²−st/u²) | Mangano-Parke ✓ |
| Z→ff̄ | N_c g²(g_V²+g_A²)m/(12π) | Standard EW ✓ |
| H→ff̄ | N_c y_f² m_H(1−4m_f²/m_H²)^{3/2}/(8π) | Standard EW ✓ |
