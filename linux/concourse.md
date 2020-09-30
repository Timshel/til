# Concourse

## Garden runtime

By default everything is in `/concourse-work-dir`.

If you fly hijack a container the prompt will give you an id Ex: `ea56d453-f9de-483d-5fa0-860b8f971277`
With it you can find the container definition in `/concourse-work-dir/depot/ea56d453-f9de-483d-5fa0-860b8f971277/`
It contains a `config.json` in which you can find how the chroot is assemnble and all the matching volume stored in `/concourse-work-dir/volumes/live`