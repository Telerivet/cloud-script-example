# Telerivet Cloud Script API Examples

https://telerivet.com/api/script

This example repository shows how to use the Cloud Script API to handle text messages
by importing code from your own git repository.

Using your own git repository has several advantages over writing the code directly in your browser:

* Allows using source control to track version history
* Allows using your preferred code editors
* Allows building much larger services (the browser-based editor is limited to 100 KB of code)
* Allows reusing functionality without copy-and-paste
* Allows using GitHub pull requests to review changes before deploying

## Deploying Cloud Script Modules

Telerivet provides two ways to deploy Cloud Script Modules: git and the cloud-script CLI (command-line interface).

When using git to deploy Cloud Script Modules, Telerivet will pull code from a branch on GitHub, and can automatically update the module when you push to the branch.

In order to simplify the workflow of developing Cloud Script modules, Telerivet also provides a cloud-script CLI for macOS, Windows, and Linux, which makes it possible to push code directly from your development environment, without needing to commit changes first. The cloud-script CLI can be downloaded from https://telerivet.com/pg/cloud_script_cli .

When developing code using Cloud Script modules, it is generaly recommended to use the cloud-script CLI for development, and use Git for production. In this case, you could create a Cloud Script module that is deployed via the cloud-script CLI (e.g. `ext/example-dev`), and another module that is deployed via git (e.g. `ext/example`).

## Testing Example Code (Git)

To try the example code in this repository, start by forking this repository into your own GitHub account by clicking the "Fork" button in GitHub. (If you don't have your own GitHub account, you can use this repository without forking it, but you won't be able to test deploying your own changes via Git.)

Next, add a Cloud Script Module at https://telerivet.com/dashboard/a/script_modules :

* Set the Module Namespace to `ext/example`
* Choose `Git` as the Deployment Mode
* In your fork on GitHub, click the "Code" dropdown and copy the HTTPS URL, then paste the URL into the "Git Repository Clone URL" field in Telerivet.
* Set the Branch to `master`.
* Click "Add Module".

To configure Telerivet to automatically pull changes whenever you push new code to the branch, click the "Set up GitHub integration" link in Telerivet and follow the prompts to give Telerivet to access your GitHub account. This authorization is optional when using public repositories. However, if you don't configure the GitHub integration, you would need to click the "update" link on https://telerivet.com/dashboard/a/script_modules each time you want Telerivet to pull code from your repository.

Then, create a Cloud Script API service that is triggered for an incoming message at
https://telerivet.com/dashboard/services , and add one line of code:

```
require('ext/example/main');
```

Then, whenever an incoming SMS message is received, it will be handled by the code in main.js.

To test deploying changes to this example code, you can clone your fork to your computer, make changes, commit changes to git, and then push changes to the master branch.

To test your code, you can use the "Test Services" function on https://telerivet.com/dashboard/services.

## Testing Example Code (cloud-script CLI)

You can also deploy the example code in this repository using the cloud-script CLI to test making changes to code in your development environment and push the changes to Telerivet immediately without needing to commit changes to git first.

To deploy the example code in this repository using the cloud-script CLI, first fork this repository into your own GitHub account and clone the forked repository to your own computer (if you haven't already done so in the previous section).

Then, add a Cloud Script Module at https://telerivet.com/dashboard/a/script_modules :

* Set the Module Namespace to `ext/example-dev`
* Choose `cloud-script CLI` as the Deployment Mode
* Click "Add Module".
* Follow the instructions on the next page to install the cloud-script CLI and create a cloud-script.yml file.

Open a terminal or command prompt on your computer, and `cd` to the directory where the code was cloned on your development computer.

Run `cloud-script login` in the terminal window, then follow the instructions to log in.

Run `cloud-script update -w` in the terminal window. The `-w` option will keep the cloud-script command active to watch for changes to any .js files in the module directory until you stop it by pressing Ctrl+C.

Create a Cloud Script API service that is triggered for an incoming message at
https://telerivet.com/dashboard/services , and add one line of code:

```
require('ext/example-dev/main');
```

Then, whenever an incoming SMS message is received, it will be handled by the code in main.js.

Edit the .js files in a text editor on your computer. When you save changes to any .js files, the `cloud-script` command will automatically synchronize changes with Telerivet's servers. You can test your changes by clicking "Test Services" on the top of the Services page.

## How Cloud Script Modules Work

All modules must have the extension .js . They may execute code directly, or may export functionality
to be used in other modules. All variables and functions described on https://telerivet.com/api/script
are automatically available as global variables in all modules.

To import other modules in the same repository, you can use relative imports. For example, if the module has the namespace "ext/example", then from main.js the code
`require('./commands/hello');` has the same effect as `require('ext/example/commands/hello');`.

However, relative imports are always preferable because they allow you to use different branches
for development and production code.

For example, you could import the "dev" branch of this repository into Telerivet with the module ID
`ext/example-dev`. Then the "dev" branch can be tested by adding a Cloud Script API service like this:

```
require('ext/example-dev/main');
```

To import your own repository into the Telerivet Cloud Script API,
go to https://telerivet.com/dashboard/a/script_modules .
