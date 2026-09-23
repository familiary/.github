# Familiary

**Tools for finding out which code a binary is related to.**

Familiary is the home of MCRIT and the surrounding toolchain for binary code
similarity analysis — disassembly, shingling, MinHash-based comparison, reference
data, and the interfaces to work with it all.

The name is a coinage in the pattern of *library* and *apiary*: a *familiary* is
where the families are kept. Given an unknown sample, these tools tell you which
known code it is related to, how strongly, and which parts of it are just library or
compiler boilerplate.

> **The repositories are moving here.** Until each one is transferred, the links
> below point at its current home. GitHub redirects the old URLs after a transfer,
> so existing clones, links and `git+https://` installs keep working.

---

## The stack

The projects here build on each other. Working bottom-up:

| | Project | What it does |
|---|---|---|
| **Disassembly** | [smda](https://github.com/danielplohmann/smda) ↗ | Minimalist recursive disassembler built on Capstone, focused on accurate function entry point detection and CFG recovery — including in memory dumps and shellcode. Emits the SMDA reports everything else consumes. |
| **Block hashing** | [picblocks](https://github.com/danielplohmann/picblocks) | Position-independent hashing of basic blocks, used by MCRIT for unique-block matching. |
| **Similarity engine** | [mcrit](https://github.com/danielplohmann/mcrit) | The MinHash-based Code Relationship & Investigation Toolkit. A framework for rapidly implementing *shinglers* — methods that encode properties of disassembled functions — and using them for scalable 1:N similarity estimation. Ships a REST API, a worker queue, a Python client, and a CLI. |
| **Web frontend** | [mcritweb](https://github.com/fkie-cad/mcritweb) | Web UI for MCRIT: submitting samples, browsing families and functions, running and reviewing matching jobs. |
| **Deployment** | [docker-mcrit](https://github.com/danielplohmann/docker-mcrit) | Fully packaged docker-compose setup — MCRIT server and workers, MongoDB, MCRITweb, NGINX. The recommended way to get started, and the only one that guarantees compatible versions across components. |
| **Disassembler integration** | [mcrit-plugin](https://github.com/danielplohmann/mcrit-plugin) | IDA Pro plugin for querying an MCRIT server from inside your database: function and block matching, label synchronisation, dedicated result views. Installs as `mcrit-ida` via Hex-Rays' HCLI. |
| **Reference data** | [mcrit-data](https://github.com/danielplohmann/mcrit-data) | Ready-to-import reference code and symbols for statically linked library and compiler artefacts (MSVC, MinGW, Go, Nim, aPLib, and more), so that known library code can be identified and filtered out instead of drowning your results. |
| **Data preparation** | [lib2smda](https://github.com/danielplohmann/lib2smda) | Converts `.LIB` / `.OBJ` files into SMDA reports via IDA Pro, for building your own reference collections. |

↗ SMDA is a standalone disassembly library with a life of its own beyond code
similarity, so it stays at
[danielplohmann/smda](https://github.com/danielplohmann/smda). Everything here depends
on it.

---

## Getting started

The fastest path to a working instance:

```bash
git clone https://github.com/danielplohmann/docker-mcrit
cd docker-mcrit
docker compose up
```

This builds the MCRIT server and workers plus MCRITweb, pulls MongoDB and NGINX, and
brings everything up. From there, import a reference collection from
[mcrit-data](https://github.com/danielplohmann/mcrit-data) via *Data → Import* in
MCRITweb and start submitting samples.

For a library-only workflow, `pip install smda` and `pip install mcrit` also work
standalone.

---

## Background

These tools grew out of research on malware reverse engineering and analysis
automation, and they are closely tied to
[Malpedia](https://malpedia.caad.fkie.fraunhofer.de/) — a curated corpus of malware
families that serves as the reference data set the similarity work was built around.
MCRIT itself was developed at [Fraunhofer FKIE](https://github.com/fkie-cad).

Familiary exists to give these projects a single, maintainer-neutral home so they can
be developed and handed on independently of any one person or institution.

---

## Contributing

Issues and pull requests are welcome on the individual repositories. If you are unsure
where something belongs, open an issue on
[mcrit](https://github.com/danielplohmann/mcrit/issues) and we will route it.

For reference data in particular: if a compiler version or library you keep running
into is missing from mcrit-data, open an issue — ideally with the input data — and we
will see what we can do.

## License

Individual projects carry their own licenses; see each repository.
