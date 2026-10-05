# ansible-role-citrix

Installs the Citrix Workspace app (ICAClient) on openSUSE. Citrix has no package repository: the role reads
the "latest" download page, takes the current token-signed link of the SUSE RPM from the download button
(`rel` attribute, static HTML) and downloads the RPM on the controller.

1. **controller** (once per run): download into `download_dir` - only a version that is not there yet. Page,
   link or download not available → notice, the newest RPM already in `download_dir` is used; only if there
   is none the run fails. The newest `keep` RPMs are kept.
2. **host**: installs the libraries the RPM needs but doesn't declare, then - only if ICAClient is missing or
   older than the RPM - copies the RPM to `/var/tmp/ansible-citrix/`, installs it with zypper from that path
   (`--allow-unsigned-rpm`: the Citrix RPM is unsigned) and removes the copy.

```yaml
citrix:
  install: true                     # default false
  # download_dir: /srv/ansible/largefiles/citrix   # on the controller (nsbldr: ansiblelargefilesdir)
  # download: true                  # false: no download, use the newest RPM in download_dir (pinning)
  # keep: 2
  # page_url: https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html
  # packages: [libva-drm2, libva-x11-2, libwebkit2gtk-4_0-37]
```

Pinning a version: put the RPM into `download_dir` and set `download: false`. A newer RPM is only installed
when it is in `download_dir`; ICAClient is never downgraded.

Tested on Leap 16.1 with `ICAClient-suse-gcc-8-26.04.10.1-0.x86_64.rpm` (425 MB): `wfica` and `selfservice`
find all libraries.

## License

Apache-2.0

Created with the help of AI
