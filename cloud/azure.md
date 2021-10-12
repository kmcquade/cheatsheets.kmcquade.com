# Azure

## Command-line setup

### Local Credential Storage

* On a Mac, install Powershell with Homebrew and then enter Powershell

```
# Have Homebrew installed
brew install --cask powershell

# Enter powershell and follow the next steps
pwsh
```

* Then enter Powershell using `pwsh` and run this script to install the `Az` module:

```

if ($PSVersionTable.PSEdition -eq 'Desktop' -and (Get-Module -Name AzureRM -ListAvailable)) {
    Write-Warning -Message ('Az module not installed. Having both the AzureRM and ' +
      'Az modules installed at the same time is not supported.')
} else {
    Install-Module -Name Az -AllowClobber -Scope AllUsers
}
```

By default, when you run `Connect-AzAccount` to sign in with your Azure credentials, it will require you to repeat those steps for every new PowerShell session you start. **This is fucking annoying**. To persist your Azure sign-in across PowerShell sessions, follow these instructions.

* first, create a directory called `~/.Azure` (uppercase A) if you don't have one already

```
mkdir -p ~/.Azure

```

* Then go back to Powershell and `Connect-AzAccount`

```
Connect-AzAccount
# That will create a file titled:
# /Users/username/.Azure/AzureRmContext.json

# then run this command so that the Azure credentials are saved 
Enable-AzContextAutosave

```

### Set up ZSH Completion

* Modify your `.zshrc` file and add the following two lines at the end of the file:

```
autoload bashcompinit && bashcompinit
source /usr/local/etc/bash_completion.d/az
```

* Save and reload your zsh profile with `source .zshrc` or by restarting your terminal.
* To test, type `az acc` and hit ‘tab’. If completions are setup correctly the command `az account` will be autocompleted for you. 
