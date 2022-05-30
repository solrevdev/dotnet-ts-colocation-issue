## dotnet-ts-colocation-issue


I have a `dotnet new razor` project with `Microsoft.TypeScript.MSBuild` and `BuildBundlerMinifier` added so that I can write TypeScript first, then minify the compiled JavaScript further

So, I want to be able to write the source TypeScript in the folder next to where I want the compiled JavaScript output file to eventually be. 

So for example:

```bash
input: src/Web/wwwroot/js/site.ts    # source code
output: src/Web/wwwroot/js/site.js   # compiled via typescript
then: src/Web/wwwroot/js/site.js.min # minified via bundleconfig.json
```

Also,

```bash
input: src/Web/Pages/Admin/Quotes/Index.cshtml.ts     # source code
output src/Web/Pages/Admin/Quotes/Index.cshtml.js     # compiled via typescript
then: src/Web/Pages/Admin/Quotes/Index.cshtml.js.min  # minified via bundleconfig.json
```

The project will build initially, but any subsequent builds or a `dotnet watch` will then fail.

The build output and error is as follows:

```bash
# first build will succeed as it creates the files as expected
dotnet build
Microsoft (R) Build Engine version 17.2.0+41abc5629 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.

  Bundler: Begin processing bundleconfig.json
    Minified wwwroot/css/site.min.css
    Minified wwwroot/js/site.min.js
    SourceMap wwwroot/js/site.min.js.map
    Minified Pages/Admin/Quotes/Index.cshtml.min.js
    SourceMap Pages/Admin/Quotes/Index.cshtml.min.js.map
  Bundler: Done processing bundleconfig.json
  dotnetissue -> /Users/solrevdev/Desktop/dotnetissue/bin/Debug/net6.0/dotnetissue.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:07.39

## then any subsequent files will fail

dotnet build
Microsoft (R) Build Engine version 17.2.0+41abc5629 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.

  Bundler: Begin processing bundleconfig.json
  Bundler: Done processing bundleconfig.json
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : Two assets found targeting the same path with incompatible asset kinds:  [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : '/Users/solrevdev/Desktop/dotnetissue/Pages/Admin/Quotes/Index.cshtml.js' with kind 'All' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : '/Users/solrevdev/Desktop/dotnetissue/Pages/Admin/Quotes/Index.cshtml.js' with kind 'All' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : for path 'Pages/Admin/Quotes/Index.cshtml.js' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]

Build FAILED.

/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : Two assets found targeting the same path with incompatible asset kinds:  [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : '/Users/solrevdev/Desktop/dotnetissue/Pages/Admin/Quotes/Index.cshtml.js' with kind 'All' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : '/Users/solrevdev/Desktop/dotnetissue/Pages/Admin/Quotes/Index.cshtml.js' with kind 'All' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
/usr/local/share/dotnet/sdk/6.0.300/Sdks/Microsoft.NET.Sdk.Razor/targets/Microsoft.NET.Sdk.Razor.JSModules.targets(105,5): error : for path 'Pages/Admin/Quotes/Index.cshtml.js' [/Users/solrevdev/Desktop/dotnetissue/dotnetissue.csproj]
    0 Warning(s)
    1 Error(s)

Time Elapsed 00:00:05.50
```


And here is some environment information:


.NET

```bash
dotnet --info
.NET SDK (reflecting any global.json):
 Version:   6.0.300
 Commit:    8473146e7d

Runtime Environment:
 OS Name:     Mac OS X
 OS Version:  11.6
 OS Platform: Darwin
 RID:         osx.11.0-x64
 Base Path:   /usr/local/share/dotnet/sdk/6.0.300/

Host (useful for support):
  Version: 6.0.5
  Commit:  70ae3df4a6

.NET SDKs installed:
  3.1.419 [/usr/local/share/dotnet/sdk]
  6.0.300 [/usr/local/share/dotnet/sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.App 3.1.25 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 6.0.5 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 3.1.25 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]
  Microsoft.NETCore.App 6.0.5 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]

To install additional .NET runtimes or SDKs:
  https://aka.ms/dotnet-download
```

macOS:

```bash
neofetch --off
solrevdev@macmini.local
------------------------------
OS: macOS 11.6.6 20G624 x86_64
Host: Macmini7,1
Kernel: 20.6.0
Uptime: 3 hours, 14 mins
Packages: 323 (brew)
Shell: zsh 5.8
Resolution: 2560x1440
DE: Aqua
WM: Quartz Compositor
WM Theme: Blue (Light)
Terminal: iTerm2
Terminal Font: CaskaydiaCoveNerdFontComplete-Regular 16
CPU: Intel i5-4278U (4) @ 2.60GHz
GPU: Intel Iris
Memory: 9128MiB / 16384MiB
````
