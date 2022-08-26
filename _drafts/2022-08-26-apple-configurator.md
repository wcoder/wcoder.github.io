---
layout: post
title: Apple Configurator
date: 2022-08-26 12:49
locale: en_US
tags:
- apple
- ios
- заметка
---

Apple Configurator stores their `.ipsw` archives in this path:

```
~/Library/Group\ Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Firmware/
```

Files look like this:

```
iPhone13,2,iPhone13,3_15.6.1_19G82_Restore.ipsw
```

iTunes Updater in Finder stores their files in this path:

```
~/Library/iTunes/iPhone\ Software\ Updates/
```

Also, you can install/update custom image via [cfgutil](https://support.apple.com/guide/apple-configurator-mac/use-the-command-line-tool-cad856a8ea58/mac) CLI:

```
cfgutil -v restore -I ~/Desktop/iPhone13,2,iPhone13,3_15.6.1_19G82_Restore.ipsw
```

Custom images you can find here: https://ipsw.me/
