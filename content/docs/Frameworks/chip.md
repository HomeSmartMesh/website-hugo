---
title: "Project CHIP"
description: "Connected Home Over IP"
date: 2021-02-12T08:00:00+00:00
lastmod: 2021-02-12T08:00:00+00:00
weight: 2
toc: true
---

# Definition

We can consider the project CHIP `Connected Home Over IP` definition as in two folds :
* a takeover of the Zigbee Cluster Library to be fit over the IP layer.
* The abstraction of all different transport protocols with IP

This will avoid that each transport (e.g. BT, Zigbee) have a custom app layer. It is interesting to notice that the `Zigbee Alliance` is driving this project whereas Zigbee itself does not map to an IP layer, and therefore `Thread` is the natural transition from Zigbee as both are 802.15.4 compliant at the bottom and both will keep the same logic of the ZCL that devices are familiar with.

# Links

{{< icon_button href="https://www.connectedhomeip.com/" text="CHIP website" icon="new" >}}

{{< icon_button href="https://github.com/project-chip/connectedhomeip" text="Github Repo" icon="github" >}}

# Tools

**ZAP** : ZCL Advaned Platform

**ZCL** : Zigbee Cluster Library

{{< icon_button href="https://github.com/project-chip/zap" text="ZAP Repo" icon="github" >}}

on windows install with node `v14.15.1` failed, errors after manuall install of GTK filed an [issue](https://github.com/project-chip/zap/issues/101)

on linux the following commands succeed using node `v14.15.5` :

```bash
  npm uninstall node-pre-gyp --save
  npm install @mapbox/node-pre-gyp --save

  npm install
  npm run zap
```

{{< icon_button href="http://gladewin32.sourceforge.net/" text="GTK" icon="new" >}}

sudo apt-get install libcairo2-dev

{{<details "npm install error..." >}}
```bash
D:\Projects\zap>npm install

> zap@0.99.2 postinstall D:\Projects\zap
> electron-builder install-app-deps && husky install

  • electron-builder  version=22.9.1
  • loaded configuration  file=package.json ("build" field)
  • rebuilding native dependencies  dependencies=bufferutil@4.0.3, canvas@2.6.1, libxmljs@0.19.7, sqlite3@5.0.1, utf-8-validate@5.0.4 platform=win32 arch=x64
  ⨯ cannot execute  cause=exit status 1
                    out=
    > bufferutil@4.0.3 install D:\Projects\zap\node_modules\bufferutil
    > node-gyp-build


    > canvas@2.6.1 install D:\Projects\zap\node_modules\canvas
    > node-pre-gyp install --fallback-to-build

    Warning: Missing input files:
    C:\GTK\bin\libpng14-14.dll
    C:\GTK\bin\libexpat-1.dll
    C:\GTK\bin\libintl-8.dll
    Building the projects in this solution one at a time. To enable parallel build, please add the "-m" switch.
      Backend.cc
      ImageBackend.cc
      PdfBackend.cc
      SvgBackend.cc
      BMPParser.cc
      Backends.cc
      Canvas.cc
      CanvasGradient.cc
      CanvasPattern.cc
      CanvasRenderingContext2d.cc
      closure.cc
      color.cc
      Image.cc
      ImageData.cc
      init.cc
      register_font.cc
      win_delay_load_hook.cc
    D:\Projects\zap\node_modules\canvas\src\init.cc(14,1): fatal error C1189: #error: ("cairo v1.10.0 or later is required")
```
{{</details>}}