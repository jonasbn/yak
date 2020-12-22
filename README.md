# Yak

This repository is the overall Yak project repository, it's goal is to specify the elements of the Yak project.

The Yak tool is a tool aimed at assisting in maintaining several repositories. It is a supplement to tools like [Dependabot].

Yak however is not language specific, but is aimed a assisting with all of the files, which are not related to a certain language.

Yak can help you to check if:

- A certain file is present
- A certain file holds specific contents based on a checksum
- A certain file holds specific contents based on a template

In addition to the above main use-cases, Yak can be configured to suit the basic needs for handling repository contents.

A key example:

> Make sure your repository contains a Code of Conduct

1. Add a file named `.yaksums.json` to your repository
1. Add the following below contents

```json
{
    "CODE_OF_CONDUCT.md": true
}
```

Running Yak on your project now checks for the contents of a `CODE_OF_CONDUCT.md`, like so:

```
❗️CODE_OF_CONDUCT.md failed
```

When you add the `CODE_OF_CONDUCT.md` a new run will report a success:

```
👍🏻./CODE_OF_CONDUCT.md succeeded
```

### About the project

It will only hold the specification but will link to relevant resources and implementations.

## Data File

## Configuration

## Implementations

1. [Perl implementation: App::Yak](https://github.com/jonasbn/perl-app-yak)

### Additional Yak Tools

1. [The GitHub Yak Action](https://github.com/jonasbn/github-action-yak), based on the Perl implementation: App::Yak

## License and Copyright

Yak and related works are (C) by Jonas B., (jonasbn) 2018-2020

[Image](https://unsplash.com/photos/3b3O75X0Jzg) used on the website is under copyright by [Shane Aldendorff](https://unsplash.com/@pluyar)

## Resources and References

- [Dependabot]
- [JSON Schema](https://json-schema.org/)
- [YAML](https://yaml.org/)

[Dependabot]: https://dependabot.com/
