# CEx86 x86_64 Emulator

CEx86 is a 64-bit x86 emulator written purely in Java, inspired by fnuecke's Sedna RISC-V emulator.
It implements most x86 features required for general purpose computing and supervisory control and
external device interaction, meaning it should be able to boot Linux. It will also support serializing
and deserializing machine state, provided by fnuecke's Ceres serialization API.

fnuecke's projects can be found at: [Sedna](https://github.com/fnuecke/sedna) and [Ceres](https://github.com/fnuecke/ceres)

## Structure

The code layout matches that of fnuecke's Sedna, being relatively flat different parts of the
emulator living in their respective packages. Many features and utilities are provided by,
and should be accredited to, fnuecke. Here are some of CEx86's notable packages:

| Package                                                            | Description                                         |
|--------------------------------------------------------------------|-----------------------------------------------------|
| [akrauf.cex86.device](src/main/java/akrauf/cex86/device)           | Non-ISA device specific implementations.            |
| [akrauf.cex86.instruction](src/main/java/akrauf/cex86/instruction) | Instruction loader and decoder generator.           |
| [akrauf.cex86.io](src/main/java/akrauf/cex86/io)                   | IO port implementation and utilities                |
| [akrauf.cex86.x86](src/main/java/akrauf/cex86/x86)                 | x86_64 CPU and devices (PIC, x87, Cirrus-VGA, etc.) |

## Instructions and decoding

CEx86 uses runtime byte-code generation to create the decoder switch used the instruction interpreter. This makes it
very easy to add new instructions and to experiment with different switch layouts to improve performance. Also heavily
inspired by fnuecke's Sedna instruction generator.

The current set of supported x86 instructions is declared [in this file](src/main/resources/x86/instructions.txt).

Instruction implementations are defined in [the x86 CPU class](src/main/java/akrauf/cex86/x86/X86CPUTemplate.java).

## Endianness

The emulator aims to match IA64 specification, thusly presents itself as a bi-endian system to code
running inside it, though the default is little-endian.

## Maven

CEx86 can be included into a project via the Github Package Repository. See [the documentation][GithubPackagesGradle]
for more information on how to set that up. In short, you'll want to add your username and a public access token into
your `~/.gradle/gradle.properties` and use those variables in your repository declaration. Note that the public access
token will need `read:packages` permissions.

For example, using Gradle:

```groovy
repositories {
	maven {
		url = uri("https://maven.pkg.github.com/Flakwy/CEx86")
		credentials {
			username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
			password = project.findProperty("gpr.key") ?: System.getenv("TOKEN")
		}
	}
}

dependencies {
	implementation 'li.cil.ceres:sedna:2.0.0'
}
```

[GithubPackagesGradle]: https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry
