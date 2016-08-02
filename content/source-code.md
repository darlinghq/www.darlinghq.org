---
title:	"Source Code"
---
# Source Code

Source code implemented as part of Darling is released under the terms of [GNU General Public License version 3](https://www.gnu.org/licenses/gpl.html). It is available for download on [GitHub](https://github.com/darlinghq/darling).

Like OS X, Darling includes **MANY** third party components. These are typically licensed under APSL, GPLv2, LGPL, BSD and similar licencies.

```
git clone --recursive https://github.com/darlinghq/darling.git
```

See the [build instructions](/build-instructions).

### darling-dmg <a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=DarlingDmg_Build&#038;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:DarlingDmg_Build)/statusIcon"/></a>

darling-dmg is a component of Darling used for mounting DMG disk images (with an HFS+ filesystem) through FUSE. It can be used standalone, without the rest of Darling, or even as a library in your own project.

For more information, visit the [project homepage at GitHub](https://github.com/darlinghq/darling-dmg).
