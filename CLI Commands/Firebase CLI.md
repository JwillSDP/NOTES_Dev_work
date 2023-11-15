# **[Firebase CLI reference](https://firebase.google.com/docs/cli)**
#### [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)

## Install the Firebase CLI via npm
``` bash
npm install -g firebase-tools
```
> This command enables the globally available Firebase command.

</br>
</br>
</br>

# Log in and test the Firebase CLI
### Log into Firebase using your Google account
``` bash
firebase login
```
> After installing the CLI, you must authenticate. Then you can confirm authentication by listing your Firebase projects. The firebase login command opens a web page that connects to localhost on your machine. If you're using a remote machine and don't have access to localhost, run the command with the flag --no-localhost.

</br>
## Initialize a Firebase project
``` bash
firebase init
```
> Many common tasks performed using the CLI, such as deploying to a Firebase project, require a project directory. You establish a project directory using the firebase init command. A project directory is usually the same directory as your source control root, and after running firebase init, the directory contains a firebase.json configuration file.

</br>
## Firebase projects
``` bash
firebase projects:list
```
> Test that the CLI is properly installed and accessing your account by listing your Firebase projects. The displayed list should be the same as the Firebase projects listed in the [Firebase console](https://console.firebase.google.com/u/0/?_gl=1*14imetw*_ga*NjE2NDk0NjYyLjE2OTI1NDUxMjI.*_ga_CW55HF8NVT*MTY5MjU0NTEyMi4xLjAuMTY5MjU0NTEyMi4wLjAuMA..&pli=1)

</br>
## Update the firebase CLI version
``` bash
npm install -g firebase-tools
```

</br>
### Firebase Logout
``` bash
firebase logout
```
</br>
## Use project aliase
``` bash
firebase use
```
</br>
## Clears the active project
``` bash
firebase use --clear
```
</br>
</br>
</br>
***
</br>
</br>
</br>

##  View and test your Firebase project on locally hosted URLs
``` bash
firebase serve --only hosting
```
