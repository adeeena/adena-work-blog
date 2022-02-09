---
title: 'Logging levels - Logback - rule-of-thumb to assign log levels'
date: '11:27 08-08-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I'm using `logback` in my current project.

It offers six levels of logging: `TRACE` `DEBUG` `INFO` `WARN` `ERROR` `OFF`

I'm looking for a rule of thumb to determine the log level for common activities. For instance, if a thread is locked, should the log message be set to the debug level or the info level. Or if a socket is being used, should its specific id be logged at the debug level or the trace level.

I will appreciate answers with more examples for each logging level.

===

I mostly build large scale, high availability type systems, so my answer is biased towards looking at it from a production support standpoint; that said, we assign roughly as follows:

### Error
The system is in distress, customers are probably being affected (or will soon be) and the fix probably requires human intervention. The "2AM rule" applies here - if you're on call, do you want to be woken up at 2AM if this condition happens? If yes, then log it as "error".

### Warn
An unexpected technical or business event happened, customers may be affected, but probably no immediate human intervention is required. On call people won't be called immediately, but support personnel will want to review these issues asap to understand what the impact is. Basically any issue that needs to be tracked but may not require immediate intervention.

### Info
Things we want to see at high volume in case we need to forensically analyze an issue. System lifecycle events (system start, stop) go here. "Session" lifecycle events (login, logout, etc.) go here. Significant boundary events should be considered as well (e.g. database calls, remote API calls). Typical business exceptions can go here (e.g. login failed due to bad credentials). Any other event you think you'll need to see in production at high volume goes here.

### Debug
Just about everything that doesn't make the "info" cut... any message that is helpful in tracking the flow through the system and isolating issues, especially during the development and QA phases. We use "debug" level logs for entry/exit of most non-trivial methods and marking interesting events and decision points inside methods.

### Trace
We don't use this often, but this would be for extremely detailed and potentially high volume logs that you don't typically want enabled even during normal development. Examples include dumping a full object hierarchy, logging some state during every iteration of a large loop, etc.

As or more important than choosing the right log levels is ensuring that the logs are meaningful and have the needed context. For example, you'll almost always want to include the thread ID in the logs so you can follow a single thread if needed. You may also want to employ a mechanism to associate business info (e.g. user ID) to the thread so it gets logged as well. In your log message, you'll want to include enough info to ensure the message can be actionable. A log like " `FileNotFound exception caught`" is not very helpful. A better message is "`FileNotFound exception caught while attempting to open config file: /usr/local/app/somefile.txt. userId=12344.`"

There are also a number of good logging guides out there... for example, here's an edited snippet from `JCL (Jakarta Commons Logging)`:

* error - Other runtime errors or unexpected conditions. Expect these to be immediately visible on a status console.
* warn - Use of deprecated APIs, poor use of API, 'almost' errors, other runtime situations that are undesirable or unexpected, but not necessarily "wrong". Expect these to be immediately visible on a status console.
* info - Interesting runtime events (startup/shutdown). Expect these to be immediately visible on a console, so be conservative and keep to a minimum.
* debug - detailed information on the flow through the system. Expect these to be written to logs only.
* trace - more detailed information. Expect these to be written to logs only.

### Mirror from
[StackOverflow - Logging levels - Logback - rule-of-thumb to assign log levels - https://stackoverflow.com/questions/7839565/logging-levels-logback-rule-of-thumb-to-assign-log-levels/8021604#8021604](https://stackoverflow.com/questions/7839565/logging-levels-logback-rule-of-thumb-to-assign-log-levels/8021604#8021604)