# Welcome to LabsJDK CE 17!

The latest release is available at https://github.com/graalvm/labs-openjdk-17/releases/latest

This is a copy of https://github.com/graalvm/labs-openjdk-17 that
exists for the purpose of building a base JDK upon which GraalVM CE 17 is built.


## How to build jvmci-22.1-b06
1. Download `Java17` archive for your [platform](https://github.com/graalvm/labs-openjdk-17/releases/tag/jvmci-22.1-b06)
2. For Mac OS only: `sudo xattr -rd com.apple.quarantine path_to_archive`
3. Unzip `Java17` archive
4. Set `JAVA_HOME` to unzipped `Java17`
5. checkout to branch `release/jvmci/22.1`
6. `$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer`
6. `$ sh buildJdk.sh`

* Artifacts for **iOS**:
    * `build/labsjdk/images/graal-builder-jdk` -> `JAVA_HOME` to build GraalVM from sources
    * `xcode/jdk-arm64-ios-d.a`
    * `xcode/jdk-arm64-ios-r.a`
    * `xcode/jdk-x86-64-ios-simulator-d.a`
    * `xcode/jdk-x86-64-ios-simulator-r.a`

#### A labsjdk binary can be built as follows:
```
# Find latest jvmci-* tag in current branch.
JVMCI_VERSION=$(git log --decorate | grep -E 'tag: jvmci-\d+\.\d+-b\d+' | sed 's/.*(\(tag: .*\))/\1/g' | tr ',' '\n' | grep 'tag:' | sed 's/.*tag: \(jvmci-[^,)]*\).*/\1/g' | sort -nr | head -1)

# Configure and build
sh configure --with-conf-name=labsjdk \
    --with-version-opt=$JVMCI_VERSION \
    --with-version-pre= \
    '--with-vendor-name=GraalVM Community' \
    --with-vendor-url=https://www.graalvm.org/ \
    --with-vendor-bug-url=https://github.com/oracle/graal/issues \
    --with-vendor-vm-bug-url=https://github.com/oracle/graal/issues
make CONF_NAME=labsjdk graal-builder-image
```
This will produce a labsjdk binary under `build/labsjdk/images/graal-builder-jdk`.

You can verify the labsjdk built successfully by checking the version reported by the `java` launcher:
```
./build/labsjdk/images/graal-builder-jdk/bin/java --version
```

The upstream JDK README is [here](https://github.com/openjdk/jdk17u/blob/master/README.md).
