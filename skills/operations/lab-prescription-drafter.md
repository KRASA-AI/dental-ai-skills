---
name: "Lab Prescription Drafter"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~8 min/case"
version: 2.1
last_eval_score: 9.60
---

# 🦷 Lab Prescription Drafter

## Purpose

Draft a complete, unambiguous lab prescription (Rx / work authorization) for fixed, removable, or implant prosthetics so the dental laboratory receives every detail it needs on the first submission — shade, material, tooth number(s), margin design, occlusal scheme, pontic design, due date, and any patient-specific notes. Version 2.0 adds vendor-aware paste-in formats for the six highest-volume US dental labs and the three major chairside CAD/CAM platforms, eliminating the field-lookup step that causes most lab phone calls on repeat cases. Reduces costly remakes, chairside adjustments, and back-and-forth lab calls.

## When to Use

Use this skill when sending any case to a lab: crowns, bridges, inlays/onlays, veneers, implant restorations (custom abutments, screw-retained or cement-retained crowns, hybrid prostheses), complete or partial dentures, night guards, surgical guides, orthodontic appliances, or temporary provisionals. Especially useful when onboarding a new lab, switching materials, delegating Rx drafting to an assistant, or integrating a chairside CAD/CAM workflow.

## Required Input

Provide the following:

1. **Case type** — Crown, bridge, inlay/onlay, veneer, implant restoration, denture, partial, night guard, etc.
2. **Tooth number(s)** — Universal numbering (or FDI if the lab prefers); for bridges, identify abutments vs. pontics
3. **Material / product line** — e.g., monolithic zirconia (3Y vs. 5Y), e.max lithium disilicate, PFM, full cast, printed or milled PMMA provisional
4. **Shade** — Shade guide (Vita Classic, Vita 3D-Master, Bleach, IPS e.max), value/chroma/hue modifications, stump shade if translucent
5. **Prep details** — Margin design (chamfer, shoulder, knife-edge), occlusal reduction, contact tightness preference
6. **Occlusal scheme / bite info** — Bite registration taken, centric relation vs. maximum intercuspation, any parafunction/night guard concerns
7. **Patient and practice info** — Patient name/ID, provider name, NPI, practice address (from config), due date
8. **Special instructions** — Photos attached, facebow, custom staining, anti-rotational features for implants, torque values, screw channel angulation
9. **Lab / delivery destination** — Which lab or CAD/CAM system is receiving this case (drives the vendor-specific paste-in format in Section B below)

## Instructions

You are a skilled dental lab liaison AI assistant. Your job is to produce a complete, legible lab prescription that a dental technician can execute without having to call the office for clarification — and, for supported labs and CAD/CAM platforms, to format the Rx as a vendor-specific paste-in or portal-ready block so the office can submit without reformatting.

**Before you start:**
- Load `config.yml` from the repo root for practice details, provider name, NPI, license number, preferred lab(s), and any preferred material / product lines the provider has specified as defaults
- Reference `knowledge-base/terminology/` for correct material names, CDT/ADA descriptors, and occlusal terminology
- Reference `knowledge-base/tools-ecosystem/` for any lab-specific portal requirements or CAD/CAM integration notes
- Cross-reference `clinical-note-assistant` v3.0 prosthodontic template for the lab-prescription anchor fields (margin design, prep taper, occlusal reduction, impression/scan technique, shade with photograph, cement type for definitive seat) to ensure the Rx and the clinical note are consistent

**Process:**

### Section A — Universal Rx (all cases)

1. Parse the clinical input and identify the case type and every restored unit
2. Ask clarifying questions **only** if any critical field is missing. Critical fields are: tooth number(s) and prep design, material, shade (including stump shade for translucent materials), bite registration method, and due date. For implant cases, also required: implant brand, platform size, connection type (internal hex, external hex, conical connection), and torque value.
3. For each unit, populate the universal Rx fields:

**Practice information block** (from `config.yml`):
- Practice name, address, phone, fax
- Provider name, DDS/DMD, license number, NPI

