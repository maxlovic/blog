---
title: "Collect Periodic Top Stats"
date: 2024-12-10
draft: false
category: "Scripts"
tags: ["CPU Usage", "top"]
mathjax: false
lead: "To run in an absense of python"
comments: true
---

A script to collect the output of `top -H -p <pid>` periodically and write it to a file.


<!--more-->


## The Script

The shell script expects two arguments as an input: process name and the output file.
It checks if there is a process with a name matching the pattern to find out its Process ID automatically
to use in a `top -H -p <pid>` command.
The `top -H -p <pid>` command will be run every 1 second (depends on the accuracy of `Sleep 1` command).


```shell
#!/bin/bash
# SPDX-License-Identifier: MPL-2.0
# Author: Maxim Sharabayko
# Usage: ./collect-top.sh <process_name> <output>
echo "Target process: $1"
echo "Cron output file: $2"

# Find the process ID of srt-xtransmit*
pid=$(ps -a | grep $1 | awk '{print $1}')

# Check if the process ID was found
if [ -z "$pid" ]; then
  echo "Process $1 not found."
  exit 1
fi

echo "Process ID: $pid"
rm -f $1
while true; do sleep 1 && top -H -b -p $pid -n1 ; done >> $2

```

Example command line would be:

```
./collect-top.sh <process_name> <output>
```


