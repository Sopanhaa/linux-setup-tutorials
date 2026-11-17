<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->

# Licensing

Copyright (c) 2026 Sopanhaa and contributors.

Linux Setup Tutorials uses two licenses. The license depends on the type of
file, not on where the file is stored.

| Material | License | SPDX identifier | License text |
|---|---|---|---|
| Documentation: Markdown pages, diagrams, images, and other written content | [Creative Commons Attribution-ShareAlike 4.0 International](CC-BY-SA-4.0.txt) | `CC-BY-SA-4.0` | `CC-BY-SA-4.0.txt` |
| Scripts: shell, Python, and other executable code; configuration files | [MIT License](MIT.txt) | `MIT` | `MIT.txt` |

The root [LICENSE](../LICENSE) is a byte-identical copy of
`CC-BY-SA-4.0.txt`, kept for GitHub's license detection. `LICENSES/` is the
authoritative license directory.

This is not a dual-license choice for a single file. Each file is covered by
the license for its content type, unless the file carries a different SPDX
notice.

## License texts

The license texts in this directory are the official texts. The MIT text
includes the project copyright line in place of the placeholder.

## Documentation

All documentation is licensed under CC BY-SA 4.0. You may share and adapt it,
including translations, if you:

- give appropriate credit to Linux Setup Tutorials and its contributors;
- link to the license;
- indicate whether you changed the material; and
- share your adaptation under the same license.

The full legal text is in [CC-BY-SA-4.0.txt](CC-BY-SA-4.0.txt). The
human-readable summary is at
<https://creativecommons.org/licenses/by-sa/4.0/>.

## Scripts and configuration

All scripts and configuration files are licensed under the MIT License. You
may use, copy, modify, merge, publish, distribute, sublicense, and sell them,
provided the copyright notice and permission notice are included.

The full license text is in [MIT.txt](MIT.txt). The SPDX license page is at
<https://spdx.org/licenses/MIT.html>.

## SPDX identifiers

Use the SPDX identifier that matches the file type.

Shell scripts should put the identifier on the line after the shebang:

```bash
#!/usr/bin/env bash
# SPDX-License-Identifier: MIT
# Copyright (c) 2026 Sopanhaa and contributors
```

Documentation may use an HTML comment:

```markdown
<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->
```

Existing documentation does not need a per-file header. The repository-level
notice in this file and in [COPYING](../COPYING) applies.

## Contributions

By submitting a contribution, you confirm that you have permission to submit
it and agree to license it under the license for that type of content. You
keep any copyright you hold in your contribution.

- Documentation contributions are accepted under CC BY-SA 4.0.
- Script and configuration contributions are accepted under the MIT License.

No Contributor License Agreement or Developer Certificate of Origin is
required.

See [CONTRIBUTING.md](../CONTRIBUTING.md) before opening a pull request.

## Third-party material

Commands, screenshots, logos, and text from other projects remain under their
own licenses. Do not add third-party material unless its license allows reuse
in this project, and record the source and license in the page or pull
request.

## GitHub license detection

GitHub may display only the root `LICENSE` (CC BY-SA 4.0). It does not
automatically detect or display this per-file split. `COPYING`, this file, and
the SPDX headers define the actual licensing scope.

## Relationship to the Linux kernel

This is an independent project. It is not affiliated with or endorsed by the
Linux kernel project. The Linux kernel's GPL-2.0-only licensing and its
syscall exception do not apply to this project.