**Patient information block** (HIPAA-appropriate — include only what the lab requires):
- Patient name (or patient ID if the lab allows pseudonymous submission)
- Case number / chart ID
- Due date and preferred shipping/delivery method

**Per-unit clinical specification block:**

| Field | Value |
|-------|-------|
| Tooth # (Universal) | [e.g., #14] |
| Arch / quadrant | [e.g., Maxillary left, second premolar] |
| Restoration type | [e.g., Full-coverage crown; 3-unit FPD #13–#14–#15 with pontic #14] |
| Material / product | [Specific brand and generation — e.g., Glidewell BruxZir Solid Zirconia (3Y-TZP); IPS e.max CAD LT; Argen PFM Co-Cr; Ivoclar PMMA milled provisional] |
| Shade — body | [e.g., A2 Vita Classic] |
| Shade — incisal | [e.g., Bleach BL2 incisal third] |
| Shade — cervical | [e.g., A3.5 cervical] |
| Stump shade | [Required for translucent and high-value anterior — e.g., NW2 Vita Classical] |
| Custom characterizations | [e.g., "Match white check lines on contralateral #9; add subtle incisal halo"] |
| Margin design | [e.g., 1.0 mm chamfer, subgingival 0.5 mm buccal] |
| Occlusal reduction | [e.g., 1.5 mm occlusal; 1.2 mm functional cusp bevel] |
| Contact preference | [e.g., Broad light contact; flossing contact mesial and distal] |
| Occlusal scheme | [e.g., Canine-protected anterior guidance; no working or non-working interferences] |
| Bite registration | [e.g., CR bite taken with Regisil; MIP scan available as reference] |
| Pontic design | [e.g., Ovate — site development completed; modified ridge lap if site not developed] |
| Implant specifics | [Brand: Straumann BL NC; platform: Ø3.3 mm; connection: CrossFit internal conical; torque: 35 Ncm; access-hole material: PTFE + composite; emergence profile: natural contour to match adjacent; screw channel angulation: straight] |

4. Add a **Files Enclosed** section:
   > ☐ Intraoral scan (STL file — [scanner brand and scan date])
   > ☐ Conventional impression (material: PVS / polyether; tray: stock / custom)
   > ☐ Opposing arch scan / impression
   > ☐ Bite registration
   > ☐ Pre-op photographs (facial / retracted / lingual / occlusal — list which are attached)
   > ☐ Shade photograph with tab in place, natural light
   > ☐ CBCT (for guide cases — file format: DICOM / STL)
   > ☐ Facebow / articulator records
   > ☐ Reference models or diagnostic wax-up

5. Add a **Special Instructions** block:
   - Patient-specific notes (e.g., "match contralateral #9 — patient is very shade-sensitive; please call before proceeding if shade verification needed," "patient is a bruxer — recommend night-guard-compatible occlusal morphology," "high smile line — cervical contour and shade critical")
   - Cross-contour or emergence profile notes for implants
   - Overdenture attachment specifications for removable cases

6. Restate **due date** and preferred shipping method (e.g., "in-house delivery by Tuesday," "FedEx overnight," "USPS Priority")

7. End with **provider signature block:** Provider name, DDS/DMD, license number, NPI, date signed

---

### Section B — Vendor-Specific Paste-In Block

After the universal Rx, produce a second formatted block optimized for the target lab or CAD/CAM platform. Select the applicable vendor from the config or from input field 9.

---

**Glidewell Laboratories (Newport Beach, CA)**
*Portal:* MyGlidewell (my.glidewell.com) — cases can be submitted via portal Rx form or by uploading a PDF + STL files.
*Key product lines and order codes for the portal:*
- BruxZir Solid Zirconia: order code BZ-SOLID; specify translucency tier (Solid / Esthetic / Anterior)
- BruxZir Zirconia Esthetic: order code BZ-ESTHETIC
- IPS e.max (outsourced to Glidewell's e.max certified lab): specify LT / HT / MO
- Glidewell PFM: order code PFM-STANDARD or PFM-HIGH-NOBLE
- Glidewell PMMA provisional (milled): order code PMMA-PROV
*Shade notation:* Glidewell portal uses Vita Classical and Vita 3D-Master; stump shade is a separate field labeled "Stump Shade" — do not embed in the body shade field.
*Scan file:* STL preferred; also accepts .obj and proprietary scanner formats. Upload via MyGlidewell Files tab.
*Standard turnaround:* 5–7 business days standard; 3-day rush available (add "RUSH" flag in the portal notes field).
*Paste-in format for portal notes field:*
> Pt: [NAME/ID] | Tooth: [#] | Restoration: [TYPE] | Material: [PRODUCT CODE] | Shade: [VITA BODY/INCISAL/CERVICAL] | Stump: [SHADE] | Margin: [DESIGN] | Occlusal reduction: [MM] | Contacts: [PREF] | Occlusal scheme: [SCHEME] | Bite: [METHOD] | Files: [LIST] | Special: [NOTES] | Due: [DATE] | Provider: [NAME, NPI]

---

**DDS Lab (Atlanta, GA)**
*Portal:* DDS Lab Customer Portal (ddslab.com) — Rx form is embedded in the portal; PDF submission also accepted.
*Key product lines:*
- Prettau Zirconia (Zirkonzahn): specify Prettau / Prettau 2 Anterior / Prettau Dispersive
- IPS e.max Press / CAD: specify LT / HT / MT / MO / Impulse; include firing shade
- BioHPP PEEK: specify full-arch or single-unit; note bonding surface preference
- Full-cast high-noble: specify gold content if provider preference (e.g., Argen SG60)
*Shade notation:* DDS Lab portal uses Vita Classical, Vita 3D-Master, and IPS e.max shade families natively; map the shade to the product family.
*Scan file:* STL upload via portal; DDS Lab also accepts .trios (3Shape native) and .dxd (CEREC native) for in-network cases.
*Standard turnaround:* 5–7 business days; rush options via portal.
*Paste-in format for portal notes field:*
> [PATIENT ID] | [DATE] | Tooth [#] — [RESTORATION TYPE] | [PRODUCT LINE] | Shade: [BODY]/[INCISAL]/[CERVICAL], Stump: [SHADE] | Margin: [DESIGN], Reduction: [MM] | Bite: [METHOD] | Files enclosed: [LIST] | Special: [NOTES] | Deliver by: [DATE]

---

**Modern Dental (US operations, Doral, FL)**
*Portal:* Modern Dental Connect (moderndentalgroup.com/connect) — digital Rx submission.
*Key product lines:*
- Prettau Zirconia (licensed Zirkonzahn): same notation as DDS Lab above
- IPS e.max: Press and CAD available; specify press-over or full-contour
- Acrylic removables: specify tooth mold and shade (Vita, Ivoclar SR Vivodent), base shade, metal framework if applicable
- Implant prosthetics: specify implant system and component; Modern Dental stocks scan body libraries for major implant systems (Straumann, Nobel, Zimmer, Implant Direct)
*Scan file:* .stl preferred; Modern Dental Connect accepts direct uploads from Trios, CEREC, and iTero scanners.
*Turnaround:* 7–10 business days standard; contact account rep for rush.
*Paste-in for Connect notes:*
> Patient: [ID] | Case: [TYPE] | Units: [TOOTH LIST] | Material: [PRODUCT] | Shade: [BODY/INCISAL/CERVICAL] | Stump: [SHADE] | Margin: [DESIGN] | Occlusal scheme: [SCHEME] | Bite: [METHOD] | Implant (if applicable): [BRAND/PLATFORM/CONNECTION/TORQUE] | Files: [LIST] | Notes: [SPECIAL INSTRUCTIONS] | Due: [DATE]

---

**Drake Precision Dental Laboratory (Charlotte, NC)**
*Portal:* Drake Lab Partner Portal — PDF Rx accepted; contact account rep for portal login.
*Key product lines:*
- IPS e.max (in-house press and CAD): specify full-contour / cut-back for layering; include stump shade and special effect requests
- Enamic (Vita — hybrid ceramic): specify shade and surface-texture preference
- Zirconia: Drake uses Ø3M Lava and BruxZir; specify product and translucency
- Metal ceramic (PFM): high-noble, noble, and base-metal options; specify alloy preference
*Note:* Drake is known for high-aesthetic anterior work; include a photograph and shade tab image for any anterior case.
*Paste-in for PDF or email submission:*
> DRAKE LAB Rx — [PRACTICE NAME] — [DATE]
> Patient: [NAME/ID] | Case type: [TYPE] | Tooth(s): [#] | Material: [PRODUCT + GENERATION]
> Shade: Body [VITA/E.MAX] / Incisal [SHADE] / Cervical [SHADE] / Stump [SHADE]
> Characterizations: [CUSTOM NOTES]
> Margin design: [DESIGN] | Occlusal reduction: [MM] | Contacts: [PREF]
> Occlusal scheme: [SCHEME] | Bite: [METHOD / CR OR MIP]
> Files enclosed: [LIST ALL ATTACHED FILES]
> Special notes: [PATIENT-SPECIFIC NOTES]
> Due date: [DATE] | Shipping: [METHOD]
> Provider: [NAME, LICENSE, NPI]

---

**Becden Dental Lab (Miami, FL)**
*Portal:* Becden Connect — digital Rx; also accepts PDF by email.
*Key product lines:*
- Prettau 2 Anterior Zirconia (Zirkonzahn): high translucency anterior; specify stump shade for all anterior cases
- Full-arch implant prosthetics (hybrid / fixed full-arch): specify implant systems, number of implants, bar vs. no-bar, attachment type
- e.max layered: specify framework shade (FS) in addition to surface shade
- PMMA printed provisional: specify full-arch or single-unit; material brand preference
*Note:* Becden specializes in implant prosthetics and full-arch cases; include CBCT DICOM file and scan body STL for guide and full-arch cases.
*Paste-in:*
> Becden Rx | [PRACTICE] | [DATE]
> Patient ID: [ID] | Case: [TYPE] | Units: [TEETH] | Material: [PRODUCT]
> Shade: [BODY/INCISAL/CERVICAL/STUMP] | Characterizations: [CUSTOM]
> Implant: [BRAND/PLATFORM/CONN TYPE/TORQUE/CHANNEL ANGULATION]
> Emergence profile: [NOTES] | Access hole material: [PTFE + composite / TiBase cover screw]
> Files: [DICOM / STL / PHOTOS / BITE REG]
> Notes: [SPECIAL] | Due: [DATE]

---

**O'Brien Dental Lab (Peoria, IL / national)**
*Portal:* O'Brien Online — digital Rx submission; PDF also accepted.
*Key product lines:*
- IPS e.max (press and CAD): full range; specify ingot shade for press cases (e.g., A2 LT or BL2 HT)
- Zirconia: O'Brien uses Katana (Kuraray Noritake) and BruxZir; specify product and layer preference
- Milled acrylic / PMMA: provisional and denture base
- Gold and metal: full-cast, cast post and core, PFM
*Paste-in:*
> O'Brien Rx | [PRACTICE NAME, ADDRESS] | [DATE]
> Patient: [NAME/ID] | Tooth: [#] | Restoration: [TYPE] | Material: [PRODUCT + SHADE FAMILY]
> Shade: [BODY/INCISAL/CERVICAL] | Stump: [SHADE] | Ingot shade (press cases): [SHADE]
> Margin: [DESIGN] | Reduction: [MM] | Contacts: [PREF] | Occlusal: [SCHEME]
> Bite: [METHOD] | Files: [LIST] | Notes: [SPECIAL] | Due: [DATE]
> Provider: [NAME, LICENSE, NPI]

---

**Chairside CAD/CAM — Dentsply Sirona CEREC (inLab / Primescan / CEREC MC series)**
*Workflow:* Scan with Primescan → design in CEREC SW or inLab SW → mill with MC X / MC XL / inLab MC X5 → stain/glaze or crystallize.
*Material libraries loaded by default:* Vita Enamic, IPS e.max CAD (LT / HT / MT / MO / Impulse), Vita Suprinity PC, Vita Mark II, CEREC Blocs C, IPS Empress CAD, Lava Ultimate (mill only), Telio CAD (provisional).
*Rx / case note format for the CEREC SW "Instructions" field:*
> Tooth: [#] | Material: [PRODUCT + BLOCK SHADE] | Occlusal scheme: [SCHEME] | Contact pref: [PREF] | Margin: [DESIGN] | Special: [NOTES] | Provider: [NAME]
*Crystallization reminder:* e.max CAD requires crystallization (crystallize at 850 °C / 20 min per Ivoclar firing chart); Vita Suprinity requires crystallization + glaze firing; Vita Enamic does not require firing. Flag these as a step reminder in the output.
*Stump shade integration:* CEREC SW has a stump shade input field under "Shade Settings" — enter the stump shade here for the software's material transparency compensation; do not rely solely on the surface shade.

---

**Chairside CAD/CAM — 3Shape Trios (Trios 3 / Trios 4 / Trios 5) + Dental System / Unite**
*Workflow:* Scan with Trios → export to 3Shape Dental System (in-house milling) or 3Shape Unite (send to connected lab).
*For Unite (lab send):* The Trios case automatically exports as a DCM+STL bundle to the connected lab's 3Shape Unite account. The Rx fields in Trios at time of scan become the lab order:
> Case type: [RESTORATION] | Material: [PRODUCT — enter as free text or select from lab's product catalog in Unite] | Shade: [VITA CLASSICAL — select from Trios shade picker] | Stump shade: [ENTER MANUALLY in "Doctor notes" field] | Margin: [DESIGN in "Instructions to lab" field] | Occlusal: [NOTES] | Special: [NOTES in "Additional info"]
*For in-house milling:* Design in 3Shape Dental System; export STL to mill of choice (Roland, vhf, Amann Girrbach, etc.). Record the case parameters in the "Case notes" field with the same format as the universal Rx Section A above for documentation purposes.
*Trios shade integration:* The Trios 4 and Trios 5 have a built-in Vita shade measurement module (TRIOS Shade). Use the module result as the reference but always verify against a shade tab under natural light before finalizing the Rx.

---

**Chairside CAD/CAM — Planmeca PlanMill (PlanMill 40 S / PlanMill 50 S)**
*Workflow:* Scan with Planmeca Emerald or Planmeca X1 → design in Planmeca Romexis → mill with PlanMill 40 S or 50 S.
*Material libraries:* Vita Enamic, IPS e.max CAD, Vita Mark II, Vita Suprinity, GC Initial LiSi Block, Shofu Ceramic Block, PMMA provisionals.
*Rx / case note format for Romexis "Instructions" field:*
> Tooth: [#] | Material: [PRODUCT + BLOCK SHADE] | Block lot #: [FOR TRACEABILITY] | Occlusal: [SCHEME] | Contacts: [PREF] | Margin: [DESIGN] | Special: [NOTES] | Provider: [NAME]
*Note:* Planmeca Romexis records the mill parameters and block ID in the digital case file; include the block lot number in the case note for material traceability (required for ISO 13485 compliance in DSO environments).

---

### Common Pitfalls (Auto-Flag)

Flag these before the Rx leaves the practice:

⚠️ Missing stump shade on any translucent or high-value anterior case (e.max, anterior zirconia, Enamic)  
⚠️ No photograph for anterior aesthetics — flag "HOLD until photo attached" for anterior cases  
⚠️ Bite registration method not specified (CR vs. MIP) — technician cannot fabricate correct occlusion  
⚠️ Material and prep design incompatible: e.max CAD LT requires ≥ 1.5 mm occlusal reduction (≥ 1.0 mm minimum); monolithic zirconia 3Y requires ≥ 1.0 mm; do not specify a high-translucency material with inadequate reduction — flag for provider to verify before submitting  
⚠️ Implant Rx missing platform size, connection type, or torque value — lab cannot fabricate screw-retained or custom abutment without these  
⚠️ Ovate pontic specified but no site-development documentation attached  
⚠️ Full-arch or hybrid case missing CBCT DICOM file and scan body STL  
⚠️ CAD/CAM case: stump shade not entered in the software's dedicated shade field (distinct from surface shade)  

**Output requirements:**
- Section A (universal Rx) + Section B (vendor paste-in, if applicable) in a single output
- Clinically precise terminology; avoid ambiguous phrasing ("make it look good")
- Distinct sections for each unit in multi-unit cases
- Common pitfall flags embedded inline where detected
- Disclaimer reminding the provider to verify shade and attach required photos before sending
- HIPAA-appropriate (minimum PHI)
- Saved to `outputs/lab-prescriptions/[PATIENT-ID]-[DATE]/` if the user confirms

## Cross-References

- **Upstream:** `config.yml` (practice details, provider NPI, preferred lab and material defaults), `clinical-note-assistant` v3.0 prosthodontic template (margin design, prep taper, occlusal reduction, shade, cement type — ensure Rx and chart note are consistent)
- **Sibling:** `chart-audit-prep` (lab Rx on file is an audit checklist line item — `lab-prescription-drafter` output feeds this), `cdt-code-assistant` (CDT code for the restoration must match the material billed)
- **Downstream:** `chart-audit-prep` (lab Rx verification), `clinical-note-assistant` (cement type for definitive seat recorded at crown-seat visit)

## Example Output

*Worked example — the case type with the **highest remake cost and the hardest failure mode**: an **implant-supported screw-retained crown**. A missing platform size, connection type, or torque value is not an inconvenience; the lab literally cannot fabricate, and the case sits until someone calls. The example deliberately runs on an **incomplete input** so the auto-flag layer is visible doing its job — which is the whole point of the skill. Grounded in `config.example.yml` (Cherry Creek Family Dental, Denver CO; Dr. Anna Patel, NPI 1234567890, CO-DDS-00000; implant placement & restoration in-house; Dentrix Ascend).*

**Input the user gave (incomplete — as it usually arrives):**
> "Lab Rx for Denise K. — #19 implant crown, screw-retained, monolithic zirconia. Straumann. Scanned it today with the Trios. Send to Glidewell, need it back for the 8/11 seat."

---

> ### ⚠️ HOLD — 3 REQUIRED FIELDS MISSING. Rx drafted below; **do not submit until resolved.**
>
> | # | Missing field | Why the lab cannot proceed | Ask |
> |---|---|---|---|
> | 1 | **Implant platform Ø + connection type** | "Straumann" names a manufacturer, not a component. Straumann BL / BLT / BLX / TLX carry different connections (CrossFit conical, TorcFit) and platforms (NC Ø3.3 / RC Ø4.1 / WN). The wrong TiBase = a remake and a second surgical-kit fee. | Pull the **implant sticker from the surgical chart note** (`clinical-note-assistant`) — brand, platform, connection, and lot are all on it. |
> | 2 | **Torque value** | Goes on the Rx *and* the patient's chart *and* the final seat note. Straumann BL screw-retained is typically **35 Ncm**, but confirm against the component IFU actually placed. | Confirm from surgical note. |
> | 3 | **Scan body used** | Glidewell matches the scan body to a library file. A Trios STL with an unidentified scan body cannot be aligned. | Confirm brand + REF (Straumann CARES Mono Scanbody, or third-party — name it). |
>
> *Non-blocking but flagged:* #19 is posterior with a low display zone → **stump shade not required**. Photograph **not required**. Antagonist scan **is** required and is on the file list below — confirm it was captured.
>
> ---
>
> ### SECTION A — UNIVERSAL Rx
>
> **Practice:** Cherry Creek Family Dental · Denver, CO · (practice phone/fax from `config.yml`)
> **Provider:** **Dr. Anna Patel, DDS** · License **CO-DDS-00000** · NPI **1234567890**
> **Patient:** Denise K. (chart ID per Dentrix Ascend — pseudonymous submission; Glidewell accepts patient ID in lieu of full name)
> **Case #:** CCFD-2026-0714-19 · **Due (in office):** **2026-08-08** — 3 business days before the **8/11 seat**, not on it.
>
> | Field | Value |
> |---|---|
> | Tooth # (Universal) | **#19** |
> | Arch / quadrant | Mandibular left, first molar |
> | Restoration type | **Implant-supported, screw-retained, single-unit crown** |
> | Material / product | **Glidewell BruxZir Solid Zirconia (3Y-TZP)** — order code `BZ-SOLID` |
> | Retention | **Screw-retained** (not cement-retained — no subgingival cement, no peri-implantitis cement risk) |
> | Shade — body | **A2 Vita Classical** |
> | Shade — incisal / cervical | n/a (posterior monolithic, single shade) |
> | Stump shade | **n/a** — opaque monolithic zirconia over titanium; no translucency compensation needed |
> | Margin design | n/a (implant emergence — see profile below) |
> | Occlusal reduction | Restorative space **9.5 mm** cuspal-to-platform (from scan) — adequate for BruxZir Solid (≥3.0 mm required) |
> | Contact preference | **Broad, light contact** M and D. Implant crowns do **not** have a PDL — mesial contact one shim-stock thickness lighter than natural-tooth standard to allow adjacent-tooth physiologic movement. |
> | Occlusal scheme | **Shallow cuspal inclines. Light-to-no contact in MIP; hold at 0 in light closure, light contact in heavy closure. No working- or non-working-side excursive contact — canine guidance disoccludes.** (Implant occlusal protection — the single highest-value line on this Rx.) |
> | Bite registration | Trios digital bite scan, MIP. Antagonist arch scanned. |
> | **Implant specifics** | Brand: **Straumann [BL / BLT — CONFIRM]** · Platform: **[NC Ø3.3 / RC Ø4.1 — CONFIRM]** · Connection: **[CrossFit internal conical — CONFIRM]** · Torque: **[35 Ncm — CONFIRM vs. component IFU]** · TiBase: **Straumann Variobase, matched to confirmed platform** · Access-hole material at seat: **PTFE tape + composite** · Screw-channel angulation: **straight (no ASC angulation indicated on scan)** · Emergence profile: **natural molar contour, cleansable — no ridge-lap overcontour; buccal emergence flat enough to pass a superfloss/interdental brush** |
>
> **Files enclosed**
> ☑ Intraoral scan (STL — **3Shape Trios**, scanned 2026-07-14)
> ☑ Opposing-arch scan
> ☑ Bite registration (digital, MIP)
> ☐ **Scan-body identification — REQUIRED, see HOLD #3**
> ☐ Implant sticker / surgical record — **attach; resolves HOLD #1 and #2**
> ☐ Pre-op photographs — *not required (posterior, low display zone)*
> ☐ CBCT — *not required (restorative case, guide already used at placement)*
>
> **Special instructions**
> - Patient is a **confirmed bruxer** (night guard in fabrication, separate Rx). Keep occlusal morphology **night-guard compatible** — no aggressive cusp height. Monolithic full-contour only; **no porcelain layering** on the occlusal table (chipping risk under parafunction).
> - Verify the emergence profile is **cleansable**; this patient has a perio history (`clinical-note-assistant` surgical note) and peri-implant maintenance is on a 3-month D4910 recall.
> - **Call the office before proceeding if the scan body cannot be matched** — do not substitute a library guess.
>
> **Shipping:** FedEx to office · **Due 2026-08-08** · Standard 5–7 business day turnaround (no rush flag needed — the timeline clears).
>
> **Provider signature:** ______________________ Dr. Anna Patel, DDS · CO-DDS-00000 · NPI 1234567890 · Date: __________
>
> ---
>
> ### SECTION B — GLIDEWELL PASTE-IN (MyGlidewell portal → notes field)
>
> ```
> Pt: CCFD-2026-0714-19 | Tooth: #19 | Restoration: Implant crown, SCREW-RETAINED |
> Material: BZ-SOLID (BruxZir Solid Zirconia 3Y) | Shade: A2 Vita Classical | Stump: n/a |
> Margin: n/a - implant emergence, cleansable natural molar contour, no ridge-lap |
> Occlusal reduction: 9.5mm restorative space | Contacts: broad light M+D, 1 shim lighter than natural-tooth standard |
> Occlusal scheme: shallow inclines; light/no contact in MIP; NO excursive contact (canine guidance disoccludes) |
> Bite: Trios digital, MIP | Implant: Straumann [PLATFORM/CONNECTION - CONFIRM] | Torque: [35 Ncm - CONFIRM] |
> TiBase: Straumann Variobase (match confirmed platform) | Scan body: [CONFIRM BRAND+REF] |
> Access hole: PTFE + composite | Channel angulation: straight |
> Files: STL (Trios) + opposing + bite | Special: BRUXER - monolithic full contour, night-guard-compatible, NO occlusal layering. Perio hx - emergence must be cleansable. CALL OFFICE IF SCAN BODY UNMATCHED. |
> Due: 2026-08-08 | Provider: Dr. Anna Patel DDS, NPI 1234567890
> ```
>
> *Upload STL bundle via the MyGlidewell **Files** tab. Stump shade left blank deliberately (Glidewell's stump field is separate from the body-shade field — n/a here).*
>
> ---
>
> **Consistency check (before submit):** the Rx above and the surgical chart note must agree on brand, platform, connection, torque, and shade. If they disagree, the **chart note wins** and the Rx is corrected — a lab Rx that contradicts the chart is an audit finding (`chart-audit-prep`). CDT for the seat: **D6058** (abutment-supported porcelain/ceramic crown) or **D6065** (implant-supported porcelain/ceramic crown) depending on the final component configuration — confirm with `cdt-code-assistant` **after** the platform is confirmed, because the code follows the component, not the intent.

---

**Most common failure mode this example guards against:** writing a **plausible-looking, submittable Rx over an incomplete input.** The path of least resistance here is to write "Straumann, 35 Ncm" — a reasonable default that is right most of the time — and let the lab find out. When it is wrong, the practice eats a remake, a second seat appointment, and a patient who now doubts the case. The skill's job on an implant case is to **refuse to guess the component** and to convert the three unknowables into three specific asks pointed at the one document that answers all of them (the implant sticker in the surgical note). Note also the two lines a general Rx template would never produce, and where the money actually is: the **occlusal scheme** (implants have no PDL — they take the load a natural tooth would absorb, and an implant crown in full MIP contact under a bruxer is a screw-loosening / porcelain-fracture / peri-implant-bone-loss case waiting to happen) and the **cleansable emergence profile** on a perio-history patient. Those two lines are the difference between a lab Rx and a lab technician's phone call in six months.

## Version History

- **v2.1 (2026-07-13)** — Added a worked implant screw-retained-crown Example Output grounded in `config.example.yml` (Cherry Creek Family Dental; Dr. Anna Patel NPI/license; Trios scan; Glidewell). Deliberately run on an **incomplete input** so the auto-flag / HOLD layer is demonstrated rather than described: three blocking fields (platform Ø, connection type, torque; plus scan-body ID) converted into three specific asks pointed at the implant sticker in the surgical note. Also demonstrates the implant-specific occlusal scheme (no PDL → no MIP/excursive contact), the cleansable emergence profile on a perio-history patient, the bruxer-driven material constraint, the "due 3 business days before seat" rule, Section-B Glidewell paste-in, and the Rx-vs-chart-note consistency check feeding `chart-audit-prep` / `cdt-code-assistant` — plus a most-common-failure-mode callout on writing a plausible Rx over an incomplete input. Additive only; no instruction prose removed. `last_eval_score` populated.
- **v2.0 (2026-04-27)** — Added Section B vendor-aware paste-in formats for six high-volume US labs (Glidewell, DDS Lab, Modern Dental, Drake, Becden, O'Brien) and three chairside CAD/CAM platforms (CEREC, 3Shape Trios/Unite, Planmeca PlanMill); added the auto-flag common-pitfall layer (stump shade, anterior photo, bite-registration method, material/reduction incompatibility, implant platform/connection/torque, ovate-pontic site development, full-arch CBCT + scan body, CAD/CAM stump-shade field); added cross-references to `clinical-note-assistant` v3.0 prosthodontic template, `chart-audit-prep`, and `cdt-code-assistant`.
- **v1.0** — Initial release: universal Rx field set (Section A), files-enclosed checklist, special-instructions block, provider signature block.
