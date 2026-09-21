# epsilonrt-labs

*[Version française](README_fr.md)*

This repository centralizes my notes explaining how to accomplish a number of
tasks and solve problems.

## Who this is for

These notes are written first and foremost for the **BTS CIEL** programme at
[Lycée Rouvière – Suzanne Lefort-Rouquette](https://www.lycee-rouviere.fr/index.php/superieur/b-t-s/systemes-numeriques-option-b)
in Toulon, France.

A *BTS* (Brevet de Technicien Supérieur) is a two-year, vocationally oriented
higher education diploma taught in French *lycées*. *CIEL* stands for
**Cybersécurité, Informatique et réseaux, Électronique** — Cybersecurity,
Computing and Networks, Electronics. The programme trains technicians who work
across embedded electronics, networking and system administration, which is
exactly the range of subjects these notes cover.

The programme's student project repositories live in the
[btsciel-toulon](https://github.com/btsciel-toulon) organization — mostly
private, as they are coursework under assessment.

That said, nothing here is locked to that context. Some documents assume a
specific environment — Windows 11 workstations in an Active Directory domain, a
personal network drive, lab-specific host naming — but the underlying technique
is general, and the environment-specific parts are always spelled out. If you
landed here from a search engine, you are welcome.

## Contents

Two kinds of material, deliberately kept apart.

### [`labs/`](labs/) — tutorials and lab assignments

Written to be read from beginning to end: they explain the principle before the
procedure, say why things are the way they are, and state what you should see
at every step.

| Lab | Language |
|---|---|
| [SSH keys for remote development](labs/ssh-key/) — generating a key pair, keeping it usable across shared workstations, declaring it on a Raspberry Pi | French |
| [Git and GitHub for team projects](labs/github/) — version control principles, creating a GitHub account, organising a project (team, repository, Kanban board), using Git from VS Code | French |

### [`how-to/`](how-to/) — short procedures

Written to be skimmed: one specific task, the context assumed known, and the
commands that settle it. Sorted by tool.

| Procedure | Language |
|---|---|
| [DFRobot FireBeetle 2 ESP32-C6 with PlatformIO](how-to/platformio/boards/firebeetle2/) — adding the board definition and building with the Arduino framework | French |

## A note on languages

Documents aimed at students are written in **French**, the teaching language.
More general notes may be in English. Each entry in the table above says which.

## Licence

**Documentation, text and illustrations** are released under
[Creative Commons Attribution 4.0 International](LICENSE) (CC BY 4.0). You may
share and adapt them, including commercially, as long as you give credit:

> Pascal JEAN (epsilonrt), lycée Rouvière, Toulon —
> https://github.com/btsciel-toulon/epsilonrt-labs

**Code snippets and configuration files** carry no such requirement: use them
freely, without attribution. Creative Commons licences are not designed for
software, and a board definition or a handful of shell commands should not come
with strings attached.

If something here saves you an afternoon, that is reason enough for it to exist.
