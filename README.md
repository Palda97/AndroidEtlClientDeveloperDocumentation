# Developer documentation for LinkedPipes ETL Client for Android
The javadoc can be found [here](https://palda97.github.io/AndroidEtlClientDeveloperDocumentation/).

## Basic overview
The application uses the [MVVM pattern](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel).
Code responsible for displaying stuff is in *view* package. Code responsible for the UI logic is located in *viewmodel* package. The rest of the code is in *model* package.
There are two source code files in the root package.

- Injector: Gives access to repositories.
- AppInit: Application class handling stuff that happens on the application start.

## model
This package contains layers responsible for gathering and storing data.

### db
DAOs for the [Room](https://developer.android.com/training/data-storage/room) database.

### entities
Entities for the database and factories to produce them, mainly from jsonLd.

### network
[Retrofit](https://square.github.io/retrofit/) interfaces for downloading data.

### repository
Repository classes responsible for communicating with the network layer and DAO layer. These are the only single source of truth for the viewmodel layer.

### services
There is a CoroutineWorker that is fed to the WorkManager and is used for monitoring the execution status of pipelines executed via this application.

### travelobjects
Package containing code for working with jsonLd.

## viewmodel
This package contains layer responsible for giving the view layer correct data and acts as as middle man between view layer and model layer.

## view
Package containing code for displaying data to user and handling input.
