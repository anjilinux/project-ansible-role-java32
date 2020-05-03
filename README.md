# Ansible Role: Install Liberica Java

Install [Liberica Java](https://bell-sw.com/) for Linux and Windows.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 31
        - 30
    - name: Ubuntu
      versions:
        - focal
        - eoan
        - disco
        - cosmic
        - bionic
        - xenial
        - trusty
    - name: Debian
      version:
        - jessie
        - stretch
        - buster
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 8
        - 7
    # TODO.
    # - name: opensuse
    #   vesrion:
    #     - tumbleweed
    - name: windows
      version:
        - 2008x64 (7 64bit)
        - 2008x86 (7 32bit)
        - 2019 (10 64bit)
```

## Requirements

min_ansible_version: 2.7

## Role Variables

```yaml
# https://api.bell-sw.com/api.html
# https://api.bell-sw.com/api.yaml
liberica_api_version: 1
liberica_api_releases_url: https://api.bell-sw.com/v{{ liberica_api_version }}/liberica/releases

liberica_gpg_key_url: https://download.bell-sw.com/pki/GPG-KEY-bellsoft

# LTS stands for Long Term Support. It means that the release will receive security updates for a long time.
# EOL stands for End Of Life. It means that the release is not supported anymore.
# GA stands for General Availability. It means that the release is stable.
# EA stands for Early Access. It means that the release is not stable.
# Bitness describes 64 or 32-bit architecture.
# Versions: feature, interim, patch and build are described in OpenJDK community document JEP 322
liberica_win_packagetype: msi
liberica_win_installationtype: installer
liberica_win_bitness: 64
liberica_win_architecture: x86
liberica_win_bundletype: jdk-full
liberica_win_eol: 'false'
liberica_win_lts: 'true'
liberica_win_ga: 'true'
liberica_win_latestlts: 'true'

liberica_bundle_state_exsist: 'true'
liberica_bundletype: 'full'
# https://bell-sw.com/pages/repositories#packages-versioning
# full — contains the full Liberica JDK, including JavaFX and a variety of JVMs for platforms that support it.
# lite — includes Liberica JDK with compressed modules and Server VM, without any extra packages.
# runtime — contains Java SE Runtime Environment only.
# runtime-full — contains Java SE Runtime Environment, including JavaFX.

liberica_checksum_algorithm: 'sha1'

# If you *NOT* use apt-cacher-ng or other caching proxy - select "https".
http_or_https: http
# http_or_https: https

liberica_windows_local_download_path: '{{ ansible_env.TMP }}\liberica'
```

### If you want deploy to Windows 7

Download and install [Windows Management Framework 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

## HowTo

Quick config WinRM for Windows: <https://ru.stackoverflow.com/a/949971/191416>

## Example Playbooks

Install latest stable supported LTS `JRE+JDK+JavaFX` on Windows or Linux over package manager of you distro:

`install-liberica-java.yml`:

```yaml
- name: Install Liberica Java
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-liberica-java
  tasks:
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

- Add openSUSE support.

## Thanks

- [Aleksei Voitylov](mailto:aleksei.voitylov@bell-sw.com)
