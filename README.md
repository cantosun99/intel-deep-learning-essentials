# intel-deep-learning-essentials

Intel® Deep Learning Essentials + oneDNN, packaged for Arch Linux.

This is a dependency package for [`llama.cpp-sycl`](https://github.com/cantosun99/llama.cpp-sycl). It installs only the oneAPI components actually needed to run llama.cpp with SYCL acceleration, the compiler, MKL, oneDNN, CCL, and the DPC++ library, instead of the full bloated `intel-oneapi-toolkit` package.

Installs to `/opt/intel/oneapi/` like in the official docs.

## Why this exists

The AUR has the full `intel-oneapi-toolkit` with all the other compilers, libraries, analysis-, debug tools and add-ons which is gigabytes of stuff you simply don't need. If all you want is SYCL acceleration for llama.cpp, you only need a small subset of it. This package ships exactly that and nothing else for close to *10 GB less than the full kit*.
It also bundles oneDNN, which is available on the arch repos already, but it is essential to download the matching package to the Intel Deep Learning Essentials to avoid issues in the /opt/intel/oneapi/ folder, which is why I decided to bundle it here instead of adding it as a dependency.

## What's included

| Component | Version | Size |
|---|---|---|
| [Intel® Deep Learning Essentials](https://www.intel.com/content/www/us/en/developer/tools/oneapi/oneapi-toolkit-download.html?packages=dl-essentials&dl-essentials-os=linux&dl-lin=offline) (compiler, MKL, DPC++, CCL, TBB) | 2026.1.0 | 1.37GB |
| [Intel® oneAPI Deep Neural Network Library (oneDNN)](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onednn-download.html?operatingsystem=linux&distribution-linux=offline) | 2026.0.1 | 316MB |

## Install

### Via AUR helper

```bash
yay -S intel-deep-learning-essentials
# or
paru -S intel-deep-learning-essentials
```

### Manual

```bash
git clone https://github.com/cantosun99/intel-deep-learning-essentials.git
cd intel-deep-learning-essentials
makepkg -si
```

The installers are downloaded directly from Intel and are large. This is expected.

## Notes

- If you encounter issues while installing, 99% of the time it's because Intel's installers are complaining about there already being files in /opt/intel or in your /home directory. Try to find them and remove them.
- The package must run outside of fakeroot so the Intel installer respects `--install-dir`. This is intentional and handled in the PKGBUILD.
- A linker config is written to `/etc/ld.so.conf.d/` so the compiler libs are found system-wide without manual environment setup.
- Installer logs are removed from the final package to avoid `$srcdir`/`$pkgdir` path leakage.

## License

PKGBUILD and packaging: MIT. Intel oneAPI components are subject to Intel's license terms.
