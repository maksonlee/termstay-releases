# Termstay

Termstay keeps your SSH sessions running on a server of your own, and lets
you start, watch, type into and end them from a Windows PC or an Android
phone, and pick them up again after a network loss or a restart. Closing
the app leaves its sessions running on the server.

This repository holds Termstay's published programs, as
[Releases](https://github.com/maksonlee/termstay-releases/releases), and
takes problem reports in its
[Issues](https://github.com/maksonlee/termstay-releases/issues). Termstay
is not open source for now, so its source is not here: the "Source code"
archives GitHub adds to every release hold only this README and the
license.

Termstay is below 1.0 and still changing: use it, but do not make it the
only way to reach a system you depend on.

## Downloads

Each release has these files, each with a `.sha256` beside it:

| Program | File | Runs on |
| --- | --- | --- |
| Termstay Server for Ubuntu | `termstay-server_<version>-<build>_amd64.deb` | Ubuntu LTS releases in standard support, x86-64 |
| Termstay Server for Windows | `termstay-server-setup-<version>-<build>.exe` | Windows 11 x64 |
| Termstay for Windows PCs | `termstay-setup-<version>-<build>.exe` | Windows 11 x64 |

The phone app, for Android 8 or later, is on
[Google Play](https://play.google.com/store/apps/details?id=com.maksonlee.readyterm).
Windows 10 is not supported.

Check a download before you run it:

```bash
sha256sum -c termstay-server_<version>-<build>_amd64.deb.sha256
```

```powershell
Get-FileHash .\termstay-setup-<version>-<build>.exe   # compare with the .sha256 file
```

The Windows programs are not signed. Windows SmartScreen may say
"Windows protected your PC" when an installer starts: after checking its
SHA-256, choose **More info**, then **Run anyway**.

## Install the server

The server keeps the sessions. Install it on the computer the sessions
should run from; the apps reach it over your network. Termstay has no cloud
account or relay: a device away from that network reaches the server only
through a network path of your own, such as a VPN.

### On Ubuntu

```bash
sudo apt install ./termstay-server_<version>-<build>_amd64.deb
sudo termstay-server sharing on --discovery
sudo ufw allow 22822/tcp    # if ufw is on
```

The package runs the server as the system account `termstay`, now and at
each boot. `sudo termstay-server` administers it; network discovery uses
Avahi (`avahi-daemon`).

### On Windows

Run `termstay-server-setup-<version>-<build>.exe` as an administrator. It
installs the server as a Windows service, lets it through Windows Firewall
on private and domain networks, and adds the **Termstay Server** icon to
the Start menu. On the icon's menu, choose **Let other devices connect**
and **Let devices on this network find this PC**. On a network Windows
calls public, devices cannot connect until the network is set to private.

## Connect an app

1. Install the PC app with `termstay-setup-<version>-<build>.exe` (no
   administrator needed), or the phone app from Google Play.
2. On **Session servers**, choose **Add server**, then a server the app
   found, or enter its IP address and port (22822 unless you chose
   another).
3. Compare the host ID and certificate fingerprint the app shows with
   `termstay-server status` on the server, then choose **Yes, request
   access**.
4. Approve the request on the server when its code matches the app's: on
   the Windows icon's menu, or with `termstay-server approve <code>` (with
   `sudo` on Ubuntu).
5. Choose **Show sessions**, and start one.

## Update

Update the server first, then the PC app; Google Play updates the phone
app. An app newer than its server says the server needs an update.
Sessions keep running across a server's update. Each version keeps the
saved data of the version before it.

- Ubuntu: `sudo apt install ./termstay-server_<version>-<build>_amd64.deb`
  with the new package.
- Windows: run the new installer.

Versions are `0.MINOR.PATCH`. A patch only fixes; a new minor adds, and its
release notes say when the server and the apps must be updated together.

## Report a problem

Open an [issue](https://github.com/maksonlee/termstay-releases/issues)
with the versions you use: the PC app's Settings, About; the phone app's
About Termstay; `dpkg -s termstay-server` on Ubuntu; Settings, Apps on
Windows. Never include passwords, private keys, terminal input or output,
commands, tokens or private host names. Report a security problem
privately, through this repository's **Security** tab, never in a public
issue.

## License

Termstay's programs are free to use, for personal or commercial purposes,
under the [Termstay License](https://github.com/maksonlee/termstay-releases/blob/main/LICENSE),
which does not allow redistributing or modifying them. The open-source
components they include are under their own licenses, listed in the
notices that come with each program.
