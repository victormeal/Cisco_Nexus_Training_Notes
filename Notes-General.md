# General Notes

## Acronyms

- Top-of-Rack (ToR)
- End-of-Row (EoR)
- ISSU (in-service software upgrade)
- NAS (Network attached storage)
- vPC (virtual port-channel)


## NX OS Architecture

### Main Components
- Chasis
- Supervisor
- Line Cards
- Fabric Module
- System Controller

### Nexus 9k
- Nexus 9300 -> no modular
- Nexus 9500 -> modular

## Useful commands

- Check licenses:
`show license brief`

- Check flash
`dir bootflash://`

- install license
`install license bottflash://sup-active/file.lic`

- show features enables/disable
`show feature`
`show feature-set `
