---
slug: net-core-project-restored-different-version
title: .NET Core - Project version mismatch
date: 2019-06-03
excerpt: Solving this cryptic error might take you many hours. Hopefully this will help
  you out.
tags:
  - Back-End
  - Common Errors
  - Guide
categories:
  - Tech
---

<script context="module">
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Callout from "$lib/components/molecules/Callout.svelte";
  import SparklingHighlight from "$lib/components/molecules/SparklingHighlight.svelte";

  import { getSrcsetFromImport } from "$lib/utils/functions";
  import CoverImage from './cover.jpg?width=1600&format=avif;webp;png&meta&imagetools';

  metadata.coverImage = getSrcsetFromImport(CoverImage);
</script>

This problem usually happens when you're working on a solution with multiple projects that were created using different versions of the .NET SDK.

<Callout type="error">
  The project was restored using Microsoft.NETCore.App version x.x.x, but with current settings, version y.y.y would be used instead
</Callout>

You may encounter this error when running `dotnet build` or maybe only on `dotnet publish`. In any case, the best way to get rid of this problem for good is the following:

1. On the folder where your `.sln` file resides, create a new file named `Directory.Build.props`. If you're not working with a solution (not using Visual Studio), you can use create it on the root directory of your repository (usually where `.gitignore` is;
2. With a text editor, add the following content to it:

<CodeBlock lang="xml" filename="Directory.Build.props">

```xml
<Project>
  <PropertyGroup>
    <TargetLatestRuntimePatch>true</TargetLatestRuntimePatch>
    <GenerateFullPaths>true</GenerateFullPaths>
    <LangVersion>latest</LangVersion>
  </PropertyGroup>
</Project>
```

</CodeBlock>

What these properties do is make your projects always restore and compile using the latest version available.

Try running the command again. If all went right, it should be compiling successfully now!

<Callout type="info">
  In case it doesn't work for you, you can add these lines manually on each of the `.csproj` files.
</Callout>

Thanks for reading!
