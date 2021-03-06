# Desugar Issue Report

The goal of this project is to reproduce an issue with [Realm - Java SDK](https://www.mongodb.com/docs/realm/sdk/java/install/) when [gradle configuration cache](https://docs.gradle.org/7.4/userguide/configuration_cache.html) is enabled in Android project.
The issue was reported on [Realm GitHub #7666](https://github.com/realm/realm-java/issues/7666).

## Problem Description

Android project using Kotlin and with **Realm - Java SDK** included won't build if [Gradle's configuration cache](https://docs.gradle.org/7.4/userguide/configuration_cache.html) is enabled.
Even very simple project with no actual functionality is not buildable under this circumstances.
Enabling cache in gradle properties or running with switch `gradlew --configuration-cache assembleDebug` fails with `Configuration cache state could not be cached` error.

Same project builds fine when Realm is not present (_plugin id 'realm-android' not applied_).

### Exception thrown

```shell
FAILURE: Build failed with an exception.

* What went wrong:
Configuration cache state could not be cached: input property '$1' of ':app:transformClassesWithRealmTransformerForDebug': error writing value of type 'org.gradle.api.internal.file.FilteredFileTree'
> Querying the mapped value of provider(interface java.util.Set) before task ':app:compileDebugKotlin' has completed is not supported

* Try:
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Exception is:
org.gradle.configurationcache.ConfigurationCacheError: Configuration cache state could not be cached: input property '$1' of ':app:transformClassesWithRealmTransformerForDebug': error writing value of type 'org.gradle.api.internal.file.FilteredFileTree'
        at org.gradle.configurationcache.serialization.beans.BeanPropertyWriterKt.writeNextProperty(BeanPropertyWriter.kt:106)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.writeRegisteredPropertiesOf$lambda-6$writeProperty(TaskNodeCodec.kt:279)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.writeRegisteredPropertiesOf$lambda-6$writeInputProperty(TaskNodeCodec.kt:283)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.writeRegisteredPropertiesOf(TaskNodeCodec.kt:294)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.access$writeRegisteredPropertiesOf(TaskNodeCodec.kt:1)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec$writeTask$3$2$1.invokeSuspend(TaskNodeCodec.kt:102)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec$writeTask$3$2$1.invoke(TaskNodeCodec.kt)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec$writeTask$3$2$1.invoke(TaskNodeCodec.kt)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.withTaskOf(TaskNodeCodec.kt:234)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodecKt.access$withTaskOf(TaskNodeCodec.kt:1)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec.writeTask(TaskNodeCodec.kt:95)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec.encode(TaskNodeCodec.kt:71)
        at org.gradle.configurationcache.serialization.codecs.TaskNodeCodec.encode(TaskNodeCodec.kt:64)
        at org.gradle.configurationcache.serialization.codecs.BindingsBackedCodec.encode(BindingsBackedCodec.kt:52)
        at org.gradle.configurationcache.serialization.DefaultWriteContext.write(Contexts.kt:80)
        at org.gradle.configurationcache.serialization.codecs.WorkNodeCodec.writeNode(WorkNodeCodec.kt:77)
        at org.gradle.configurationcache.serialization.codecs.WorkNodeCodec.writeNodes(WorkNodeCodec.kt:54)
        at org.gradle.configurationcache.serialization.codecs.WorkNodeCodec.writeWork(WorkNodeCodec.kt:39)
        at org.gradle.configurationcache.ConfigurationCacheState.writeWorkGraphOf(ConfigurationCacheState.kt:254)
        at org.gradle.configurationcache.ConfigurationCacheState.writeBuildState$configuration_cache(ConfigurationCacheState.kt:207)
        at org.gradle.configurationcache.ConfigurationCacheState.writeRootBuild(ConfigurationCacheState.kt:169)
        at org.gradle.configurationcache.ConfigurationCacheState.writeRootBuildState(ConfigurationCacheState.kt:112)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeRootBuildStateTo$1.invokeSuspend(ConfigurationCacheIO.kt:130)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeRootBuildStateTo$1.invoke(ConfigurationCacheIO.kt)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeRootBuildStateTo$1.invoke(ConfigurationCacheIO.kt)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeConfigurationCacheState$1$1.invokeSuspend(ConfigurationCacheIO.kt:181)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeConfigurationCacheState$1$1.invoke(ConfigurationCacheIO.kt)
        at org.gradle.configurationcache.ConfigurationCacheIO$writeConfigurationCacheState$1$1.invoke(ConfigurationCacheIO.kt)
        at org.gradle.configurationcache.serialization.RunningKt$runWriteOperation$1.invokeSuspend(Running.kt:45)
        at kotlin.coroutines.jvm.internal.BaseContinuationImpl.resumeWith(ContinuationImpl.kt:33)
        at kotlin.coroutines.ContinuationKt.startCoroutine(Continuation.kt:115)
        at org.gradle.configurationcache.serialization.RunningKt.runToCompletion(Running.kt:56)
        at org.gradle.configurationcache.serialization.RunningKt.runWriteOperation(Running.kt:44)
        at org.gradle.configurationcache.ConfigurationCacheIO.writeConfigurationCacheState(ConfigurationCacheIO.kt:180)
        at org.gradle.configurationcache.ConfigurationCacheIO.writeRootBuildStateTo$configuration_cache(ConfigurationCacheIO.kt:128)
        at org.gradle.configurationcache.DefaultConfigurationCache$writeConfigurationCacheState$1.create(DefaultConfigurationCache.kt:378)
        at org.gradle.configurationcache.DefaultConfigurationCache$writeConfigurationCacheState$1.create(DefaultConfigurationCache.kt:377)
        at org.gradle.internal.work.DefaultWorkerLeaseService.withoutLocks(DefaultWorkerLeaseService.java:332)
        at org.gradle.api.internal.project.DefaultProjectStateRegistry.lambda$withMutableStateOfAllProjects$0(DefaultProjectStateRegistry.java:154)
        at org.gradle.internal.work.DefaultWorkerLeaseService.withLocks(DefaultWorkerLeaseService.java:270)
        at org.gradle.api.internal.project.DefaultProjectStateRegistry.withMutableStateOfAllProjects(DefaultProjectStateRegistry.java:154)
        at org.gradle.configurationcache.DefaultConfigurationCache.writeConfigurationCacheState(DefaultConfigurationCache.kt:377)
        at org.gradle.configurationcache.DefaultConfigurationCache.access$writeConfigurationCacheState(DefaultConfigurationCache.kt:54)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveWorkGraph$1.invoke(DefaultConfigurationCache.kt:304)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveWorkGraph$1.invoke(DefaultConfigurationCache.kt:304)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveToCache$1$1.invoke(DefaultConfigurationCache.kt:319)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveToCache$1$1.invoke(DefaultConfigurationCache.kt:317)
        at org.gradle.configurationcache.ConfigurationCacheRepository$StoreImpl$useForStore$1.invoke(ConfigurationCacheRepository.kt:177)
        at org.gradle.configurationcache.ConfigurationCacheRepository$StoreImpl$useForStore$1.invoke(ConfigurationCacheRepository.kt:169)
        at org.gradle.configurationcache.ConfigurationCacheRepository$withExclusiveAccessToCache$1.create(ConfigurationCacheRepository.kt:244)
        at org.gradle.cache.internal.LockOnDemandCrossProcessCacheAccess.withFileLock(LockOnDemandCrossProcessCacheAccess.java:90)
        at org.gradle.cache.internal.DefaultCacheAccess.withFileLock(DefaultCacheAccess.java:191)
        at org.gradle.cache.internal.DefaultPersistentDirectoryStore.withFileLock(DefaultPersistentDirectoryStore.java:188)
        at org.gradle.cache.internal.DefaultCacheFactory$ReferenceTrackingCache.withFileLock(DefaultCacheFactory.java:209)
        at org.gradle.configurationcache.ConfigurationCacheRepository.withExclusiveAccessToCache(ConfigurationCacheRepository.kt:242)
        at org.gradle.configurationcache.ConfigurationCacheRepository.access$withExclusiveAccessToCache(ConfigurationCacheRepository.kt:46)
        at org.gradle.configurationcache.ConfigurationCacheRepository$StoreImpl.useForStore(ConfigurationCacheRepository.kt:169)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveToCache$1.invoke(DefaultConfigurationCache.kt:317)
        at org.gradle.configurationcache.DefaultConfigurationCache$saveToCache$1.invoke(DefaultConfigurationCache.kt:316)
        at org.gradle.configurationcache.ConfigurationCacheBuildOperationsKt$withOperation$1.call(ConfigurationCacheBuildOperations.kt:39)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$CallableBuildOperationWorker.execute(DefaultBuildOperationRunner.java:204)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$CallableBuildOperationWorker.execute(DefaultBuildOperationRunner.java:199)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:66)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:157)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.call(DefaultBuildOperationRunner.java:53)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor.call(DefaultBuildOperationExecutor.java:73)
        at org.gradle.configurationcache.ConfigurationCacheBuildOperationsKt.withOperation(ConfigurationCacheBuildOperations.kt:37)
        at org.gradle.configurationcache.ConfigurationCacheBuildOperationsKt.withStoreOperation(ConfigurationCacheBuildOperations.kt:32)
        at org.gradle.configurationcache.DefaultConfigurationCache.saveToCache(DefaultConfigurationCache.kt:316)
        at org.gradle.configurationcache.DefaultConfigurationCache.saveWorkGraph(DefaultConfigurationCache.kt:304)
        at org.gradle.configurationcache.DefaultConfigurationCache.access$saveWorkGraph(DefaultConfigurationCache.kt:54)
        at org.gradle.configurationcache.DefaultConfigurationCache$loadOrScheduleRequestedTasks$1.invoke(DefaultConfigurationCache.kt:134)
        at org.gradle.configurationcache.DefaultConfigurationCache$loadOrScheduleRequestedTasks$1.invoke(DefaultConfigurationCache.kt:132)
        at org.gradle.configurationcache.DefaultConfigurationCache.runWorkThatContributesToCacheEntry(DefaultConfigurationCache.kt:276)
        at org.gradle.configurationcache.DefaultConfigurationCache.loadOrScheduleRequestedTasks(DefaultConfigurationCache.kt:132)
        at org.gradle.configurationcache.ConfigurationCacheAwareBuildTreeWorkPreparer.scheduleRequestedTasks(ConfigurationCacheAwareBuildTreeWorkPreparer.kt:28)
        at org.gradle.internal.buildtree.DefaultBuildTreeLifecycleController.lambda$doScheduleAndRunTasks$2(DefaultBuildTreeLifecycleController.java:89)
        at org.gradle.composite.internal.DefaultIncludedBuildTaskGraph.withNewWorkGraph(DefaultIncludedBuildTaskGraph.java:75)
        at org.gradle.internal.buildtree.DefaultBuildTreeLifecycleController.doScheduleAndRunTasks(DefaultBuildTreeLifecycleController.java:88)
        at org.gradle.internal.buildtree.DefaultBuildTreeLifecycleController.lambda$runBuild$4(DefaultBuildTreeLifecycleController.java:106)
        at org.gradle.internal.model.StateTransitionController.lambda$transition$6(StateTransitionController.java:166)
        at org.gradle.internal.model.StateTransitionController.doTransition(StateTransitionController.java:238)
        at org.gradle.internal.model.StateTransitionController.lambda$transition$7(StateTransitionController.java:166)
        at org.gradle.internal.work.DefaultSynchronizer.withLock(DefaultSynchronizer.java:44)
        at org.gradle.internal.model.StateTransitionController.transition(StateTransitionController.java:166)
        at org.gradle.internal.buildtree.DefaultBuildTreeLifecycleController.runBuild(DefaultBuildTreeLifecycleController.java:103)
        at org.gradle.internal.buildtree.DefaultBuildTreeLifecycleController.scheduleAndRunTasks(DefaultBuildTreeLifecycleController.java:69)
        at org.gradle.tooling.internal.provider.ExecuteBuildActionRunner.run(ExecuteBuildActionRunner.java:31)
        at org.gradle.launcher.exec.ChainingBuildActionRunner.run(ChainingBuildActionRunner.java:35)
        at org.gradle.internal.buildtree.ProblemReportingBuildActionRunner.run(ProblemReportingBuildActionRunner.java:49)
        at org.gradle.launcher.exec.BuildOutcomeReportingBuildActionRunner.run(BuildOutcomeReportingBuildActionRunner.java:69)
        at org.gradle.tooling.internal.provider.FileSystemWatchingBuildActionRunner.run(FileSystemWatchingBuildActionRunner.java:119)
        at org.gradle.launcher.exec.BuildCompletionNotifyingBuildActionRunner.run(BuildCompletionNotifyingBuildActionRunner.java:41)
        at org.gradle.launcher.exec.RootBuildLifecycleBuildActionExecutor.lambda$execute$0(RootBuildLifecycleBuildActionExecutor.java:40)
        at org.gradle.composite.internal.DefaultRootBuildState.run(DefaultRootBuildState.java:128)
        at org.gradle.launcher.exec.RootBuildLifecycleBuildActionExecutor.execute(RootBuildLifecycleBuildActionExecutor.java:40)
        at org.gradle.internal.buildtree.DefaultBuildTreeContext.execute(DefaultBuildTreeContext.java:40)
        at org.gradle.launcher.exec.BuildTreeLifecycleBuildActionExecutor.lambda$execute$0(BuildTreeLifecycleBuildActionExecutor.java:65)
        at org.gradle.internal.buildtree.BuildTreeState.run(BuildTreeState.java:53)
        at org.gradle.launcher.exec.BuildTreeLifecycleBuildActionExecutor.execute(BuildTreeLifecycleBuildActionExecutor.java:65)
        at org.gradle.launcher.exec.RunAsBuildOperationBuildActionExecutor$3.call(RunAsBuildOperationBuildActionExecutor.java:61)
        at org.gradle.launcher.exec.RunAsBuildOperationBuildActionExecutor$3.call(RunAsBuildOperationBuildActionExecutor.java:57)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$CallableBuildOperationWorker.execute(DefaultBuildOperationRunner.java:204)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$CallableBuildOperationWorker.execute(DefaultBuildOperationRunner.java:199)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:66)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:157)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.call(DefaultBuildOperationRunner.java:53)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor.call(DefaultBuildOperationExecutor.java:73)
        at org.gradle.launcher.exec.RunAsBuildOperationBuildActionExecutor.execute(RunAsBuildOperationBuildActionExecutor.java:57)
        at org.gradle.launcher.exec.RunAsWorkerThreadBuildActionExecutor.lambda$execute$0(RunAsWorkerThreadBuildActionExecutor.java:36)
        at org.gradle.internal.work.DefaultWorkerLeaseService.withLocks(DefaultWorkerLeaseService.java:270)
        at org.gradle.internal.work.DefaultWorkerLeaseService.runAsWorkerThread(DefaultWorkerLeaseService.java:119)
        at org.gradle.launcher.exec.RunAsWorkerThreadBuildActionExecutor.execute(RunAsWorkerThreadBuildActionExecutor.java:36)
        at org.gradle.tooling.internal.provider.ContinuousBuildActionExecutor.execute(ContinuousBuildActionExecutor.java:103)
        at org.gradle.tooling.internal.provider.SubscribableBuildActionExecutor.execute(SubscribableBuildActionExecutor.java:64)
        at org.gradle.internal.session.DefaultBuildSessionContext.execute(DefaultBuildSessionContext.java:46)
        at org.gradle.tooling.internal.provider.BuildSessionLifecycleBuildActionExecuter$ActionImpl.apply(BuildSessionLifecycleBuildActionExecuter.java:100)
        at org.gradle.tooling.internal.provider.BuildSessionLifecycleBuildActionExecuter$ActionImpl.apply(BuildSessionLifecycleBuildActionExecuter.java:88)
        at org.gradle.internal.session.BuildSessionState.run(BuildSessionState.java:69)
        at org.gradle.tooling.internal.provider.BuildSessionLifecycleBuildActionExecuter.execute(BuildSessionLifecycleBuildActionExecuter.java:62)
        at org.gradle.tooling.internal.provider.BuildSessionLifecycleBuildActionExecuter.execute(BuildSessionLifecycleBuildActionExecuter.java:41)
        at org.gradle.tooling.internal.provider.StartParamsValidatingActionExecuter.execute(StartParamsValidatingActionExecuter.java:63)
        at org.gradle.tooling.internal.provider.StartParamsValidatingActionExecuter.execute(StartParamsValidatingActionExecuter.java:31)
        at org.gradle.tooling.internal.provider.SessionFailureReportingActionExecuter.execute(SessionFailureReportingActionExecuter.java:58)
        at org.gradle.tooling.internal.provider.SessionFailureReportingActionExecuter.execute(SessionFailureReportingActionExecuter.java:42)
        at org.gradle.tooling.internal.provider.SetupLoggingActionExecuter.execute(SetupLoggingActionExecuter.java:47)
        at org.gradle.tooling.internal.provider.SetupLoggingActionExecuter.execute(SetupLoggingActionExecuter.java:31)
        at org.gradle.launcher.daemon.server.exec.ExecuteBuild.doBuild(ExecuteBuild.java:65)
        at org.gradle.launcher.daemon.server.exec.BuildCommandOnly.execute(BuildCommandOnly.java:37)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.WatchForDisconnection.execute(WatchForDisconnection.java:39)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.ResetDeprecationLogger.execute(ResetDeprecationLogger.java:29)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.RequestStopIfSingleUsedDaemon.execute(RequestStopIfSingleUsedDaemon.java:35)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.ForwardClientInput$2.create(ForwardClientInput.java:78)
        at org.gradle.launcher.daemon.server.exec.ForwardClientInput$2.create(ForwardClientInput.java:75)
        at org.gradle.util.internal.Swapper.swap(Swapper.java:38)
        at org.gradle.launcher.daemon.server.exec.ForwardClientInput.execute(ForwardClientInput.java:75)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.LogAndCheckHealth.execute(LogAndCheckHealth.java:55)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.LogToClient.doBuild(LogToClient.java:63)
        at org.gradle.launcher.daemon.server.exec.BuildCommandOnly.execute(BuildCommandOnly.java:37)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.EstablishBuildEnvironment.doBuild(EstablishBuildEnvironment.java:84)
        at org.gradle.launcher.daemon.server.exec.BuildCommandOnly.execute(BuildCommandOnly.java:37)
        at org.gradle.launcher.daemon.server.api.DaemonCommandExecution.proceed(DaemonCommandExecution.java:104)
        at org.gradle.launcher.daemon.server.exec.StartBuildOrRespondWithBusy$1.run(StartBuildOrRespondWithBusy.java:52)
        at org.gradle.launcher.daemon.server.DaemonStateCoordinator$1.run(DaemonStateCoordinator.java:297)
        at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:64)
        at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:48)
Caused by: org.gradle.api.InvalidUserCodeException: Querying the mapped value of provider(interface java.util.Set) before task ':app:compileDebugKotlin' has completed is not supported
        at org.gradle.api.internal.provider.TransformBackedProvider.lambda$beforeRead$0(TransformBackedProvider.java:84)
        at org.gradle.api.internal.provider.BuildableBackedProvider$1.visitProducerTasks(BuildableBackedProvider.java:56)
        at org.gradle.api.internal.provider.ValueSupplier$ValueProducer.visitContentProducerTasks(ValueSupplier.java:59)
        at org.gradle.api.internal.provider.TransformBackedProvider.beforeRead(TransformBackedProvider.java:81)
        at org.gradle.api.internal.provider.TransformBackedProvider.calculateOwnValue(TransformBackedProvider.java:63)
        at org.gradle.api.internal.provider.AbstractMinimalProvider.calculateValue(AbstractMinimalProvider.java:103)
        at org.gradle.api.internal.provider.Collectors$ElementsFromCollectionProvider.collectEntries(Collectors.java:216)
        at org.gradle.api.internal.provider.AbstractCollectionProperty$PlusCollector.collectEntries(AbstractCollectionProperty.java:450)
        at org.gradle.api.internal.provider.AbstractCollectionProperty$PlusCollector.collectEntries(AbstractCollectionProperty.java:450)
        at org.gradle.api.internal.provider.AbstractCollectionProperty$CollectingSupplier.calculateValue(AbstractCollectionProperty.java:337)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:184)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:37)
        at org.gradle.api.internal.provider.AbstractProperty.doCalculateValue(AbstractProperty.java:133)
        at org.gradle.api.internal.provider.AbstractProperty.calculateOwnValue(AbstractProperty.java:127)
        at org.gradle.api.internal.provider.AbstractMinimalProvider.calculateValue(AbstractMinimalProvider.java:103)
        at org.gradle.api.internal.provider.Collectors$ElementsFromCollectionProvider.collectEntries(Collectors.java:216)
        at org.gradle.api.internal.provider.AbstractCollectionProperty$CollectingSupplier.calculateValue(AbstractCollectionProperty.java:337)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:184)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:37)
        at org.gradle.api.internal.provider.AbstractProperty.doCalculateValue(AbstractProperty.java:133)
        at org.gradle.api.internal.provider.AbstractProperty.calculateOwnValue(AbstractProperty.java:127)
        at org.gradle.api.internal.provider.AbstractMinimalProvider.calculateValue(AbstractMinimalProvider.java:103)
        at org.gradle.api.internal.provider.Collectors$ElementsFromCollectionProvider.collectEntries(Collectors.java:216)
        at org.gradle.api.internal.provider.AbstractCollectionProperty$CollectingSupplier.calculateValue(AbstractCollectionProperty.java:337)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:184)
        at org.gradle.api.internal.provider.AbstractCollectionProperty.calculateValueFrom(AbstractCollectionProperty.java:37)
        at org.gradle.api.internal.provider.AbstractProperty.doCalculateValue(AbstractProperty.java:133)
        at org.gradle.api.internal.provider.AbstractProperty.calculateOwnValue(AbstractProperty.java:127)
        at org.gradle.api.internal.provider.AbstractMinimalProvider.get(AbstractMinimalProvider.java:84)
        at org.gradle.api.internal.provider.ProviderResolutionStrategy$2.resolve(ProviderResolutionStrategy.java:33)
        at org.gradle.api.internal.file.collections.ProviderBackedFileCollection.visitChildren(ProviderBackedFileCollection.java:63)
        at org.gradle.api.internal.file.CompositeFileCollection.visitContents(CompositeFileCollection.java:119)
        at org.gradle.api.internal.file.AbstractFileCollection.visitStructure(AbstractFileCollection.java:351)
        at org.gradle.api.internal.file.CompositeFileCollection.lambda$visitContents$0(CompositeFileCollection.java:119)
        at org.gradle.api.internal.file.collections.UnpackingVisitor.add(UnpackingVisitor.java:74)
        at org.gradle.api.internal.file.collections.DefaultConfigurableFileCollection$UnresolvedItemsCollector.visitContents(DefaultConfigurableFileCollection.java:372)
        at org.gradle.api.internal.file.collections.DefaultConfigurableFileCollection.visitChildren(DefaultConfigurableFileCollection.java:284)
        at org.gradle.api.internal.file.CompositeFileCollection.visitContents(CompositeFileCollection.java:119)
        at org.gradle.api.internal.file.AbstractFileCollection.visitStructure(AbstractFileCollection.java:351)
        at org.gradle.api.internal.file.FileCollectionBackedFileTree.visitContents(FileCollectionBackedFileTree.java:98)
        at org.gradle.api.internal.file.FileCollectionBackedFileTree.visitContentsAsFileTrees(FileCollectionBackedFileTree.java:73)
        at org.gradle.api.internal.file.FilteredFileTree.visitChildren(FilteredFileTree.java:66)
        at org.gradle.api.internal.file.CompositeFileCollection.visitContents(CompositeFileCollection.java:119)
        at org.gradle.api.internal.file.AbstractFileCollection.visitStructure(AbstractFileCollection.java:351)
        at org.gradle.configurationcache.serialization.codecs.FileTreeCodec.rootSpecOf(FileTreeCodec.kt:113)
        at org.gradle.configurationcache.serialization.codecs.FileTreeCodec.encode(FileTreeCodec.kt:86)
        at org.gradle.configurationcache.serialization.codecs.FileTreeCodec.encode(FileTreeCodec.kt:77)
        at org.gradle.configurationcache.serialization.codecs.BindingsBackedCodec.encode(BindingsBackedCodec.kt:52)
        at org.gradle.configurationcache.serialization.DefaultWriteContext.write(Contexts.kt:80)
        at org.gradle.configurationcache.serialization.beans.BeanPropertyWriterKt.writeNextProperty(BeanPropertyWriter.kt:100)
        ... 156 more
```

### Expected Behavior

Project can build with configuration cache enabled.