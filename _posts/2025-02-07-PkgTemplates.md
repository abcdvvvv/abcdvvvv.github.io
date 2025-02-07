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
    # Your GitHub username, used to generate the repository link
    user="abcdvvvv",
    # List of author information (format: "name <email>")
    authors=["karei <abcdvvvv@gmail.com>"],
    # Specify the directory where the generated package is stored
    dir="D:\\packages",
    # Specify the minimum Julia version required by the package
    julia=v"1.10",
    # Configure multiple plugins to customize various aspects of the package
    plugins=[
        ProjectFile(; version=v"1.0.0-DEV"), # Creates a Project.toml.
        # version::VersionNumber: The initial version of created packages.
        Tests(),
        Readme(),
        GitHubActions(; windows=true, extra_versions=["1.10", "1.11", "pre"]),
        TagBot(),
        CompatHelper(), # Automatically check for dependent updates and generate PRs
        Documenter{GitHubActions}(), # Add GitHub Actions-based online documentation build support for packages
        Logo(), # Logo information for documentation.
        BlueStyleBadge(),
        ColPracBadge(),
        PkgEvalBadge(),
        Formatter(), # Create a .JuliaFormatter.toml file
        PkgBenchmark(),# Sets up a PkgBenchmark.jl benchmark suite.
        # To ensure benchmark reproducibility, you will need to manually create an environment in the benchmark subfolder (for which the Manifest.toml is committed to version control). In this environment, you should at the very least: 
        # pkg> add BenchmarkTools  pkg> dev your new package
    ]
)
```

Use the template to generate a new package, and name the package "MyNewPackage.jl".
```julia
generate(t, "MyNewPackage.jl")
```
