# Yak

This is the Yak specification v.0.1.

This repository is the overall Yak specification repository, it's goal is to outline the Yak specification.

The Yak specification outlines the requiresments for a tool aimed at assisting in maintenance across repositories. It is a supplement to tools like [Dependabot] and [Renovate], it is however not as advanced at this time, which is reflected in this specification.

Yak is not intended to be language specific, it is aimed at assisting with all of the files, which are not related to a certain language.

Yak can for example help you to check if:

- A certain file is present
- A certain file holds specific contents based on a checksum
- A certain file holds specific contents based on another file (_template_) available on the local file system (when used locally)
- A certain file holds specific contents based on another file (_template_) available via a URL

An example:

> Make sure your repository contains a Code of Conduct

1. Add a data file named `.yaksums.json` to your repository
2. Add the following below contents

```json
{
    "CODE_OF_CONDUCT.md": true
}
```

Running Yak on your repository now checks for the contents of a `CODE_OF_CONDUCT.md`, would indicate that the file is not present as required.

An example implementation:

```text
❗️CODE_OF_CONDUCT.md is not present
```

When you add the `CODE_OF_CONDUCT.md` a new run will report a success:

```text
👍🏻./CODE_OF_CONDUCT.md is present
```

If you want to be more specific about the version of your `CODE_OF_CONDUCT.md`, there are a few options available:

1. You can specify a checksum for the code of conduct, then Yak will check if the file in the repository holds the same checksum as your canonical version
2. Alternatively you can specify a path to a file in your filesystem, so the checksum is calculated dynamically

Lets start with the first option:

```json
{
    "CODE_OF_CONDUCT.md": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
}
```

A Yak check would report the following, for a file with a different checksum

```text
❗️CODE_OF_CONDUCT.md not matching
```

For a file with a similar checksum

```text
👍🏻./CODE_OF_CONDUCT.md matching
```

The checksum is a standard checksum based on SHA256.

```shell
sha256sum CODE_OF_CONDUCT.md
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855  CODE_OF_CONDUCT.md
```

The second option would look as follows:

```json
{
    "CODE_OF_CONDUCT.md": "file://CODE_OF_CONDUCT.md"
}
```

This points to a file in the configuration directory of Yak, meaning that canonical versions can easily be maintained in this directory as files and the repositories specified to this file, will report a failure if and when the file is updated and the file can easily be copied to the relevant repositories.

The path for the file has to be unique, so the configuration has to complete paths working from the root of the given repository in which

```json
{
    ".github/dependabot.yml": true
}
```

## About the project

It will only hold the specification and will link to relevant resources and implementations.

## Data File

The data file is named: `.yaksums.json`

It contains a JSON structure with the following format.

```json
{
    'filename' => boolean | checksum | filename
}
```

## Configuration

Checking a deep directory structure can be expensive so an _ignore file_ can be specified.

The file should be named: `.yakignore`.

It should follow the specification of the commonly available implementation of [git ignore][gitignore].

The ignore pattern also works from the root of the directory structure.

## Checksum

The checksum is based on SHA256.

A commonly available tool to calculate the SHA256 checksum is: `sha256sum`.

## Outcome Matrix

| Type     | File        | Success  | Failure      | Note                |
|----------|-------------|----------|--------------|---------------------|
| Boolean  | Present     | Present  |              |                     |
| Boolean  | Not Present |          | Not present  |                     |
| File     | Present     | Matching |              | file://             |
| File     | Present     |          | Not matching | file://             |
| File     | Not present |          | Not present  | file://             |
| Checksum | Present     | Matching |              | SHA256              |
| Checksum | Present     |          | Not matching | SHA256              |
| Checksum | Not present |          | Not present  | SHA256              |
| URL      | Present     | Matching |              | http:// or https:// |
| URL      | Present     |          | Not matching | http:// or https:// |
| URL      | Not present |          | Not present  | http:// or https:// |

In addition different error scenarios are left up to the implementation.

- Permission issue: File cannot be read from file system, meaning checksum cannot be calculated
- Network issue: File cannot be read from URL, meaning checksum cannot be calculated

## Implementations

1. Perl implementation: [App::Yak] this also acts as the initial prototype, this is still in alpha.

### Additional Yak Tools

1. [The GitHub Yak Action](https://github.com/jonasbn/github-action-yak), based on the Perl implementation: [App::Yak], this is still in alpha.

## License and Copyright

Yak and related works are (C) by Jonas B., (jonasbn) 2018-2022

[Image](https://unsplash.com/photos/3b3O75X0Jzg) used on the website is under copyright by [Shane Aldendorff](https://unsplash.com/@pluyar)

## Resources and References

- [Dependabot]
- [gitignore]
- [JSON.org][JSON]
- [Renovate]
- [sha256sum man page][sha256sum]

[JSON]: https://www.json.org/json-en.html
[Dependabot]: https://dependabot.com/
[App::Yak]: https://github.com/jonasbn/perl-app-yak
[gitignore]: https://git-scm.com/docs/gitignore
[sha256sum]: https://linux.die.net/man/1/sha256sum
[YAML]: https://yaml.org/
[JSON Schema]: https://json-schema.org/
[Renovate]: https://www.mend.io/free-developer-tools/renovate/
