<!--
   Copyright 2012 The Android Open Source Project

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->

Tags represent one of the metrics the data usage counters will be
tracked against. By default, and implicitly, a tag is just based on
the UID. The UID is used as the base for policing, and cannot be
ignored. So a tag will always at least represent a UID (uid_tag). A
tag can be explicitly augmented with an "accounting tag" which is
associated with a UID. User space can use
`TrafficStats.setThreadStatsTag()` to set the acct_tag portion of the
tag which is then used  with sockets: all data belonging to that
socket will be counted against the tag. The policing is then based on
the tag's uid_tag portion, and stats are collected for the acct_tag
portion separately.

Without explicit tagging, the qtaguid module will assume the
`default_tag:  {acct_tag=0, uid_tag=10003}`

        a:  {acct_tag=1, uid_tag=10003}
        b:  {acct_tag=2, uid_tag=10003}
        c:  {acct_tag=3, uid_tag=10003}

`a, b, c…` represent explicit tags associated with specific sockets.

`default_tag (acct_tag=0)` is the default accounting tag that contains
the total traffic for that uid, including all untagged
traffic, and is typically used to enforce policing/quota rules.

These tags can be used to profile the network traffic of an
application into separate logical categories (at a network socket
level). Such tags can be removed, reapplied, or modified during
runtime.

The qtaguid module has been implemented on [kernel/common branch of
android-3.0](https://android-review.googlesource.com/#/q/project:kernel/common+branch:android-3.0,n,z)
