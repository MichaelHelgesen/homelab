# Homelab Arkitektur

Oversikt over min homelab-infrastruktur basert på Proxmox.

**Sist oppdatert:** 6. februar 2025  
**Status:** Under bygging

---

## Hardware

### Mac Pro 6,1 (2013)
- **CPU:** 6-core Intel Xeon E5
- **RAM:** 64GB
- **Hypervisor:** Proxmox VE
- **Intern lagring:** 500GB SSD (Proxmox OS + VM storage)

### Ekstern lagring

#### Testing/læring (nåværende)

#### Produksjon (planlagt)
- Eksterne kabinetter
- Disker
- Formål: Permanent lagring av media

---

## Nettverksoversikt
```
Internet
    ↓
[Router/ISP]
    ↓
[Hjemmenettverk]
    ↓
Mac Pro (Proxmox)
    ├─ vmbr0 (Linux Bridge)
    │   └─ Ubuntu Server VM(er)
    │       └─ Docker containers
    │           ├─ Jellyfin (planlagt)
    │           ├─ Navidrome (planlagt)
    │           ├─ Immich (planlagt)
    │           └─ Nextcloud (planlagt)
    └─ WireGuard VPN (for ekstern tilgang (plalagt))
```

**Notater:**
- Nøyaktige IP-adresser fylles ut etter hvert
- Nettverkskonfigurasjon dokumenteres mer detaljert i `networking.md`

---

## Virtualisering

### Proxmox VE
- **Versjon:** 9.11
- **Kernel:** 6.17.2-1-pve
- **Web GUI:** 192.168.1.120:8006
- **Rolle:** Hypervisor - kjører alle VMs og containers

### Virtual Machines

#### Ubuntu Server "Ubuntu"
- **OS:** Ubuntu Server Ubuntu 24.04.3 LTS
- **CPU:** 2
- **RAM:** 8 GB
- **Disk:** 32GB
- **IP:** 192.168.1.89/24
- **Rolle:** Docker host for alle selvhostede tjenester

---

## Tjenester

### Media (planlagt)
- **Jellyfin** - Video/film streaming
  - Status: Planlagt

### Media (planlagt)
- **Navidrome** - Musikk streaming
- Status: Planlagt

### Personlig (planlagt)
- **Immich** - Foto management og backup
- **Nextcloud** - Fil-synkronisering og lagring

### Infrastruktur
- **Nginx Proxy Manager** - Reverse proxy for tjenester
  - Status: Planlagt
- **WireGuard VPN** - Sikker ekstern tilgang
  - Status: Planlagt

---

## Storage-struktur

### Proxmox host
```
/dev/sdX (intern disk) → Proxmox OS
/dev/sdY (USB - LaCie) → Passthrough til VM
```

### Ubuntu Server VM
```
~/docker/           # Docker Compose-filer og konfig
  ├── jellyfin/
  ├── navidrome/
  └── ...

~/media/            # Media-innhold (mountet fra ekstern disk)
  ├── movies/
  ├── tv/
  ├── music/
  └── photos/
```

**Notater:**
- Konfigurasjon og data holdes separert
- `~/docker` for tjeneste-oppsett
- `~/media` for faktisk innhold

---

## Tilgang og sikkerhet

### Intern tilgang (hjemmenettverk)
```
Laptop/Desktop → Router → Proxmox → VM → Docker → Tjeneste
```

### Ekstern tilgang
```
Mobil/Laptop (eksternt) → WireGuard VPN → Proxmox → VM → Tjeneste
```

**Sikkerhetstiltak:**
- WireGuard VPN for ekstern tilgang (ingen åpne porter)
- [Mer kommer etter hvert]

---

## Viktige beslutninger

### Hvorfor én Ubuntu VM for alle tjenester?
- Enklere å administrere for læring
- Alle tjenester i Docker = portabel konfigurasjon
- Kan separere til flere VMs senere hvis nødvendig

### Hvorfor USB-passthrough for lagring?
- Direct Attached Storage (DAS) - enklere enn NAS for oppstart
- Lærer grunnleggende mounting og disk-håndtering
- Kan utvide til NAS-løsning senere

### Hvorfor WireGuard fremfor port forwarding?
- Sikrere - ingen eksponerte tjenester direkte til internett
- Tilgang til hele hjemmenettverket, ikke bare én tjeneste
- Kjent utfordring: Routing-problemer dokumentert i nettverkslogg

---

## Neste steg

- [ ] Kommer

---

## Ressurser og dokumentasjon

- Proxmox dokumentasjon: https://pve.proxmox.com/wiki/
- Docker dokumentasjon: https://docs.docker.com/
- Mine bloggposter: michaelhelgesen.no
