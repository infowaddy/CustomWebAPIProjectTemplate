# ASP.NET Core Web API Template with Scalar and Serilog

A .NET 10 project template for controller-based ASP.NET Core Web APIs, with optional Scalar API documentation and Serilog logging.

## Requirements

- .NET 10 SDK
- An editor such as Visual Studio or Visual Studio Code (optional)

## Install from NuGet

```shell
dotnet new install WebAPIWithScalarAndSerilog.Templates
dotnet new WebAPIWithScalarAndSerilog -n MyWebApi -o MyWebApi
cd MyWebApi
dotnet run --launch-profile https
```

If needed, trust the local HTTPS development certificate with `dotnet dev-certs https --trust`.

Using the supplied launch profile:

- Sample endpoint: `https://localhost:7008/weatherforecast`
- Scalar UI: `https://localhost:7008/scalar`
- OpenAPI document: `https://localhost:7008/openapi/v1.json`

Scalar and OpenAPI routes are available only in Development. Use the actual application address printed in the terminal if you change the ports. Serilog writes to the console and rolling JSON files in `Logs/` under the application's working directory.

## Template options

Scalar and Serilog are both enabled by default. The supported framework is `net10.0`.

```shell
dotnet new WebAPIWithScalarAndSerilog --help
dotnet new WebAPIWithScalarAndSerilog -n BasicApi -o BasicApi --scalar false --serilog false
```

Uninstall the NuGet template package:

```shell
dotnet new uninstall WebAPIWithScalarAndSerilog.Templates
```

## Work with the source code

This repository contains the template source and the project that packages it for NuGet.

| Path | Purpose |
| --- | --- |
| `CustomWebAPIProject/` | Web API template source copied into generated projects |
| `CustomWebAPIProject/.template.config/` | Template identity, options, and IDE metadata |
| `CustomeWebAPIProjectTemplate.slnx` | Solution for the Web API source project |
| `WebAPIWithScalarAndSerilog.csproj` | NuGet template packaging project |

From the repository root, compile the template source:

```shell
dotnet build CustomeWebAPIProjectTemplate.slnx -c Release
```

To test the template, install the source folder and generate an application outside the repository:

```shell
dotnet new install ./CustomWebAPIProject
dotnet new WebAPIWithScalarAndSerilog -n TemplateDemo -o ../TemplateDemo
```

From the generated `TemplateDemo` directory:

```shell
dotnet build -c Release
dotnet run --launch-profile https
```

Use generated applications for runtime validation: template condition comments are processed by `dotnet new`, whereas building the source directly includes both conditional branches. Test all four combinations of `--scalar true/false` and `--serilog true/false` when changing conditional code.

To remove the local folder installation, run `dotnet new uninstall` to find its registered path, then run `dotnet new uninstall <registered-path>`.

## Build the NuGet package

From the repository root:

```shell
dotnet pack WebAPIWithScalarAndSerilog.csproj -c Release -o artifacts
```

The `.nupkg` is written to `artifacts/`. At the current package version, test it with:

```shell
dotnet new install ./artifacts/WebAPIWithScalarAndSerilog.Templates.1.0.0.nupkg
```

Uninstall a conflicting folder or package installation first if the CLI reports an existing template. Building the solution does not build the NuGet package; use the packaging command above.

## Before publishing to GitHub or NuGet

Resolve the remaining findings recorded in `CODE_REVIEW.md`: package exclusions and incomplete license files. The Program.cs error-handling and startup-logging findings have been fixed. The sample API has no authentication; add the access controls required by your application before deploying it.

The repository `.gitignore` excludes build output, local IDE settings, logs, and review artifacts. Inspect staged files before committing, keep credentials out of configuration, and inspect the actual `.nupkg` contents before release. Git exclusions do not control NuGet package contents.

## License

The package declares MIT licensing and the original README identifies Copyright (c) 2026 Zin Min. Before publishing, populate the empty root `LICENSE.txt` and replace the placeholders in `CustomWebAPIProject/LICENSE.txt` with the intended notice.

Third-party dependencies are covered by their respective licenses.

