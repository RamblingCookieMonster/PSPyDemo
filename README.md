## What is this?

This DockerFile was used for the September Boston PowerShell User Group.

It does the following - read the dockerfile for more:

* Use centos/latest
* Grab Python 3.5.2 and related bits
* Grab PowerShell 6.0.0-alpha.9
* Set up a virtual environment including LDAP3 for the demo: /source/environments/PSPyExample
* Default to /bin/bash command

## So how do I use it?

We'll assume that you've [installed Docker](https://docs.docker.com/engine/installation) and know [the basics](https://docs.docker.com/engine/getstarted/).

### Run the container

Optionally pull this down beforehand with `docker pull ramblingcookiemonster/pspydemo`

```bash
# Run this container
docker run -it ramblingcookiemonster/pspydemo
```

```bash
# Run this container, mapping your home to /source in the container
docker run -v ~/sc:/source -it ramblingcookiemonster/pspydemo
```

```bash
# Run this container, kick of PowerShell right out
docker run -it ramblingcookiemonster/pspydemo /usr/local/bin/powershell
```

### Do stuff in the container

```bash
# If desired, activate the Python 3 + LDAP3 virtual environment
source bin/activate
```

```bash
# Not running PowerShell yet?
powershell
```

```powershell
# Once we're in PowerShell, we're good to go
# PowerShell works!
    # What commands are available?
    # With a name? with Uri parameter? For a module?
    Get-Command
    Get-Command -Name *-Object
    Get-Command -ParameterName Uri
    Get-Command -Module PackageManagement

    # Number of cmdlets
    # PowerShell v1 (Vista) = 129
    # PowerShell v5 [Win10] = 1,285
    # Linux v6.0.0 [CentOS] = ?
    Get-Command |
        Measure-Object |
        Select-Object -ExpandProperty Count

    # What modules are loaded?  Available?
    Get-Module
    Get-Module -ListAvailable

    # Get help for a command
    Get-Help Invoke-RestMethod -Full

    # See what type / members come back from Get-Process
    Get-Process | Get-Member

# Explore!
    dir ENV:
    $ENV:PSMODULEPATH -split ":"

    Get-Variable
    Get-Variable -Name Is*
    $PSVersionTable
    $PROFILE | Select *

    # New Built-In Variables
    $isLinux
    $isWindows
    $IsOSX
    $IsCoreCLR
```

See [the Boston PowerShell User Group repo](https://github.com/BosPSUG/PresentationMaterials) for more on how this was used

## Caveats

* Note on Windows: Initial testing on Windows 8.1 with Docker Machine appeared to be unusable.  It "Worked", but was glitchy.  Very glitchy.  The newer Docker option for Windows 10 might work better.

* Note in general: This isn't an optimized image:
  * I pull down a bunch of dependencies, more than needed
  * I create a random virtual environment and place you in that path
  * Several other dockerfiles are more interesting, and more optimized:
    * [davidobrien/centos_ansible_powershell](https://hub.docker.com/r/davidobrien/centos_ansible_powershell/~/dockerfile/)
    * [trevorsullivan/powershell](https://hub.docker.com/r/trevorsullivan/powershell/)
    * [manojlds/powershell](https://hub.docker.com/r/manojlds/powershell/)
