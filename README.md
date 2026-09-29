# Q-Sys-Advanced-UCI-Controller
A plugin for the QSC Q-Sys platform to allow easy creation of advanced UCI's using multiple layers.

Intructions for use:

1. Fill the setup section with the details of the UCI & page you wish to control.
2. FIll in the layer names of the layers that you wish to control.
3. Optionally, give a group of controls a 'Radio Group' name if they are to operate as a radio group. Any name can be used, and you can create as many groups as required.
4. If any layers are to be 'sub layers', choose a parent layer. These layers will only be shown if the parent layer is visible. Nested parents are also possible.
5. Select the desired transitions for in and out. When a parent layer is changed, the transitions will be inherited.
6. If layers are part of a radio group, it will be possible to select the 'Reset Group' option. This will reset the group to its first member when a parent is enabled.
7. A 'Logical Parent' can be created that doesnt have an associated layer by selecting the 'Logical Parent' button. These are useful as a master control to hide a radio group.

---

## Fork: Layer Controller

This fork carries **Layer Controller**, a substantially rewritten descendant of
the Advanced UCI Controller, maintained by Digital Counterpoint and published
here under the same GPL-3.0 licence as its origin.

It began life as a drop-in replacement for the Advanced UCI Controller in
designs that were already running it, so it keeps that plugin's control names
(`enable`, `layer`, `logical parent`, `page`, `parent`, `radio group`,
`reset on parent`, `reset`, `status`, `transition in`, `transition out`, `uci`,
`visible`) and its transition and group-colour tables. A commissioned design
upgrades in place, without rewiring.

### What is different

| | Advanced UCI Controller | Layer Controller |
|---|---|---|
| Visibility | applied imperatively as controls change | computed declaratively (enable AND every ancestor visible), then diffed against what was last pushed, so `Uci.SetLayerVisibility` is called only on a real change |
| Parent loops | error detection | rejected at config time, with the offending row named |
| Radio groups | can be emptied by re-pressing the live member | optional **Sticky**: the group refuses to empty, re-asserts on a press of the live member, and falls back to its first permitted row |
| Access control | — | **PIN layer**: a permission column per PIN code, a Default column that is state as well as permission, and every enable masked against the live column |
| Refused writes | — | remembered, so a layer comes up the moment a column that permits it goes live |
| `reassert` | — | output pulse telling external scripts their requests were overwritten |
| Layout | fixed | configuration grid derived from the layer and PIN counts |
| Tests | — | 224 assertions in plain Lua, no Core or Designer needed |

### Licence

GPL-3.0, inherited. See the header of the `.qplug` for the full notice.

Original: Copyright (c) Callum Brieske / NSL NZ.
Modifications: Copyright (c) 2026 Digital Counterpoint.
