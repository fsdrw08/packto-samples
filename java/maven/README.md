# Java Maven Sample Application

See [prerequisites](https://paketo.io/docs/howto/java/#prerequisites) of this sample.

## Building

```bash
pack build applications/maven
```

Alternatively, if you want to attach a Maven `settings.xml` file to pass additional configuration to Maven.

```bash
pack build applications/maven --env BP_JVM_VERSION=17 --volume $(pwd)/bindings:/platform/bindings
```

The command above will use the sample `settings.xml` file from this repo. It may be more useful to copy your local `settings.xml` first.

```bash
cp ~/.m2/settings.xml java/maven/bindings/maven/settings.xml
pack build applications/maven --volume $(pwd)/bindings:/platform/bindings
```

## Running

```bash
docker run --rm --tty --publish 8080:8080 applications/maven
```

## Viewing

```bash
curl -s http://localhost:8080/actuator/health | jq .
```

```powershell
# ref: https://paketo.io/docs/
# https://buildpacks.io/docs/tools/pack/#windows-container
cd "$(git rev-parse --show-toplevel)\java\maven"
$imageName="paketo-demo-app"
$builder="docker.io/paketobuildpacks/builder:base"
$buildPack="docker.io/paketobuildpacks/java-azure:10"
$workspace=$(wsl wslpath "$($PWD.path -replace "\\","\\")")
$env:HTTP_PROXY="192.168.255.1:7890"
$env:HTTPS_PROXY="192.168.255.1:7890"
$sock="/var/run/user/1000/podman/podman.sock" # /var/run/podman/podman.sock
    # --privileged=true `
podman run `
    --cap-add=CAP_FOWNER `
    --tmpfs /tmp:rw `
    --volume /var/run/user/1000/podman/podman.sock:/var/run/docker.sock `
    --volume $workspace/:/workspace:rw `
    --workdir /workspace `
    --env HOME="/workspace" `
    docker.io/buildpacksio/pack:latest build `
        --env HTTP_PROXY="192.168.255.1:7890" `
        --env HTTPS_PROXY="192.168.255.1:7890" `
        --trust-builder `
        $imageName --path . --builder $builder --buildpack $buildPack

    --env HTTP_PROXY="192.168.255.1:7890" `
    --env HTTPS_PROXY="192.168.255.1:7890" `


$builder="docker.io/paketobuildpacks/builder:base"
$buildPack="docker.io/paketobuildpacks/java-azure:10"
pack build $imageName `
    --env "HTTP_PROXY=10.20.72.21:9999" `
    --env "HTTPS_PROXY=10.20.72.21:9999" `
    --builder $builder `
    --buildpack $buildPack `
    --volume $workspace/:/workspace:rw `
    --workspace /workspace `
    --path . 
```