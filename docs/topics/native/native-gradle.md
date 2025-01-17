[//]: # (title: Kotlin/Native 入门——使用 Gradle)

[Gradle](https://gradle.org) 是一个在 Java、Android 与其他生态系统项目中非常常用的构建系统。
它是 Kotlin/Native 与 Multiplatform 的默认构建系统。

尽管包括 [IntelliJ IDEA](https://www.jetbrains.com/idea) 在内的大多数 IDE 都可以生成相应的 Gradle 文件， 
但还是建议看一下如何手动创建此 Gradle 文件，以更好地了解事物的本质。 
如果想使用 IDE，请参阅[使用 IntelliJ IDEA 的 Hello Kotlin/Native](native-get-started.md)。 

Gradle 支持两种语言的构建脚本：

- Groovy 脚本为 `build.gradle` 文件
- Kotlin 脚本为 `build.gradle.kts` 文件

Groovy 语言是 Gradle 最早支持的脚本语言，它利用了该语言的动态类型与运行时特性。
也可以在 Gradle 脚本中使用 Kotlin。
作为一种静态类型的语言，当涉及到编译与错误检测时，可以在 IDE 中更好地发挥作用。

两种都可以使用，示例将展示两种语言的语法。

## Create project files 

First, create a project directory. Inside it, create `build.gradle` or `build.gradle.kts` 
Gradle build file with the following contents:

<tabs group="build-script">
<tab title="Kotlin" group-key="kotlin">

```kotlin
plugins {
    kotlin("multiplatform") version "%kotlinVersion%"
}

repositories {
    mavenCentral()
}

kotlin {
  macosX64("native") { // on macOS
  // linuxX64("native") // on Linux
  // mingwX64("native") // on Windows
    binaries {
      executable()
    }
  }
}

tasks.withType<Wrapper> {
  gradleVersion = "%gradleVersion%"
  distributionType = Wrapper.DistributionType.BIN
}
```

</tab>
<tab title="Groovy" group-key="groovy">

```groovy
plugins {
    id 'org.jetbrains.kotlin.multiplatform' version '%kotlinVersion%'
}

repositories {
    mavenCentral()
}

kotlin {
  macosX64('native') { // on macOS
  // linuxX64('native') // on Linux
  // mingwX64('native') // on Windows
    binaries {
      executable()
    }
  }
}

wrapper {
  gradleVersion = '%gradleVersion%'
  distributionType = 'BIN'
}
```

</tab>
</tabs>

Next, create an empty `settings.gradle` or `settings.gradle.kts` file in the project folder.

取决于目标平台，不同的[函数](multiplatform-dsl-reference.md#目标)，
例如：`macosX64`、`mingwX64`、`linuxX64`、`iosX64`，用于创建 Kotlin 目标。
函数名称是其编译代码的平台。
这些函数可以选择将目标名称作为参数，在本例中为 `"native"`。
指定的 _目标名称_ 用于在项目中生成源路径和任务名称。

按照惯例，所有源代码都位于 `src/<目标名称>[Main|Test]/kotlin` 文件夹中，其中 `main` 用于源代码，而 `test` 用于测试。
`<目标名称>` 对应于构建文件中指定的目标平台（在本例中为 `native`）。

创建一个文件夹 `src/nativeMain/kotlin`，并在其中放置文件 `hello.kt`，其内容如下：

```kotlin
fun main() {
  println("Hello Kotlin/Native!")
}
```

## 构建项目

在项目根目录中，通过运行执行构建

`gradle nativeBinaries`

这将创建一个文件夹 `build/bin/native`，其中包含两个子文件夹 `debugExecutable` 与 `releaseExecutable` 以及相应的二进制文件。
默认情况下，二进制文件的名称与项目文件夹的名称相同。

## 在 IDE 中打开项目

任何支持 Gradle 的 IDE 都应允许在 IDE 中打开项目。 对于 [IntelliJ IDEA](https://www.jetbrains.com/idea)，
只需打开项目文件夹，会自动将其检测为 Kotlin/Native 项目。

## 下一步做什么？

Learn how to [write Gradle build scripts for real-life Kotlin/Native projects](multiplatform-dsl-reference.md).

