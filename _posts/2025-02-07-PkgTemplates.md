---
title: Create new package via PkgTemplates.jl
author: karei
date: 2025-02-07 00:00:00 +0000
categories: [Blogging, Julia]
tags: [tutorial, programming, julia]
pin: false
media_subpath: '/posts/20250117'
---

## Create new package via PkgTemplates.jl

```julia
using PkgTemplates
t = Template(
    user    = "Your_GitHub_username",
    authors = ["name <your_email@example.com>"],
    # Specify the directory where the generated package is stored
    dir     = "D:\\packages",
    # Specify the minimum Julia version required by the package
    julia   = v"1.10",
    # Configure multiple plugins to customize various aspects of the package
    plugins = [
        ProjectFile(version=v"0.1.0-DEV"), # Creates a Project.toml.
        # version::VersionNumber: The initial version of created packages.
        Tests(),
        Readme(),
        License(name="ASL"),
        GitHubActions(extra_versions=["1", "lts"]),
        TagBot(),
        # Automatically check for dependent updates and generate PRs
        CompatHelper(),
        # Add GitHub Actions-based online documentation build support for packages
        Documenter{GitHubActions}(),
        # Logo information for documentation.
        Logo(),
        BlueStyleBadge(),
        ColPracBadge(),
        PkgEvalBadge(),
        # Create a .JuliaFormatter.toml file
        Formatter(),
        # Sets up a PkgBenchmark.jl benchmark suite.
        PkgBenchmark(),
        # To ensure benchmark reproducibility, you will need to manually create an environment in the benchmark subfolder (for which the Manifest.toml is committed to version control). In this environment, you should at the very least:
        # pkg> add BenchmarkTools  pkg> dev your new package
    ]
)
```

Use the template to generate a new package, and name the package "MyNewPackage".
```julia
generate(t, "MyNewPackage")
```
