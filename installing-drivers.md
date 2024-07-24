# How to install major Windows driver in minor Windows through pnputil.exe

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

## Update

Instead of the Command Prompt, I have tried the Windows PowerShell as instructed [here](https://superuser.com/questions/1420011/how-do-i-install-drivers-silently-with-pnputil-exe).

<blockquote>

Execute PowerShell as administrator and perform the following steps.

1. Navigate to your driver's folder containing the INF file through the `cd` command.
2. Run `$signature = Get-AuthenticodeSignature <certificate filename>`, where the certificate filename might be a file with the **.cat** extension; so you obtain the driver's certificate in your PowerShell session.

Now run these commands to install your driver:

```
$store = Get-Item -Path Cert:\LocalMachine\TrustedPublisher
$store.Open("ReadWrite")
$store.Add($signature.SignerCertificate)
$store.Close()
PnPutil.exe -i -a <INF filename>
```

</blockquote>

I get this error at the `Add()` call:

```
Exception on calling "Add" with "1" argument: "The X509 certificate repository was not open."
Linha:1 character:1
+ $store.Add($signature.SignerCertificate)
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : CryptographicException
```

## Answer

The DevCon command can silently install the drivers without the certificate, so you can forget the `pnputil.exe` command in this case. Note that DevCon does not come within your Windows installation; see first [Where can I download DevCon?](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/devcon#where-can-i-download-devcon), as it comes within a component of a Microsoft kit.

Assuming you have `devcon.exe` in your local environment, follow these steps:
