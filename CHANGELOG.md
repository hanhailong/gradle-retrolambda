#### 3.2.5
- Bumped default retrolambda version to `2.1.0`
- If the commandline parameters are over a certain limit, they will be written to files. This should
prevent failures on projects with large classpaths or when a huge number of incremental changes
happen.

#### 3.2.4
- Bumped default retrolambda version to `2.0.6`.

#### 3.2.3
- Fixed long builds times on large projects due to https://issues.gradle.org/browse/GRADLE-3283.
Note: Running the retrolambda task directly will no longer work, you must run the relevant java
compile task instead.

#### 3.2.2
- Fixed wrongly deleting lambda classes where the related class is a prefix of the one that actually
changed during incremental compilation. (thanks clemp6r!)

#### 3.2.1
- Fixed unit tests on android gradle plugin `1.3.0`.
- Bumped default retrolambda version to `2.0.5`.

#### 3.2.0
- Support for targeting java 5 with retrolambda.
- Don't depend on the android gradle plugin being on the classpath when using in pure java projects.
- Delay calculating classpath for retrolambda. This fixes missing aar libs added by the android 
gradle plugin.

#### 3.1.0
- Major refactoring of android plugin.
The method for modifying the javaCompile task is now *way* less hackey. In fact,
it is much closer to the original way that it was done. I had originally
abandoned this approach because it was breaking increment compilation is some
weird ways. However, I think I have solved it.

What this means for you: It is now less fickle of plugin application order and
way less likely to break other plugins.
- Properly split bootclasspath if it has multiple paths

#### 3.0.1
- Fixed occasional "Build exception: cannot call Task.setEnabled(boolean)" error.
- Fixed minor warning typo.
- Uploaded to the gradle plugin portal.

#### 3.0.0
A whole bunch of changes!
- Changed the default retrolambda to 2.0.0
- Added support for default methods, add `defaultMethods true` to the retrolambda block. Note: due
  to a current limitation in retrolamba, this will require all of your class files to be fed through
  retrolambda on each build. This may adversely affect build times.
- `incremental false` is no longer deprecated but has different semantics. Instead of being a hack
  around gradle-retrolambda breaking other plugins, it now only forces all of your class files to be
  run through retrolambda instead of only the changed ones.
- Added support for android unit tests, including lambdas in the tests themselves.
- No longer patch the android jar, modify the classpath instead. This should resolve issues with
  using gradle-retrolambda with more obscure android sdks, like google glass. This should also speed
  up a clean build since it doesn't have to do any zipping shenanigans.
- Ensure the gradle plugin is compiled with java 6 compatibility. This should allow you to run
  gradle with an older version of java if you don't want java 8 set as the default. This was always
  the intention, but was broken in the last build.
- More minor changes to how the java compile task is replaced, this should ensure better
  compatibility with other plugins. Note: these changes make the plugin application order more
  important. *Make sure you you apply this plugin last.*
- Removed 'retrolambda', now you can only apply the plugin with 'me.tatarka.retrolambda'.

#### 2.5.0
- A more robust fix for android-apt compatibility. Important: If you were experiencing issues with
android-apt previously and updated to this version, you must run `gradle build --rerun-tasks` once.
- Deprecate plugin name 'retrolambda' for 'me.tatarka.retrolambda' in preparation to publishing on
the gradle plugin portal.

#### 2.4.1
- Fixed compatibility with android-apt.
- Fixed typo in one of the thrown exceptions. (tomxor)
- Support groovy testing (ex. spock). (harningt)

#### 2.4.0

- Better incremental compile method that doesn't break lint and proguard (and
  probably other tasks). Because of this, `retrolambda.incremental` is deprecated
  and does nothing.
- Better handling of manually setting the retrolamba version with
  `retrolambConfig`.
- Don't use the retrolambda javaagent if using version `1.6.0+`.
- Set the default retrolambda version to `1.6.0`.

#### 2.3.1

- Fixed `retrolambda.incremental false` causing the retrolambda task not to run.

#### 2.3.0

- Add ability to set `retrolambda.incremental false` to disable incremental compilation, since it is
  incompatible with android lint/proguard.

#### 2.2.3

- Change dependency back to `localGroovy()`, `org.codehaus.groovy:groovy-all:2.3.3` was causing
  issues.

#### 2.2.2

- Support a `java.home` path that does not end in `/jre`, by using it as it is.
This is an issue on OSX which may have a different directory structure.

#### 2.2.1

- Ensure output directory is created even if the source set is missing files for the java plugin.
Otherwise, compiling the source set would error out.

#### 2.2.0

- Added way to add custom jvm arguments when running retrolambda.
- Disable `extractAnnotations` tasks since they are incompatible with java 8 sources.

#### 2.1.0

- Also check system property 'java.home' for the current java location. IDEs set this but not
  JAVA_HOME, so checking here first is more robust. (aphexcx)

#### 2.0.0
- Hooks into gradle's incremental compilcation support. This should mean faster build times and less
  inconsistencies when changing the build script without running `clean`. To fully take advantage of
  this you need to use retrolambda `1.4.0+` which is now the default.

#### 1.3.3
- Allow `retrolamba` plugin to be applied before or after `java` and `android`
  plugins

#### 1.3.2
- Fixed for android gradle plugin `0.10.+`

#### 1.3.1
- Removed `compile` property, which didn't work anyway. Use `retrolambdaConfig`
  instead.
- Minor error message improvement.

#### 1.3.0
- Support android instrument tests.

#### 1.2.0
- Support android-library projects.

#### 1.1.1
- Fixed not correctly finding java 8 executable when running from java 6 or 7 on
  windows. (Mart-Bogdan)

#### 1.1
- Fixed bug where java unit tests were not being run through retrolambda
- Allow gradle to be called with java 6 or 7, i.e. Java 8 no longer has to be
  your default java.
- Thank you Mart-Bogdan for starting these fixes.
