# SEEK Ltd. PowerShell Desired State Configuration Resources

This project includes custom DSC resources used by SEEK Ltd.

## Setup

After cloning this repository, it is necessary to perform a once-off setup. The setup will setup symlinks in the PowerShell Module path to the custom DSC resources.

### Why?

The PowerShell DSC configuration is applied by the Local Configuration Manager (LCM). The LCM runs as the SYSTEM user, so any custom resources must reside in the SYSTEM user's `$env:PSModulePath` (either `$env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules` or `$env:ProgramFiles\WindowsPowerShell\Modules`)

### Pre-requisites

- Git client
- [Windows Management Framework 4.0](http://www.microsoft.com/en-au/download/details.aspx?id=40855) required to provide PowerShell 4 and necessary cmdlets
- Your user must have local administrator privileges
- This project must be cloned to the local machine
- [.Net Framework 4](http://www.microsoft.com/en-au/download/details.aspx?id=17718) required to run build

```
PS> git clone https://github.com/SEEK-Jobs/DSC.git
```

### How?

Simply run the setup script from the project (as administrator):

```
PS> cd DSC
PS> .\Setup.ps1
```

You should now see some modules in the PowerShell Module path. For example:

```
PS> ls $env:ProgramFiles\WindowsPowerShell\modules


    Directory: C:\Program Files\WindowsPowerShell\Modules


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        26/05/2014   3:57 PM            SEEK - Modules
```

## Creating Custom DSC Resources

Please see the Writing a Custom DSC Resource chapter in [The DSC book](http://powershell.org/wp/ebooks/).

## Testing

If you wish to contribute to the SEEK DSC resources, please ensure you run the tests before sending a pull request. Tests are run using [Pester](https://github.com/pester/Pester) (a BDD test framework for PowerShell). The Pester package is included under the `Tools` directory. Tests can be found under the `Tests` directory.

The following command will run the full suite of tests:

```
.\build.bat /t:Test
```

### Unit tests:

Unit tests are the most important and test the DSC resource contract. Namely the `Get-TargetResource`, `Set-TargetResource` and `Test-TargetResource` functions. The unit tests should run in isolation and mock-out any interactions with external commands or the network.

NOTE: if cmdlets cannot be mocked directly, it is acceptable to partially mock the Script Under Test (SUT). This can be achieved by wrapping the cmdlet call in a function and simply mocking the wrapper function.

To run just the unit tests, use the following command:

```
.\build.bat /t:UnitTest
```

### Integration tests:

Itegration tests are not absolutely necessary. They are however useful to get some extra confidence that your wrapper functions interact with external commands correctly.

Run the integration tests using the following command:

```
.\build.bat /t:\TntegrationTest
```

### E2E tests:

Each module should have one end-to-end test. This ensures that a DSC configuration can import and utilise the custom resource. There is no other way to test that the resource schema matches the `Get-TargetResource` and `Set-targetResource` parameters. E2E tests are expensive so do not use E2E tests for exhaustive scenarios, they should simply test a single happy-days scenario.

To run the E2E tests, run the following command:

```
.\build.bat /t:E2ETest
```