# crio-nydus-store-plugin

Let [podman](https://podman.io/) support [nydus](https://nydus.dev/) image.

# Usage

Make sure you have install podman.

1. Clone this repo

```shell
git clone git@github.com:fatelei/crio-nydus-store-plugin.git
```

2. Install dep.

```shell
>>> cd /path/to/crio-nydus-store-plugin
>>> go mod tidy
```

3. Install nydusd

You can download it from [https://github.com/dragonflyoss/image-service/releases/](https://github.com/dragonflyoss/image-service/releases/)

4. Update podman config

you should edit `/etc/containers/storage.conf`, add this line in `[storage.options]` section

```shell
additionallayerstores = ["/var/lib/nydus-store/store:ref"]
```

Make sure you have mkdir `/var/lib/nydus-store`.

5. Run

This package need nydus filesystem config, this is demo config.

```json
{
  "device": {
    "backend": {
      "type": "registry",
      "config": {
        "scheme": "http",
        "timeout": 5,
        "connect_timeout": 5,
        "retry_limit": 2
      }
    },
    "cache": {
      "type": "blobcache",
      "config": {
        "work_dir": "/var/lib/nydus/cache"
      }
    }
  },
  "mode": "direct",
  "digest_validate": false,
  "iostats_files": false,
  "enable_xattr": true,
  "fs_prefetch": {
    "enable": true,
    "threads_count": 2
  }
}
```

Exce command below.

```shell
>>> cd /path/to/crio-nydus-store-plugin
>>> go run cmd/store/main.go --nydusd-path /usr/local/bin/nydusd --log-to-stdout --log-level debug --config-path /etc/nydusd-config.json
```
