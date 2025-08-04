# GEDCOM Extended ASSO Specification

Version 1.0

## Purpose

This extension enhances the GEDCOM 7 ASSO (association) structure to support:
1. Time-bounded relationships
2. Confidence levels in relationship assertions  
3. Specific relationship types
4. Relationship-specific privacy

## Extended Structure

```
n ASSO @<XREF:INDI>@               {1:M}    g7:ASSO
  +1 PHRASE <Text>                 {0:1}    g7:PHRASE
  +1 ROLE <Enum>                   {0:1}    g7:ROLE
     +2 PHRASE <Text>              {0:1}    g7:PHRASE
  +1 <<NOTE_STRUCTURE>>            {0:M}
  +1 <<SOURCE_CITATION>>           {0:M}
  +1 _TYPE <Text>                  {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_TYPE
  +1 _PERIOD                       {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_PERIOD
     +2 _START <DateValue>         {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_START
     +2 _END <DateValue>           {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_END
  +1 _CONF <Integer>               {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_CONF
  +1 _PRIV <Y|N>                   {0:1}    https://github.com/glamberson/gedcom-extended-ASSO/_PRIV
```

## Structure Definitions

### _TYPE (Association Type)

Specifies the type of association beyond the standard ROLE enumeration.

- **Payload**: Text (max 120 characters)
- **Cardinality**: [0:1]
- **Parent**: ASSO

Examples:
- "Godparent" (with ROLE "Godfather")
- "Business Partnership" (with ROLE "Business Partner")
- "Military Service" (with ROLE "Fellow Soldier")

### _PERIOD (Time Period Container)

Container for time boundaries of the association.

- **Payload**: None (container only)
- **Cardinality**: [0:1]
- **Parent**: ASSO
- **Children**: _START, _END

### _START (Period Start)

Date the association began.

- **Payload**: DateValue (GEDCOM 7 date format)
- **Cardinality**: [0:1]
- **Parent**: _PERIOD

### _END (Period End)

Date the association ended.

- **Payload**: DateValue (GEDCOM 7 date format)
- **Cardinality**: [0:1]
- **Parent**: _PERIOD

### _CONF (Confidence Level)

Confidence in the association assertion.

- **Payload**: Integer (0-5)
- **Cardinality**: [0:1]
- **Parent**: ASSO

Scale:
- 0: Unreliable evidence or estimated data
- 1: Questionable reliability
- 2: Secondary evidence, probably reliable
- 3: Direct source, primary evidence
- 4: Well-supported conclusion
- 5: Very high confidence

### _PRIV (Privacy Flag)

Indicates if association should be treated as private.

- **Payload**: Y or N
- **Cardinality**: [0:1]
- **Parent**: ASSO
- **Default**: N (if omitted)

## Implementation Guidelines

### Symmetric Relationships

For symmetric relationships (e.g., friendship), create ASSO records on both individuals:

```gedcom
0 @I1@ INDI
1 ASSO @I2@
2 ROLE Friend
2 _TYPE Close Friend
2 _PERIOD
3 _START 1920

0 @I2@ INDI
1 ASSO @I1@
2 ROLE Friend
2 _TYPE Close Friend
2 _PERIOD
3 _START 1920
```

### Asymmetric Relationships

For asymmetric relationships, ROLE and dates may differ:

```gedcom
0 @I1@ INDI
1 ASSO @I2@
2 ROLE Employer
2 _TYPE Employment
2 _PERIOD
3 _START 1 JAN 1850
3 _END 31 DEC 1860

0 @I2@ INDI
1 ASSO @I1@
2 ROLE Employee
2 _TYPE Employment
2 _PERIOD
3 _START 1 JAN 1850
3 _END 15 JUN 1855
```

### Data Consistency

Applications should:
1. Warn about asymmetric data during editing
2. Offer to synchronize changes
3. Preserve intentional asymmetries
4. Validate date logic (START before END)

### Privacy Handling

When _PRIV is Y:
- Exclude from public reports
- Require authentication to view
- Mask in exports unless explicitly included

## Examples

### Business Partnership
```gedcom
1 ASSO @I2@
2 ROLE Business Partner
2 _TYPE General Partnership
2 _PERIOD
3 _START 1845
3 _END JUN 1867
2 _CONF 5
2 SOUR @S1@
```

### Godparent Relationship
```gedcom
1 ASSO @I2@
2 ROLE Godfather
2 _TYPE Godparent
2 _PERIOD
3 _START 15 MAY 1850
3 _END 15 MAY 1868
2 NOTE Until ward reached majority
```

### Uncertain Relationship
```gedcom
1 ASSO @I2@
2 ROLE Possible Sister
2 _TYPE Sibling
2 _CONF 1
2 NOTE Same household in census
```

### Private Association
```gedcom
1 ASSO @I2@
2 ROLE Informant
2 _TYPE Intelligence Asset
2 _PERIOD
3 _START 1962
3 _END 1975
2 _PRIV Y
```

## Version History

- 1.0 (2025-01-28): Initial release