# Licensing

Copyright (c) 2026 Sopanhaa and contributors.

Linux Setup Tutorials uses two licenses. The license depends on the type of
file, not on where the file is stored.

| Material | License | SPDX identifier |
|---|---|---|
| Documentation: Markdown pages, diagrams, images, and other written content | [Creative Commons Attribution-ShareAlike 4.0 International](LICENSE) | `CC-BY-SA-4.0` |
| Scripts: shell, Python, and other executable code; configuration files | [MIT License](LICENSES/MIT.txt) | `MIT` |

This is not a dual-license choice for a single file. Each file is covered by
the license for its content type, unless the file carries a different SPDX
notice.

## Documentation

All documentation is licensed under CC BY-SA 4.0. You may share and adapt it,
including translations, if you:

- give appropriate credit to Linux Setup Tutorials and its contributors;
- link to the license;
- indicate whether you changed the material; and
- share your adaptation under the same license.

The full legal text is in [LICENSE](LICENSE). The human-readable summary is at
<https://creativecommons.org/licenses/by-sa/4.0/>.

## Scripts and configuration

All scripts and configuration files are licensed under the MIT License. You
may use, copy, modify, merge, publish, distribute, sublicense, and sell them,
provided the copyright notice and permission notice are included.

The full license text is in [LICENSES/MIT.txt](LICENSES/MIT.txt). The SPDX
license page is at <https://spdx.org/licenses/MIT.html>.

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
notice in this file and in [LICENSE](LICENSE) applies.

## Contributions

By submitting a contribution, you agree to license it under the license for
that type of content. You keep any copyright you hold in your contribution.

- Documentation contributions are accepted under CC BY-SA 4.0.
- Script and configuration contributions are accepted under the MIT License.

See the [contributing section](README.md#contributing) before opening a pull
request.

## Third-party material

Commands, screenshots, logos, and text from other projects remain under their
own licenses. Do not add third-party material unless its license allows reuse
in this project, and record the source and license in the page or pull
request.

## Relationship to the Linux kernel

This is an independent project. It is not affiliated with or endorsed by the
Linux kernel project. The Linux kernel's `GPL-2.0 WITH Linux-syscall-note`
license and its kernel-specific exceptions do not apply to this project.
