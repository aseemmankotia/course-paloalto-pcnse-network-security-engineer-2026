## Chapter 7 Cheat Sheet — Templates, Template Stacks & Device Groups

### Templates vs Template Stacks
| Item | Holds | Precedence Rule |
|---|---|---|
| Template | Network + device config (interfaces, zones, NTP, syslog, HA, etc.) | N/A — single config source |
| Template Stack | Multiple templates combined, assigned to a firewall/managed device | For overlapping settings, the template at the **TOP of the stack order wins** |

A firewall belongs to exactly **one** template stack. Reorder templates in the stack (not alphabetically, not by name specificity) to control which value is pushed.

### Device Group Hierarchy
```
Shared (root — visible to ALL device groups)
  └── Parent Device Group
        └── Child Device Group
```
- Objects/policies at **Shared** apply everywhere unless overridden.
- Override = create a **same-named object** at a lower (more specific) level; the more specific object wins for that device group and its descendants.
- No merging occurs on name collisions — it's always "closest ancestor wins."

### Rule Enforcement Order (full stack)
1. Shared **Pre-rules**
2. Device Group **Pre-rules** (top-down through hierarchy)
3. **Local** firewall rules
4. Device Group **Post-rules**
5. Shared **Post-rules**
6. **Default** (implicit) rules

Exam trap: Post-rules run **after** local rules, not before. Pre-rules always run **before** local rules.

### Commit to Panorama vs Push to Devices
| Action | Scope | Result |
|---|---|---|
| Commit to Panorama | Panorama itself | Saves candidate config into Panorama's running config only |
| Push to Devices | Managed firewalls / device groups | Sends the already-committed Panorama config to selected devices |
| Commit and Push | Both | Convenience action combining both steps in sequence |

Key point: **Committing does not push automatically.** Firewalls keep running their last-pushed config until an explicit push occurs.

### Log Collector Modes
| Mode | Function |
|---|---|
| Panorama (default) | Full management UI + optional local log storage |
| Log Collector | Dedicated log storage/forwarding only; no full management UI |
| Management Only | Device management only; no local log storage |

Use **Dedicated Log Collectors** (M-Series or Panorama VM in Log Collector mode, grouped into a Collector Group) when log volume or firewall count exceeds what the Panorama management server can handle locally — this decouples log storage scaling from management functions.

### Quick Memory Tricks
- **Stack = "top wins."**
- **Shared = broadest, easiest to override.**
- **Pre before local, post after local.**
- **Commit ≠ Push — always two clicks for full deployment.**
