# GEDCOM Extended ASSO

This repository defines a GEDCOM 7 extension for enhanced association (ASSO) structures, providing time boundaries, confidence levels, specific relationship types, and privacy flags for associations between individuals.

## Overview

The GEDCOM 7 ASSO structure provides basic person-to-person associations but lacks several features needed by modern genealogy software, particularly:
- Time boundaries (when relationships started/ended)
- Confidence levels in relationship assertions
- Specific relationship types beyond generic roles
- Relationship-specific privacy controls

This extension adds these capabilities while maintaining full backward compatibility.

## Extension URI

```
https://github.com/glamberson/gedcom-extended-ASSO
```

## New Structures

| Tag | Description |
|-----|-------------|
| `_TYPE` | Specific relationship type (e.g., "Godparent", "Business Partnership") |
| `_PERIOD` | Container for time boundaries |
| `_START` | When the association began |
| `_END` | When the association ended |
| `_CONF` | Confidence level (0-5 scale) |
| `_PRIV` | Privacy flag (Y/N) |

## Usage Example

```gedcom
0 HEAD
1 SCHMA
2 TAG _TYPE https://github.com/glamberson/gedcom-extended-ASSO/v1/TYPE
2 TAG _PERIOD https://github.com/glamberson/gedcom-extended-ASSO/v1/PERIOD
2 TAG _START https://github.com/glamberson/gedcom-extended-ASSO/v1/START
2 TAG _END https://github.com/glamberson/gedcom-extended-ASSO/v1/END
2 TAG _CONF https://github.com/glamberson/gedcom-extended-ASSO/v1/CONF
2 TAG _PRIV https://github.com/glamberson/gedcom-extended-ASSO/v1/PRIV

0 @I1@ INDI
1 NAME John Smith
1 ASSO @I2@
2 ROLE Godfather
2 _TYPE Godparent
2 _PERIOD
3 _START 15 MAY 1850
3 _END 15 MAY 1868
2 _CONF 4
2 NOTE Spiritual guardian from baptism to majority
```

## Compatibility

This extension is designed to be fully backward compatible:
- Non-supporting applications will preserve standard ASSO structures
- Extension tags will be ignored but preserved on import/export
- No changes to existing GEDCOM 7 semantics

## Documentation

See the [specification](specification.md) for detailed documentation.

## Contributing

Issues and pull requests welcome. Please ensure any changes maintain backward compatibility.

## License

This specification is released under the [Creative Commons Attribution 4.0 International License](LICENSE).