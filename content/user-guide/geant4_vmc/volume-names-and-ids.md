+++
title = "Volume Names and IDs"
menuTitle = "Volume Names and IDs"
chapter = false
hidden = false
showhidden = true
weight = 2
+++

The behavior described here is available in the **Geant4 VMC development version**, following the integration of "Unify VMC volume IDs across Geant4 volume representations".

## Source volumes and Geant4 representations

A source volume is a volume identified by its name in the VMC geometry. One source volume can have several Geant4 logical-volume representations, for example when reflection creates an additional logical volume or when `Gsposp` creates variants with different shape parameters.

Geant4 VMC assigns **one VMC volume ID to all representations of the same source volume**. This applies both to scoring through `TVirtualMCApplication::Stepping()` and to scoring through user sensitive detectors. A VMC volume ID identifies the source volume; copy numbers and geometry paths describe its placements.

Use `VolId(sourceName)` to obtain the source-volume ID and compare it with the ID returned by `CurrentVolID(copyNo)` during stepping. For example, after geometry initialization:

```cpp
Int_t absorberId = mc->VolId("ABSO");
```

During stepping, the same comparison covers the ordinary and reflected representations of `ABSO`:

```cpp
Int_t copyNo;
Int_t currentId = mc->CurrentVolID(copyNo);
if (currentId == absorberId) {
  // Score a step in the absorber.
}
```

Here `mc` is the application's `TVirtualMC` pointer. There is no need to look up a separate `ABSO_refl` ID. The E03a and E03b examples use source-volume IDs for absorber and gap scoring.

## Volume names

Geant4 VMC resolves reflected logical volumes through the relationships recorded by `G4ReflectionFactory`. It then applies the existing G3toG4 naming convention for `Gsposp` variants when G3toG4 name normalization is enabled. It does not identify reflections by simply removing `_refl` from a string: a source name that naturally contains `_refl` is preserved.

`VolName(id)` and logical-volume name queries such as `CurrentVolName()`, `CurrentVolOffName()` and `VolDaughterName()` use the source name for these representations. Assembly-level names remain available through ancestor queries. Physical-volume paths and assembly placement aliases retain their placement meaning.

Source names must identify source volumes uniquely. Distinct native Geant4 logical volumes with the same source name are grouped by the VMC name-based interface. The G3toG4 separator remains a reserved naming convention; arbitrary converter suffixes are not normalized.

## Sensitive detectors and volume selection

Register a user sensitive detector under the source name. Its registration and sensitive-volume selection then cover all representations of that source, including reflected volumes and recognized `Gsposp` variants.

One source volume has at most one registered user sensitive detector, but the same detector object can serve several source volumes. For example, E03b registers one detector for both `ABSO` and `GAPX`; these volumes retain distinct VMC IDs. A detector object's internal ID must not be used as a substitute for a VMC volume ID.

See [Sensitive Detectors and Volumes](/user-guide/geant4_vmc/sensitive-volumes) for registration and selection instructions.

## Operations covering all representations

The following Geant4 VMC operations use source names to cover matching logical-volume representations:

| Operation | Behavior |
|---|---|
| Optical skin surface assignment (`SetSkinSurface`) | Attaches the skin surface to every matching representation. |
| Name-based tracking-medium assignment | Assigns the medium to every matching representation. |
| ROOT local magnetic fields | Creates a field for every matching logical volume in the supported geometry modes. Field parameters are indexed by the actual Geant4 logical-volume name; field evaluation remains in world coordinates. |
| Geant4 VMC visualization volume selection | Recognizes source names as well as actual Geant4 names. |
| Volume-limit diagnostics | Prints the limits for every matching representation. |

See also [Magnetic Field](/user-guide/geant4_vmc/magnetic-field) and [Visualization](/user-guide/geant4_vmc/visualization).

## Representative and placement-specific queries

Sharing a VMC ID does not imply that all representations have identical shapes, daughter layouts or regions.

- `NofVolDaughters`, `VolDaughterName`, `VolDaughterCopyNo`, `GetMaterial`, `GetMedium` and `DumpRegion` still use a single representative. They do not combine the daughters or properties of all representations.
- `GetShape` and `GetTransformation` remain path-based queries for a particular placement. In particular, `Gsposp` variants can have different shapes.
- `SetBorderSurface` selects two physical placements. It does not create surfaces between every pair of matching representations. A global name and copy number can be ambiguous when repeated under different mothers.

Use the appropriate placement path when a query requires a particular shape or transformation.

## Compatibility and Geant4 service access

Applications should obtain IDs through the VMC interface rather than calculate them from Geant4 instance IDs, sensitive-detector IDs or generated names. Generated representations now share the source's ID, so their numeric IDs can differ from those in earlier versions. Numeric IDs should not be assumed stable across geometry changes or scoring configurations.

For applications using the Geant4 VMC service classes directly:

- `TG4SDServices::GetLogicalVolumes(id)` returns all mapped representations. The existing `GetLogicalVolume(id)` returns the first mapped representative.
- `TG4GeometryServices::FindLogicalVolumes(name)` returns all source-name matches. If none exist, it falls back to exact Geant4 logical-volume names. The singular `FindLogicalVolume(name)` prefers an unreflected representative.
- `GetConstituentVolumeName(lv)` preserves the exact constituent Geant4 name; `UserVolumeName(lv)` additionally applies the supported G3toG4 normalization.
- The third `MapVolume` argument is retained for source compatibility, but reverse mappings are always populated.

The volume maps and reflection relationships are prepared on the master for workers to read. This follows the normal geometry-construction lifecycle and does not support concurrent geometry mutation. The service class layouts changed, so rebuild Geant4 VMC and dependent code when updating to this implementation.
