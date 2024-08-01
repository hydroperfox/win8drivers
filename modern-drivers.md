# Installing modern drivers in a previous Windows version

## Case 1

I am in dual boot between Windows 11 (originally a Windows 10) and Windows 8.1. I have copied my Windows 11 drivers into a flash drive and passed them to the Windows 8.1 Downloads folder.

There are several reasons I am doing this:

- I need Wireless internet, brightness adjusting, and touchpad.
- I like Windows 8.1 more than Windows 11.
- I do like Windows 10 more than Windows 11, but I still like 8 more.
- The only thing I like in Windows 11 is <kbd>Prt Sc</kbd> slicing the screen; nothing else.

> I did this through `dism` as instructed in an internet tutorial:
>
> ```
> dism /online /export-driver /destination:<folder path>
> ```
>
> It has also been instructed with an alternative command:
>
> ```
> pnputil /export-driver * <folder path>
> ```

When I execute Command Prompt as administrator, navigate to the folder `C:\Users\<user ID>\Downloads\My Drivers\<driver INF folder>`, and run the following `pnputil.exe` command to install a driver related to "atheros":

```
pnputil.exe -i -a atheros_bth.inf
```

I get this:

```
PnP Utility from Microsoft

Processing information:            atheros_bth.inf
Failure in driver package addition: The software conformity in relation to the Windows logotype criteria were tested in a different Windows version, and might not be compatible with this version.

Total attempts:              1
Number of successful imports: 0
```

I would like to know if there is a way to pass through this error.

## Case 2

Instead of the Command Prompt, I have tried the Windows PowerShell as instructed [here](https://superuser.com/questions/1420011/how-do-i-install-drivers-silently-with-pnputil-exe).

<blockquote>

Execute PowerShell as administrator and perform the following steps.

1. Run

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

2. Change to your driver's folder containing the INF and CAT (certificate) files through the `cd` command.

3. Run:

```
$signature = Get-AuthenticodeSignature <CAT certificate filename>
$store = Get-Item -Path Cert:\LocalMachine\TrustedPublisher
$store.Open("ReadWrite")
$store.Add($signature.SignerCertificate)
$store.Close()
PnPutil.exe -i -a <INF filename>
```

</blockquote>

## Disable driver signature enforcement

To disable driver signature enforcement, access the Recovery > Advanced Settings > Advanced Startup Options in the Boot menu.

<!--

## DevCon

* https://community.spiceworks.com/t/force-install-drivers-via-powershell/719454/6
* https://ss64.com/nt/devcon.html

The DevCon command can silently install the drivers without the CAT certificate (in case it is broken), so you can use it instead of the `pnputil.exe` command in this case.

<!--
Note that DevCon does not come within your Windows installation; see first [Where can I download DevCon?](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/devcon#where-can-i-download-devcon), as it comes within certain Microsoft products.

When installing the Microsoft products containing DevCon, you will, in order (according to [learn.microsoft.com](https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)):

* Install [Microsoft .NET Framework 4.6](https://www.microsoft.com/pt-br/download/details.aspx?id=48137)
* Install Visual Studio 2022 (the [Community](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=17) edition for example)
* Install [Windows SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* Install the [Windows Driver Kit (WDK)](https://go.microsoft.com/fwlink/?linkid=2272234)

-->

[Quick method to install DevCon.exe](https://superuser.com/questions/1002950/quick-method-to-install-devcon-exe)

Assuming you already have `devcon.exe` in your local environment, follow these steps:

- ...

-->
