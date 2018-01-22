---
title: DetectX Module
permalink: "/docs/modules/detectx/"
---

This module pulls data from DetectX Swift's results.json file. It does not **run** DetectX, admins are responsible for creating their own launch daemon that runs DetectX and outputs the results to:

```
/usr/local/munki/preflight.d/cache/detectx.json
```

- This requires a Management License, $299 (site license) from [Sqwarq.com](https://sqwarq.com).
- Flags needed:
1. `search` - Starts a search
2. `-a` - Searches all accounts, required to be run as `root` or via `sudo`
3. `-j /usr/local/munki/preflight.d/cache/detectx.json` - Outputs results data as a `JSON` file.


Example Launch Daemon:
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>DetectX All User Run</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Applications/DetectX Swift.app/Contents/MacOS/DetectX Swift</string>
    <string>search</string>
    <string>-aj</string>
    <string>/usr/local/munki/preflight.d/cache/detectx.json</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
</dict>
</plist>
```


# [Return to Module List](../../configuration/modulelist/)
