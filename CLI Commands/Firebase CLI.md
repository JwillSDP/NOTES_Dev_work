# **[Firebase CLI reference](https://firebase.google.com/docs/cli)**
#### [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)

### Install the Firebase CLI via npm
``` bash
npm install -g firebase-tools
```
> This command enables the globally available firebase command.

## Log in and test the Firebase CLI
### Firebase Login
``` bash
firebase login
```

### Initialize a Firebase project
``` bash
firebase init
```
> Many common tasks performed using the CLI, such as deploying to a Firebase project, require a project directory. You establish a project directory using the firebase init command. A project directory is usually the same directory as your source control root, and after running firebase init, the directory contains a firebase.json configuration file.
> 
### Firebase Logout
``` bash
firebase logout
```
### Firebase projects
``` bash
firebase projects:list
```
> Test that the CLI is properly installed and accessing your account by listing your Firebase projects. The displayed list should be the same as the Firebase projects listed in the [Firebase console](https://console.firebase.google.com/u/0/?_gl=1*14imetw*_ga*NjE2NDk0NjYyLjE2OTI1NDUxMjI.*_ga_CW55HF8NVT*MTY5MjU0NTEyMi4xLjAuMTY5MjU0NTEyMi4wLjAuMA..&pli=1)
### Update the firebase CLI version
``` bash
npm install -g firebase-tools
```
### Firebase Login CI
``` bash
firebase login:ci
```

### Use project aliase
``` bash
firebase use
```



### Clears the active project
``` bash
firebase use --clear
```
###  View and test your Firebase project on locally hosted URLs
``` bash
firebase serve --only hosting
```
### Firebase Login
``` bash
firebase login
```
### Firebase Login
``` bash
firebase login
```
### Firebase Login
``` bash
firebase login
```
