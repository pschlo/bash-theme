# bash-theme
Wrapper script for oh-my-posh on bash



### Step 1

Add `use-theme` and `ssh-use-theme` to `$PATH`:

* **with root access:** copy to `/usr/local/bin`

* **without root access:**

  * copy to a new folder that is accessible by all users you have access to and add `PATH="/path/to/folder:$PATH"` in the `~/.profile` of each user you have access to, or
  * always specify the full path, or
  * copy to `~/.local/bin` for each user you have access to (harder to update/uninstall!). `~/.local/bin` is added to `$PATH` by `~/.profile` by default.

  When switching to another user (e.g. with `su` or `sudo`), make sure to start a login shell, otherwise this might not work.

### Step 2

Specify the theme:

* **per-SSH-key themes:** open `~/.ssh/authorized_keys` and edit the line with your key so that it looks like this:
  `command="ssh-use-theme /path/to/theme" AAAAE2VjZHNhLXNo...`

  `ssh-use-theme` must be in the system-wide `$PATH` for SSH to be able to find it. If it is installed without root, you must thus specify the full path.

* **per-user themes:** put `source use-theme /path/to/theme` in `~/.profile`

Note that an SSH theme takes precedence over the theme defined for the user. You can override this with `-f`.

### Step 3

Ensure that themes are always loaded and kept when starting a new `bash` instance (e.g. by running `bash`, `su` or `sudo`):

* **with root access:** add `source use-theme` to `/etc/bash.bashrc`
* **without root access:** add `source use-theme` to `~/.bashrc` for each user you have access to.
  Because `~/.bashrc` is sourced before `~/.profile`, a new login shell might complain with `use-theme: not found`. This can be ignored because the theme is loaded in `~/.profile` anyway, and can be silenced by changing the line in `.bashrc` to `source use-theme 2>/dev/null`.

Note that after changing user (e.g. with `su` or `sudo`), the currently active theme takes precedence over the user-specific theme. You can override this with `-f`.

### Step 4

To increase startup time, it is recommended to disable oh-my-posh automatic update checks by entering `oh-my-posh disable notice`.





To load a theme manually, you can always use the following:

* `. use-theme`: tries to activate the last active theme.
* `. use-theme /path/to/theme`: tries to activate the last active theme. If that fails, uses the given theme.
* `. use-theme -f /path/to/theme`: uses the given theme.

To unload a theme, use `. use-theme -u` .

*Implementation Note: Before, we tried to preserve the theme by defining aliases that ensure a theme loader is sourced. It turned out to be way easier to preserve the theme by preserving environment variables.*
