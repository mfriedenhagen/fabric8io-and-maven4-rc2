# Read Me First

## General
* This is a sample project for an error condition with Maven 4 (RC2) and the fabric8io:docker-maven-plugin

## Requirements

* Install latest Maven 3 (3.9.9) and Maven 4 (4.0.0-RC) (I created aliases for them)
* Create an account at hub.docker.com (actually an internal Docker registry will work as well).
* Create a `server` entry in `~/.m2/settings.xml`:
```xml
    <server>
        <id>docker.io</id>
        <username>YOUR_USERNAME_AT_HUB_DOCKER_COM</username>
        <password>YOUR_PASSWORD_OR_TOKEN_AT_HUB_DOCKER_COM</password>
    </server>
```

* `mvn3 -B clean verify -Ddocker.autoPull=always` does work.
* `mvn4 -B clean verify -Ddocker.autoPull=always` gives the following stacktrace:
```
❯ mvn4 -V -B -e clean verify -Ddocker.autoPull=always
Apache Maven 4.0.0-rc-2 (273314404f85ec3c089e295d8b4e0cb18c287cf5)
Maven home: /home/user/lib/apache-maven-4
Java version: 21.0.5, vendor: Eclipse Adoptium, runtime: /Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents/Home
Default locale: de_DE, platform encoding: UTF-8
OS name: "mac os x", version: "15.1.1", arch: "aarch64", family: "mac"
[INFO] Error stacktraces are turned on.
...
[INFO] --- docker:0.45.1:build (docker-build) @ fabric8io-and-maven4-rc2 ---
[INFO] Copying files to /home/user/ghq/github.com/mfriedenhagen/fabric8io-and-maven4-rc2/target/docker/example.com/example/fabric8io-and-maven4-rc2/build/maven
[INFO] Building tar: /home/user/ghq/github.com/mfriedenhagen/fabric8io-and-maven4-rc2/target/docker/example.com/example/fabric8io-and-maven4-rc2/tmp/docker-build.tar
[INFO] DOCKER> [example.com/example/fabric8io-and-maven4-rc2:latest]: Created docker-build.tar in 90 milliseconds
[ERROR] DOCKER> Error looking security dispatcher [java.util.NoSuchElementException
      role: org.sonatype.plexus.components.sec.dispatcher.SecDispatcher
  roleHint: maven]
[INFO] --------------------------------------------------------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] --------------------------------------------------------------------------------------------------------------------------
[INFO] Total time:  6.163 s
[INFO] Finished at: 2024-12-18T17:54:21+01:00
[INFO] --------------------------------------------------------------------------------------------------------------------------
[ERROR] Failed to execute goal io.fabric8:docker-maven-plugin:0.45.1:build (docker-build) on project fabric8io-and-maven4-rc2: Error looking security dispatcher: java.util.NoSuchElementException
[ERROR]       role: org.sonatype.plexus.components.sec.dispatcher.SecDispatcher
[ERROR]   roleHint: maven
[ERROR] -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal io.fabric8:docker-maven-plugin:0.45.1:build (docker-build) on project fabric8io-and-maven4-rc2: Error looking security dispatcher
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2(MojoExecutor.java:346)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute(MojoExecutor.java:310)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:214)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:179)
    at org.apache.maven.lifecycle.internal.MojoExecutor$1.run(MojoExecutor.java:168)
    at org.apache.maven.plugin.DefaultMojosExecutionStrategy.execute(DefaultMojosExecutionStrategy.java:39)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:165)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:110)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:76)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:60)
    at org.apache.maven.lifecycle.internal.DefaultLifecycleStarter.execute(DefaultLifecycleStarter.java:123)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:311)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:225)
    at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:149)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.doExecute(MavenInvoker.java:470)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:95)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:85)
    at org.apache.maven.cling.invoker.LookupInvoker.doInvoke(LookupInvoker.java:145)
    at org.apache.maven.cling.invoker.LookupInvoker.invoke(LookupInvoker.java:116)
    at org.apache.maven.cling.ClingSupport.run(ClingSupport.java:64)
    at org.apache.maven.cling.MavenCling.main(MavenCling.java:51)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
    at java.lang.reflect.Method.invoke(Method.java:580)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:255)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:201)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:361)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:314)
Caused by: org.apache.maven.plugin.MojoExecutionException: Error looking security dispatcher
    at io.fabric8.maven.docker.util.AuthConfigFactory.decrypt(AuthConfigFactory.java:680)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfigFromServer(AuthConfigFactory.java:689)
    at io.fabric8.maven.docker.util.AuthConfigFactory.getAuthConfigFromSettings(AuthConfigFactory.java:497)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createStandardAuthConfig(AuthConfigFactory.java:231)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfig(AuthConfigFactory.java:120)
    at io.fabric8.maven.docker.service.RegistryService$RegistryConfig.createAuthConfig(RegistryService.java:267)
    at io.fabric8.maven.docker.service.RegistryService.createAuthConfig(RegistryService.java:225)
    at io.fabric8.maven.docker.service.RegistryService.pullImageWithPolicy(RegistryService.java:139)
    at io.fabric8.maven.docker.service.BuildService.autoPullBaseImage(BuildService.java:325)
    at io.fabric8.maven.docker.service.BuildService.buildImage(BuildService.java:66)
    at io.fabric8.maven.docker.BuildMojo.proceedWithDockerBuild(BuildMojo.java:110)
    at io.fabric8.maven.docker.BuildMojo.proceedWithBuildProcess(BuildMojo.java:91)
    at io.fabric8.maven.docker.BuildMojo.buildAndTag(BuildMojo.java:84)
    at io.fabric8.maven.docker.BuildMojo.processImageConfig(BuildMojo.java:163)
    at io.fabric8.maven.docker.BuildMojo.executeInternal(BuildMojo.java:73)
    at io.fabric8.maven.docker.AbstractDockerMojo.execute(AbstractDockerMojo.java:302)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:144)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2(MojoExecutor.java:339)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute(MojoExecutor.java:310)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:214)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:179)
    at org.apache.maven.lifecycle.internal.MojoExecutor$1.run(MojoExecutor.java:168)
    at org.apache.maven.plugin.DefaultMojosExecutionStrategy.execute(DefaultMojosExecutionStrategy.java:39)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:165)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:110)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:76)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:60)
    at org.apache.maven.lifecycle.internal.DefaultLifecycleStarter.execute(DefaultLifecycleStarter.java:123)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:311)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:225)
    at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:149)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.doExecute(MavenInvoker.java:470)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:95)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:85)
    at org.apache.maven.cling.invoker.LookupInvoker.doInvoke(LookupInvoker.java:145)
    at org.apache.maven.cling.invoker.LookupInvoker.invoke(LookupInvoker.java:116)
    at org.apache.maven.cling.ClingSupport.run(ClingSupport.java:64)
    at org.apache.maven.cling.MavenCling.main(MavenCling.java:51)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
    at java.lang.reflect.Method.invoke(Method.java:580)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:255)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:201)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:361)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:314)
Caused by: org.codehaus.plexus.component.repository.exception.ComponentLookupException: java.util.NoSuchElementException
      role: org.sonatype.plexus.components.sec.dispatcher.SecDispatcher
  roleHint: maven
    at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:269)
    at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:245)
    at io.fabric8.maven.docker.util.AuthConfigFactory.decrypt(AuthConfigFactory.java:674)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfigFromServer(AuthConfigFactory.java:689)
    at io.fabric8.maven.docker.util.AuthConfigFactory.getAuthConfigFromSettings(AuthConfigFactory.java:497)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createStandardAuthConfig(AuthConfigFactory.java:231)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfig(AuthConfigFactory.java:120)
    at io.fabric8.maven.docker.service.RegistryService$RegistryConfig.createAuthConfig(RegistryService.java:267)
    at io.fabric8.maven.docker.service.RegistryService.createAuthConfig(RegistryService.java:225)
    at io.fabric8.maven.docker.service.RegistryService.pullImageWithPolicy(RegistryService.java:139)
    at io.fabric8.maven.docker.service.BuildService.autoPullBaseImage(BuildService.java:325)
    at io.fabric8.maven.docker.service.BuildService.buildImage(BuildService.java:66)
    at io.fabric8.maven.docker.BuildMojo.proceedWithDockerBuild(BuildMojo.java:110)
    at io.fabric8.maven.docker.BuildMojo.proceedWithBuildProcess(BuildMojo.java:91)
    at io.fabric8.maven.docker.BuildMojo.buildAndTag(BuildMojo.java:84)
    at io.fabric8.maven.docker.BuildMojo.processImageConfig(BuildMojo.java:163)
    at io.fabric8.maven.docker.BuildMojo.executeInternal(BuildMojo.java:73)
    at io.fabric8.maven.docker.AbstractDockerMojo.execute(AbstractDockerMojo.java:302)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:144)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2(MojoExecutor.java:339)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute(MojoExecutor.java:310)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:214)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:179)
    at org.apache.maven.lifecycle.internal.MojoExecutor$1.run(MojoExecutor.java:168)
    at org.apache.maven.plugin.DefaultMojosExecutionStrategy.execute(DefaultMojosExecutionStrategy.java:39)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:165)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:110)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:76)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:60)
    at org.apache.maven.lifecycle.internal.DefaultLifecycleStarter.execute(DefaultLifecycleStarter.java:123)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:311)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:225)
    at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:149)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.doExecute(MavenInvoker.java:470)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:95)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:85)
    at org.apache.maven.cling.invoker.LookupInvoker.doInvoke(LookupInvoker.java:145)
    at org.apache.maven.cling.invoker.LookupInvoker.invoke(LookupInvoker.java:116)
    at org.apache.maven.cling.ClingSupport.run(ClingSupport.java:64)
    at org.apache.maven.cling.MavenCling.main(MavenCling.java:51)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
    at java.lang.reflect.Method.invoke(Method.java:580)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:255)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:201)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:361)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:314)
Caused by: java.util.NoSuchElementException
    at java.util.Collections$EmptyIterator.next(Collections.java:4531)
    at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:265)
    at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:245)
    at io.fabric8.maven.docker.util.AuthConfigFactory.decrypt(AuthConfigFactory.java:674)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfigFromServer(AuthConfigFactory.java:689)
    at io.fabric8.maven.docker.util.AuthConfigFactory.getAuthConfigFromSettings(AuthConfigFactory.java:497)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createStandardAuthConfig(AuthConfigFactory.java:231)
    at io.fabric8.maven.docker.util.AuthConfigFactory.createAuthConfig(AuthConfigFactory.java:120)
    at io.fabric8.maven.docker.service.RegistryService$RegistryConfig.createAuthConfig(RegistryService.java:267)
    at io.fabric8.maven.docker.service.RegistryService.createAuthConfig(RegistryService.java:225)
    at io.fabric8.maven.docker.service.RegistryService.pullImageWithPolicy(RegistryService.java:139)
    at io.fabric8.maven.docker.service.BuildService.autoPullBaseImage(BuildService.java:325)
    at io.fabric8.maven.docker.service.BuildService.buildImage(BuildService.java:66)
    at io.fabric8.maven.docker.BuildMojo.proceedWithDockerBuild(BuildMojo.java:110)
    at io.fabric8.maven.docker.BuildMojo.proceedWithBuildProcess(BuildMojo.java:91)
    at io.fabric8.maven.docker.BuildMojo.buildAndTag(BuildMojo.java:84)
    at io.fabric8.maven.docker.BuildMojo.processImageConfig(BuildMojo.java:163)
    at io.fabric8.maven.docker.BuildMojo.executeInternal(BuildMojo.java:73)
    at io.fabric8.maven.docker.AbstractDockerMojo.execute(AbstractDockerMojo.java:302)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:144)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2(MojoExecutor.java:339)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute(MojoExecutor.java:310)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:214)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:179)
    at org.apache.maven.lifecycle.internal.MojoExecutor$1.run(MojoExecutor.java:168)
    at org.apache.maven.plugin.DefaultMojosExecutionStrategy.execute(DefaultMojosExecutionStrategy.java:39)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:165)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:110)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:76)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:60)
    at org.apache.maven.lifecycle.internal.DefaultLifecycleStarter.execute(DefaultLifecycleStarter.java:123)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:311)
    at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:225)
    at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:149)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.doExecute(MavenInvoker.java:470)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:95)
    at org.apache.maven.cling.invoker.mvn.MavenInvoker.execute(MavenInvoker.java:85)
    at org.apache.maven.cling.invoker.LookupInvoker.doInvoke(LookupInvoker.java:145)
    at org.apache.maven.cling.invoker.LookupInvoker.invoke(LookupInvoker.java:116)
    at org.apache.maven.cling.ClingSupport.run(ClingSupport.java:64)
    at org.apache.maven.cling.MavenCling.main(MavenCling.java:51)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
    at java.lang.reflect.Method.invoke(Method.java:580)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:255)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:201)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:361)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:314)
[ERROR] 
[ERROR] Re-run Maven using the '-X' switch to enable verbose output
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```
