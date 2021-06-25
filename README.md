# Developer documentation for LinkedPipes ETL Client for Android
The javadoc can be found [here](https://palda97.github.io/AndroidEtlClientDeveloperDocumentation/).

## Technologies
There is no need for anything else than the following IDE and stuff that comes with it.

### Android Studio
The recommended IDE is [Android Studio](https://developer.android.com/studio). There is no need to have Java already installed, because Android Studio comes with its own version already bundled.

### Android SDK
After downloading Android Studio, the Android SDK needs to be set up.
Open Android Studio and click on **Configure** and **SDK Manager**. In the bottom right corner, click on **Show Package Details**, scroll to **Android 10.0 (Q)** and check **Android SDK Platform 29**, **Sources for Android 29** and **Google APIs intel x86 Atom System Image**.
In the top part, click on the tab **SDK Tools** and check **Google Play services**. Check again **Show Package Details**, scroll up and check the **29.0.3** version of Android SDK Build-Tools.
Confirm changes by clicking **Apply** and then **OK**.

### Virtual Device
Android Studio comes with one virtual Android phone created, but if there is a need for another device, it can be created by clicking on **Configure** and **AVD Manager**.
Device used for running this application while developing is called **3.2" HVGA slider (ADP1)** containing Android 10 (**Q 29 x86**).
After the first launch, it is recommended to go to the settings, developer options and turn on the **Don't keep activities** setting, located near the bottom.

### Building/Running the application
The toolbar menu contains **Build** folder, where everything around building can be accessed. To run the application, click on the green arrow/triangle in the top part of Android Studio.

### Running the application on a physical device
In order to set up a physical device, [this tutorial](https://developer.android.com/studio/run/device) needs to be followed. For connecting via Wi-Fi to the physical device running Android 10 or an older version, follow the previous tutorial and them follow [this tutorial](https://developer.android.com/studio/command-line/adb#wireless).

## Server instances
An online demo server instance can be found [here](https://demo.etl.linkedpipes.com/). Server instances can also be downloaded from its [github page](https://github.com/linkedpipes/etl).

## Basic code overview
The application uses the [MVVM pattern](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel).
Code responsible for displaying stuff is in *view* package. Code responsible for the UI logic is located in *viewmodel* package. The rest of the code is in *model* package.
There are two important source code files in the root package.

- Injector: Gives access to repositories, provides [Context](https://developer.android.com/reference/kotlin/android/content/Context) and log functions.
- AppInit: Application class handling stuff that happens on the application start.

## model
This package contains layers responsible for gathering and storing data.

### db
DAOs for the [Room](https://developer.android.com/training/data-storage/room) database alongside with the class representing the database itself. New entities and DAOs have to be put into the database class *AppDatabase.kt*.

### entities
Entities for the database and factories to produce them, mainly from jsonLd.

### network
[Retrofit](https://square.github.io/retrofit/) interfaces for downloading data. Retrofit builders are gathered from class *RetrofitHelper.kt* using method *getBuilder* that will create a builder instance configured for accepting jsonLd and optionally with login credentials, if they exist. Each retrofit interface contains a builder extension property that will return the desired retrofit instance.

### repository
Repository classes are responsible for communicating with the network layer and DAO layer. These are the only single source of truth for the viewmodel layer. Each repository is named after the entity it is in charge of, except *RepositoryRoutines.kt*, which is a wrapper for refreshing/updating processes and database cleanup.

### services
There is a [CoroutineWorker](https://developer.android.com/topic/libraries/architecture/workmanager/advanced/coroutineworker) that is fed to the [WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) and is used for monitoring execution statuses of all executed pipelines.
Switching monitoring on and off is controlled from *SettingsViewModel.kt*.

### travelobjects
Package containing code for working with jsonLd.

## viewmodel
This package contains the layer responsible for giving the view layer correct data and acts as a middle man between view layer and repositories. The names of these viewmodels indicate which screens they work with, except *CommonViewModel.kt*, which serves multiple screens.

## view
Package containing code for displaying data to user and handling input. The view layer communicate exclusively with the viewmodel layer by observing its streams and calling its methods.
*MainActivity* is created on launch, containing navigation with *ExecutionsFragment*, *PipelinesFragment* and *SettingsFragment*.