# **[Firebase CLI reference](https://firebase.google.com/docs/cli)**
#### [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)
<br></br>

## Install the Firebase CLI via npm
``` bash
npm install -g firebase-tools
```
> This command enables the globally available Firebase command.



<br></br>


# Log in and test the Firebase CLI
### Log into Firebase using your Google account
``` bash
firebase login
```
> After installing the CLI, you must authenticate. Then you can confirm authentication by listing your Firebase projects. The firebase login command opens a web page that connects to localhost on your machine. If you're using a remote machine and don't have access to localhost, run the command with the flag --no-localhost.

<br></br>

## Initialize a Firebase project
``` bash
firebase init
```
> Many common tasks performed using the CLI, such as deploying to a Firebase project, require a project directory. You establish a project directory using the firebase init command. A project directory is usually the same directory as your source control root, and after running firebase init, the directory contains a firebase.json configuration file.
> The firebase init command steps you through setting up your project directory and some Firebase products. During project initialization:
> - Select desired Firebase products to set up in your Firebase project
> - Select a default Firebase project
> - **firebase.json** configuration file that lists your project configuration.
> - **.firebaserc** file that stores your project aliases.

##  Prints the Google services configuration of a Firebase App
``` bash
firebase apps:sdkconfig
```

<br></br>
#  FIREBASE LISTS

## Firebase projects List
``` bash
firebase projects:list
```
> Test that the CLI is properly installed and accessing your account by listing your Firebase projects. The displayed list should be the same as the Firebase projects listed in the [Firebase console](https://console.firebase.google.com/u/0/?_gl=1*14imetw*_ga*NjE2NDk0NjYyLjE2OTI1NDUxMjI.*_ga_CW55HF8NVT*MTY5MjU0NTEyMi4xLjAuMTY5MjU0NTEyMi4wLjAuMA..&pli=1)

##  List databases in your Firebase project
``` bash
firebase firestore:databases:list
```

##  Lists all the extension instances installed in a Firebase project. Prints the instance ID for each extension.
``` bash
firebase ext:list
```

##  Lists all Hosting sites for the active Firebase project
``` bash
firebase hosting:sites:list
```

##  Lists all channels (including the "live" channel) in the default Hosting site
``` bash
firebase hosting:channel:list
```

##  List all database instances for this project. Accepts the --location option for listing databases in a specified region
``` bash
database:instances:list
```

<br></br>

### Firebase Logout
``` bash
firebase logout
```

<br></br>

## Use project aliase
``` bash
firebase use
```

<br></br>

## Clears the active project
``` bash
firebase use --clear
```

<br></br>


##  View and test your Firebase project on locally hosted URLs
``` bash
firebase serve --only hosting
```


##  List available locations for your Cloud Firestore database
``` bash
firebase firestore:locations
```
