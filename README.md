# Workstation Setup
This project is intended to facilitate the task of setting up a fresh install.

## Usage
```bash
ansible-playbook -K setup.yml --extra-vars "@extra_vars.json"
```
Use the `-K` modifier so Ansible can ask for your `sudo` password if needed.

## Project Structure
```
.
├── extra_vars.json
├── roles
│   ├── common
│   │   ├── files
│   │   │   └── repos
│   │   └── tasks
│   │       ├── common.yml
│   │       ├── main.yml
│   │       └── repos.yml
│   ├── dotfiles
│   │   ├── files
│   │   │   ├── mpv
│   │   │   │   └── mpv.conf
│   │   │   ├── qbittorrent
│   │   │   │   ├── download_rules.json
│   │   │   │   └── feeds.json
│   │   │   └── vscode
│   │   │       └── settings.json
│   │   ├── tasks
│   │   │   ├── git.yml
│   │   │   ├── main.yml
│   │   │   ├── mpv.yml
│   │   │   ├── qbittorrent.yml
│   │   │   └── vscode.yml
│   │   └── templates
│   │       ├── gitconfig.j2
│   │       └── qbittorrent.conf.j2
│   ├── extras
│   │   ├── files
│   │   │   └── repos
│   │   └── tasks
│   │       ├── extras.yml
│   │       ├── main.yml
│   │       └── repos.yml
│   ├── gnome
│   │   └── tasks
│   │       ├── gsettings.yml
│   │       └── main.yml
│   └── nvidia
│       ├── files
│       │   └── repos
│       └── tasks
│           ├── main.yml
│           ├── nvidia.yml
│           └── repos.yml
└── setup.yml
```

## Files
```
.
└── files
    └── repos
```
This sub-directory is present in tasks that need to make use of specific repositories. These are YUM/DNF repo files that will be copied under `/etc/yum.repos.d`. Files placed here must have an `.repo` extension to be processed.

The included repositories cover the author's basics needs. Feel free to adjust them as you like.

## Tasks
```
.
└── setup.yml
```
This is the main playbook.

## Roles
```
.
├── common
├── dotfiles
├── extras
├── gnome
└── nvidia
```
The roles under this directory try to organize tasks and provide flexibility.

### Common
Installs packages needed by most home users (strongly opinionated).
Like `unrar` or `gstreamer` plugins.

### Dotfiles
Place any dotfile playbook under this role.

### Extras
Installs packages for specific tasks. Like `spotify` or `steam`.

### GNOME
Configures GNOME settings through the `gsettings` CLI tool. It can be switched in favor of another role to configure another desktop environment.
Keys that need to be adjusted can be placed under the `gsettings` array in the `extra_vars.json` file.

### NVIDIA
Expects an NVIDIA card installed in the system but it can be switched in favor of another role to accomodate a graphics card from another vendor.

## Vars
```
{
    "local_user": "johndoe",
    "local_user_name": "John Doe",
    "local_user_email": "johndoe@mail.com",
    "gsettings": [
        {
            "path": "org.gnome.settings-daemon.plugins.color",
            "key": "night-light-enabled",
            "value": "true"
        },
        {
            "path": "org.gnome.nautilus.preferences",
            "key": "default-folder-viewer",
            "value": "list-view"
        },
        {
            "path": "org.gnome.GWeather",
            "key": "temperature-unit",
            "value": "centigrade"
        }
    ]
}
```
This JSON file enables the playbook to be configured for another local systems.
Remember to modify `local_user_*` and `gsettings` to reflect you preferences before running this playbook.

## License
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html)

Copyright (c) 2019-present, Pablo Spizzamiglio
