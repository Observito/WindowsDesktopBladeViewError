# WindowsDesktopBladeViewError

Reproduction of error when using the BladeView control (Windows Community Toolkit ) and the UWP application uses TargetDeviceFamily="Desktop".

To make the exception disappear, either remove the BladeView control from the MainPage.xaml file or change the TargetDeviceFamily back to Windows.Universal in the Package.appxmanifest file.

## Diagnostics

Exception:

```csharp
Windows.UI.Xaml.Markup.XamlParseException
  HResult=0x802B000A
  Message=The text associated with this error code could not be found.

Cannot find a Resource with the Name/Key AppBarButtonBackground [Line: 14 Position: 50]
  Source=Windows
  StackTrace:
   at Windows.UI.Xaml.Application.LoadComponent(Object component, Uri resourceLocator, ComponentResourceLocation componentResourceLocation)
   at WindowsDesktopBladeViewError.MainPage.InitializeComponent() in C:\Users\Bent Rasmussen\source\repos\WindowsDesktopBladeViewError\WindowsDesktopBladeViewError\obj\x86\Debug\MainPage.g.i.cs:line 33
   at WindowsDesktopBladeViewError.MainPage..ctor() in C:\Users\Bent Rasmussen\source\repos\WindowsDesktopBladeViewError\WindowsDesktopBladeViewError\MainPage.xaml.cs:line 27
```

Error without the debugger attached:

```
---------------------------
Microsoft Visual Studio
---------------------------
Unable to activate Windows Store app 'a24be30d-3376-40f2-a8fe-50895707a31e_jjkatmj1dg54t!App'. The activation request failed with error 'Operation not supported. Unknown error: 0x80040904'.



See help for advice on troubleshooting the issue.
---------------------------
OK   Help   
---------------------------
```

As an aside, copying the project folder to a new folder (to prepare this repo), meant the following VS error:

```chsharp
Unhandled exception at 0x762EC762 (KernelBase.dll) in WindowsDesktopBladeViewError.exe: 0xE0434352 (parameters: 0x80070005, 0x00000000, 0x00000000, 0x00000000, 0x6DDC0000). occurred
```

That error was fixed by removing the deployed application, which then oddly gave this less detailed (call-stackless) exception instead:

```chsharp
Exception thrown at 0x762EC762 (KernelBase.dll) in WindowsDesktopBladeViewError.exe: WinRT originate error - 0x80004005 : 'Cannot find a Resource with the Name/Key AppBarButtonBackground'. occurred
```

## "Responsible" XAML

```xaml
<Page
    x:Class="WindowsDesktopBladeViewError.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WindowsDesktopBladeViewError"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <!-- NOTE: remove use of this control to make the error disappear -->
        <controls:BladeView>
            <controls:BladeItem Header="Blade 1">
            </controls:BladeItem>
        </controls:BladeView>

    </Grid>
</Page>
```
