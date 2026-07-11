# intel-deep-learning-essentials

Intel® Deep Learning Essentials + oneDNN, packaged for Arch Linux.

This is a dependency package for [`llama.cpp-sycl`](https://github.com/cantosun99/llama.cpp-sycl). It installs only the oneAPI components actually needed to run llama.cpp with SYCL acceleration, the compiler, MKL, oneDNN, CCL, and the DPC++ library, instead of the full bloated and/or outdated `intel-oneapi-toolkit`, `intel-oneapi-basekit-2025` or `intel-oneapi-hpckit` packages.

Installs to `/opt/intel/oneapi/` like in the official docs.

## If the install fails

If you encounter issues while installing this package, 99% of the time it's one of the following options:
- If you previously or currently have the `intel-oneapi-toolkit`, `intel-oneapi-basekit-2025` or `intel-oneapi-hpckit` packages installed, please uninstall them and verify that everything in /opt/intel is deleted. Most likely you don't even need everything that's included in the full Intel® oneAPI Toolkit for developers, that's why the Intel® Deep Learning Essentials for users exists. I suggest replacing the three packages with this one unless you are one of the few people that need the full kit.
- If you are updating this package and it fails, you have to delete `/home/yourusername/intel` and `/home/yourusername/.intel` and retry the update. Please verify just in case, but there usually is nothing important in there anyway. 

## Why this exists

The AUR/extra has the full `intel-oneapi-toolkit`, `intel-oneapi-basekit-2025` or `intel-oneapi-hpckit` packages with all the other analysis-, debug tools and add-ons which is gigabytes of stuff you simply don't need. The full Intel® oneAPI Toolkit is intended for developers, if all you want is SYCL acceleration for llama.cpp, you only need a small subset of it. That's why the Intel® Deep Learning Essentials exist. This package ships exactly that and nothing else for close to *10 GB less than the full kit*.
It also bundles oneDNN, which is available on the arch repos already, but it is essential to download the matching package to the Intel® Deep Learning Essentials to avoid issues in the /opt/intel/oneapi/ folder, which is why I decided to bundle it here instead of adding it as a dependency. OneDNN used to be part of the Intel® Deep Learning Essentials up until 2025.3.3, no idea why they even decided to ship it seperately with 2026.0.0.

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

- The package must run outside of fakeroot so the Intel installer respects `--install-dir`. This is intentional and handled in the PKGBUILD.
- A linker config is written to `/etc/ld.so.conf.d/` so the compiler libs are found system-wide without manual environment setup.
- Installer logs are removed from the final package to avoid `$srcdir`/`$pkgdir` path leakage.

## License

PKGBUILD and packaging: MIT. Intel oneAPI components are subject to Intel's license terms.
