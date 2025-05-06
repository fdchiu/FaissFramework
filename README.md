**FAISS Framework for macOS & iOS**

## CLAIM: THIS IS NOT AN OFFICIAL RELEASE FROM ORIGINAL AUTHOR OF FAISS. IT NOT RELATED TO EITHER META OR FACEBOOK.

This repository provides a preconfigured framework build of [FAISS (Facebook AI Similarity Search)](https://github.com/facebookresearch/faiss) for native macOS and iOS development. It includes the necessary static libraries and headers packaged as an XCFramework for easy integration into Xcode projects.

---

## Table of Contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [Clone & Prepare](#clone--prepare)
* [Building the Framework](n##building-the-framework)

  * [macOS Build](#macos-build)
  * [iOS Build](#ios-build)
  * [Assemble XCFramework](#assemble-xcframework)
* [Integration into Xcode](#integration-into-xcode)
* [Usage Example](#usage-example)
* [Troubleshooting](#troubleshooting)
* [License](#license)

---

## Overview

FAISS is a library for efficient similarity search and clustering of dense vectors. This project automates the process of building FAISS for Apple platforms, producing an `FAISS.xcframework` containing:

* **macOS** (x86\_64 & Apple Silicon slices)
* **iOS device** (arm64)
* **iOS simulator** (x86\_64 & arm64)

You can drop the XCFramework into any Xcode project and link against the FAISS APIs without additional native build steps.

---

## Prerequisites

1. **Xcode 12+** with command-line tools installed
2. **CMake 3.18+** (`brew install cmake`)
3. **Ninja** (optional, but recommended for faster builds: `brew install ninja`)
4. **Python 3.8+** (for any helper scripts)

---

## Clone & Prepare

```bash
# Clone FAISS repository
git clone https://github.com/fdchiu/FaissFramework.git
cd FaissFramework

# (Optional) Checkout a stable tag
# git checkout v1.7.4

# Create an out-of-source build directory
```

---

## Using the Framework

Drop the framework in your Xcode project as a dependency. Use it the same way you would use faiss for.  See bellow "Integration into Xcode".

###

### Assemble XCFramework

If needed, you can convert the framework into an xcframework

```bash
xcodebuild -create-xcframework -framework 'FaissFramework.framework' -output 'FaissFramewor.xcframework'

```

After this command, you will have `FaissFramwork.xcframework` in your working directory.

---

## Integration into Xcode

1. Drag the `FaissFramework.xcframework` into your Xcode project’s **Frameworks** group.
2. In your target’s **General** settings, under **Frameworks, Libraries, and Embedded Content**, ensure FaissFramework.xcframework is set to **Embed & Sign**.
3. Add the FAISS include path:

   * **Build Settings** → **Header Search Paths** → add `$(PROJECT_DIR)/path/to/FaissFramework.xcframework/Headers` (recursive: NO)

---

## Usage Example

```cpp
#include <faiss/IndexFlat.h>
#include <vector>
#include <iostream>

int main() {
    // Create a flat (exhaustive) index for d-dimensional vectors
    int d = 128;
    faiss::IndexFlatL2 index(d);

    // Add some vectors\    
    std::vector<float> xb(10 * d);
    for (int i = 0; i < 10 * d; i++) xb[i] = drand48();
    index.add(10, xb.data());

    // Query
    int k = 5;
    std::vector<float> xq(d);
    for (int i = 0; i < d; i++) xq[i] = drand48();
    std::vector<faiss::idx_t> I(k);
    std::vector<float> D(k);

    index.search(1, xq.data(), k, D.data(), I.data());

    std::cout << "Nearest neighbors:" << std::endl;
    for (int i = 0; i < k; i++) {
        std::cout << I[i] << ": " << D[i] << std::endl;
    }

    return 0;
}
```

---

## Troubleshooting

* **Segmentation fault at runtime**: Ensure your target architectures match the XCFramework slices. Clean build folder and rebuild.
* **Undefined symbols for architecture**: Verify you added all slices (device + simulator) to the XCFramework.
* **Header not found**: Check **Header Search Paths** and framework embedding settings.

---

## License

FAISS is released under the BSD-licensed terms. See the [FAISS LICENSE](https://github.com/facebookresearch/faiss/blob/main/LICENSE) for details.

---

For consulting services or professional support integrating this framework into your business project, please get in touch.

**Enjoy efficient similarity search on Apple platforms!**
