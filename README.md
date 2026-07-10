<div align="center">

# 🦊 Kitsune

**The platform that powers [SuMo — support.mozilla.org](https://support.mozilla.org)**

Kitsune is the open-source [Django](http://www.djangoproject.com/) application behind Mozilla's
support site: knowledge base, community Q&A, forums, and localization for 100+ locales.

![Status: Sustain](https://img.shields.io/badge/Status-Sustain-green)
[![Docs](https://img.shields.io/badge/docs-mozilla.github.io%2Fkitsune-blue)](https://mozilla.github.io/kitsune/)
[![Framework: Django](https://img.shields.io/badge/framework-Django-092E20?logo=django&logoColor=white)](http://www.djangoproject.com/)
[![Code of Conduct](https://img.shields.io/badge/code%20of%20conduct-Mozilla%20CPG-orange)](./CODE_OF_CONDUCT.md)

[Documentation](https://mozilla.github.io/kitsune/) ·
[Contribute](https://mozilla.github.io/kitsune/contributors) ·
[Dev setup](https://mozilla.github.io/kitsune/hacking_howto/) ·
[Report an issue](https://bugzilla.mozilla.org/enter_bug.cgi?product=support.mozilla.org)

</div>

---

## Contents

- [About](#about)
- [Getting started](#getting-started)
- [Releasing a new version](#releasing-a-new-version)
- [Contributing](#contributing)
- [Reporting issues](#reporting-issues)
- [Code of Conduct](#code-of-conduct)
- [Contributors](#contributors)

## About

Kitsune is a [Django](http://www.djangoproject.com/) application that powers
[SuMo (support.mozilla.org)](https://support.mozilla.org). Full documentation lives
online at **[mozilla.github.io/kitsune](https://mozilla.github.io/kitsune/)**.

## Getting started

The fastest path from clone to a running local instance is documented in the
**[developer setup guide](https://mozilla.github.io/kitsune/hacking_howto/)**. For everything
else — architecture, APIs, localization, and day-to-day workflows — see the
**[full documentation](https://mozilla.github.io/kitsune/)**.

> [!TIP]
> You can access the staging site at <https://support.allizom.org/>.

## Releasing a new version

1. **Create a [signed tag](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)** for the
   new version, following [semantic versioning](https://semver.org/).

   Given a version number `MAJOR.MINOR.PATCH`, increment the:

   | Part      | When you…                                              |
   | --------- | ------------------------------------------------------ |
   | **MAJOR** | make incompatible API changes                          |
   | **MINOR** | add functionality in a backward-compatible manner      |
   | **PATCH** | make backward-compatible bug fixes                     |

   Example:

   ```bash
   git tag -s 1.0.1 -m "Bump version: 1.0.0 to 1.0.1"
   ```

2. **Draft a new release in GitHub** for the new tag. Document the **highlights** of the release,
   and use the option to automatically document the release through the commit history.

3. **Trigger the release** for the specified tag in the deploy repository.

## Contributing

See our [contribution guide](https://mozilla.github.io/kitsune/contributors), or dive straight into
[setting up your development environment](https://mozilla.github.io/kitsune/hacking_howto/).

## Reporting issues

We use [Bugzilla](https://bugzilla.mozilla.org/enter_bug.cgi?product=support.mozilla.org) for
submitting and prioritizing issues.

## Code of Conduct

By participating in this project, you're agreeing to uphold the
[Mozilla Community Participation Guidelines](https://www.mozilla.org/en-US/about/governance/policies/participation/).
If you need to report a problem, please see our [Code of Conduct](./CODE_OF_CONDUCT.md) guide.

## Contributors

Thanks to all of our contributors ❤️

<a href="https://github.com/mozilla/kitsune/contributors">
  <img src="https://contrib.rocks/image?repo=mozilla/kitsune" alt="Kitsune contributors" />
</a>
