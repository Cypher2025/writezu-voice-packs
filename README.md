# Writezu voice packs

Prebuilt speech engine binaries for [Writezu](https://github.com/Cypher2025/Writezu). This
repository holds no source code — only the compiled packs Writezu downloads, and the catalogue that
describes them.

## Why these are a separate download

Writezu's installer is 26 MB. The speech engine is several hundred megabytes, and the model weights
are a further download again, so the engine is fetched on request rather than bundled. Most people
never open the voice features, and they should not pay for them.

Writezu reads `packs.json`, picks the pack matching the machine it is running on, verifies the
download against the checksum recorded here, and unpacks it locally. Nothing is installed without
that checksum matching.

## The packs

A pack is identified by `os-arch-backend`. The triple is what decides whether a build can run, which
is why it is the key rather than "cpu" and "gpu".

| Pack | Download | Installed | Needs |
|---|---|---|---|
| `windows-x86_64-cpu` | 133 MB | 329 MB | any 64-bit Windows machine |
| `windows-x86_64-cuda` | 547 MB | 936 MB | an NVIDIA graphics card |
| `macos-aarch64-cpu` | 104 MB | 258 MB | an Apple Silicon Mac |

There is no CUDA pack for macOS and there will not be one. CTranslate2, which runs the inference,
builds against CPU and CUDA only — asking it for Metal or MPS is an error — so a Mac pack is a
processor pack and the GPU goes unused.

CUDA acceleration needs an **NVIDIA** card specifically. AMD, Intel and Apple graphics cannot use
it, and nor can integrated graphics. The processor pack does the same work more slowly: measured
against a real five-minute recording it produced 250 words where the graphics card produced 251.

## Licences

Each pack carries `THIRD-PARTY-LICENSES.txt` inside it, listing every open-source component it
redistributes together with its full licence text.

The CUDA pack additionally contains NVIDIA's cuBLAS, cuBLASLt and CUDA runtime libraries, which are
not open source. They are redistributed under the NVIDIA CUDA Toolkit EULA, and that pack's licence
file reproduces the attribution and conditions the agreement requires.

## Checksums

The `sha256` in `packs.json` is not decoration. Writezu hashes each archive as it downloads and
refuses to unpack anything that does not match, so replacing a published asset without updating the
catalogue will break installs for everyone.
