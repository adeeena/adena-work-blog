---
title: 'Bash - Piping command output to tee but also save exit code of command'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
media_order: brina-blum-wJglNsvS9jM-unsplash.jpg
hero_image: brina-blum-wJglNsvS9jM-unsplash.jpg
---

I have a shell script in which I wrap a command (`mvn clean install`), to redirect the output to a logfile.

>     #!/bin/bash
>     ...
>     mvn clean install $@ | tee $logfile
>     echo $? # Does not show the return code of mvn clean install

Now if `mvn clean install` fails with an error, I want my wrapper shell script also fail with that error. But since I'm piping all the output to `tee`, I cannot access the return code of `mvn clean install`, so when I access `$?` afterwards, it's always 0 (since tee successes).

I tried letting the command write the error output to a separate file and checking that afterwards, but the error output of mvn is always empty (seems like it only writes to stdout).

How can I preserve the return code of `mvn clean install` but still piping the output to a logfile?

===

You can set the `pipefail` shell option option on to get the behavior you want.

From the Bash Reference Manual:

>    The exit status of a pipeline is the exit status of the last command in the pipeline, unless the `pipefail` option is enabled (see The Set Builtin). If `pipefail` is enabled, the pipeline's return status is the value of the last (rightmost) command to exit with a non-zero status, or zero if all commands exit successfully.

Example:

>     $ false | tee /dev/null ; echo $?
>     0
>     $ set -o pipefail
>     $ false | tee /dev/null ; echo $?
>     1

To restore the original pipe setting:

>     $ set +o pipefail


