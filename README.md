# BLoader (Java)

Java Swing + Java Agent project that demonstrates runtime bytecode instrumentation using ASM APIs provided by the JDK internal module namespace.

## Disclaimer

Use this code only in environments you own or are explicitly authorized to test.
Do not use it to bypass licensing, authentication, or legal restrictions.

## Features

- Swing desktop UI (`KeygenForm`) as the main entry point.
- Java agent (`Loader`) with `premain` instrumentation hook.
- Bytecode transformation implemented with ASM tree API.
- Manifest configured for both `Main-Class` and `Premain-Class`.

## Requirements

- JDK 17+ recommended
- `javac` and `jar` available on your `PATH`

Check your environment:

```bash
java -version
javac -version
jar --version
```

## Build

Run from the repository root:

```bash
javac \
	--add-exports java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED \
	--add-exports java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED \
	com/burpsuite/burploaderkeygen/*.java

jar cfm BurpLoaderKeygen.jar META-INF/MANIFEST.MF com
```

## Complete Rebuild (Clean + Build)

Use this to remove all compiled `.class` files and existing `.jar` files, then rebuild from scratch:

```bash
find com -type f -name '*.class' -delete && \
rm -f ./*.jar && \
javac \
	--add-exports java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED \
	--add-exports java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED \
	com/burpsuite/burploaderkeygen/*.java && \
jar cfm BurpLoaderKeygen.jar META-INF/MANIFEST.MF com
```

## Run

```bash
java -jar BurpLoaderKeygen.jar
```

Because the manifest defines:

- `Main-Class: com.burpsuite.burploaderkeygen.KeygenForm`
- `Premain-Class: com.burpsuite.burploaderkeygen.Loader`

the generated JAR can act as both the GUI app and a Java agent artifact.

## Project Layout

```text
.
|- com/
|  `- burpsuite/
|     `- burploaderkeygen/
|        |- Filter.java
|        |- Keygen.java
|        |- KeygenForm.java
|        `- Loader.java
|- META-INF/
|  `- MANIFEST.MF
|- LICENSE
`- README.md
```

## Troubleshooting

- `package jdk.internal.org.objectweb.asm ... is not visible`
	Build command is missing required `--add-exports` flags.
- UI starts but run command fails
	Verify a supported Java runtime is installed and executable.
- `ClassNotFoundException` when running JAR
	Rebuild the JAR from repository root and ensure `com/` is included.

## Development Notes

- Source package: `com.burpsuite.burploaderkeygen`
- UI/version string currently indicates `v1.1` in `KeygenForm.java`
- Manifest file: `META-INF/MANIFEST.MF`