---
layout: default
title: Iconv Basic
---

# Iconv Basic

## Useful Commands

```bash
# MSVC build log convert
iconv -c -f ucs-2le -t ASCII BuildLog.htm > BuildLog.err
 
# big5 to gb
iconv -c -f BIG5 -t GB18030 "$file" > "$file.cov"
```
