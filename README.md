# chezmoi-firefox-setup

Note that if your installation method puts Firefox itself in a dynamically named directory, this will probably not work as is. For example, [scoop](https://scoop.sh/) on Windows installs applications under directories named after the version of the application (such as `path/to/firefox/91.0.2/firefox.exe` etc.), then creates a junction directory named `current` which points to the latest version. I don't think Firefox has any way of knowing about the junction so the install ID is based off the directory with the version as its name (and so is always changing). You could probably get around this by creating a script that simply looks in the Firefox install directory and outputs the full path (with the correct platform path separators) with the highest version number, and using this with an `output` in a template. Something like:

```ini
{{- if eq .chezmoi.os "windows" -}}
{{-   $releaseId = mozillaInstallHash (output "path/to/get-latest-firefox-release-version-path-script") -}}
{{-   $nightlyId = mozillaInstallHash (output "path/to/get-latest-firefox-nightly-version-path-script") -}}
```