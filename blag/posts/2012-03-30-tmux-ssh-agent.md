---
title: tmux + ssh-agent
tags: ssh, tmux, imported
---

Lately I've been using tmux on remote servers. This allows me to maintain
task-specific sessions on remote servers, regardless of what computer I work
from or if my internet connection get severed.

The biggest wrinkle in my setup has been ssh-agent forwarding. When I reconnect
the remote side no longer knows where to find <tt>SSH_AUTH_SOCK</tt>. I found a
solution.

1. On the remote host, create <tt>$HOME/.ssh/rc</tt>, make it executable, and add the following contents:

    <script src="https://gist.github.com/2253834.js?file=rc"></script>

    Don't forget: <tt>chmod 755 $HOME/.ssh/rc</tt>

2. Add these two lines to your <tt>.tmux.conf</tt>:

    <script src="https://gist.github.com/2253845.js?file=.tmux.conf"> </script>

A few things kept me from getting this working straight away. You have to
disable the default behavior of updating <tt>SSH_SOCK_AUTH</tt> (that's what
the first line does), then you have to set <tt>SSH_SOCK_AUTH</tt> to point at
the symlink created by the script in step 1.

Edit: I mistakenly thought previously that you have to use an absolute path to
the symlink because tmux won't expand "<tt>$HOME</tt>"or "<tt>~</tt>". Turns
out, if you don't quote the path then tmux can do the right thing with
<tt>~</tt>.
