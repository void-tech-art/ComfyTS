# ComfyTS

## The most powerful and modular stable diffusion GUI and backend.

![ComfyUI Screenshot](comfyui_screenshot.png)

ComfyTS ("Comfy-The-Sequel" or "Comfy-TypeScript") is a fork of ComfyUI. It serves as the backend for [void.tech](https://void.tech). Project goals:

-   Fix issues with ComfyUI
-   Adapt ComfyUI to work in a serverless, multi-user environment more easily
-   Maintain compatability with the existing ComfyUI ecosystem of custom-nodes and workflows

### Docker Instructions:

-   Start your docker daemon, then in the root folder run the build command:

    `docker build -t voidtech0/comfy-ts:0.1.0 .`

Note that the docker-build does not copy the models in the docker-image (that would be stupid). Instead, it expects to load the models from an NFS-drive mounted to the container on startup.

-   `docker run -it --name (???) --gpus all -p 8188:8188 -v "$(pwd)"/storage:(???)`

Other ComfyUI docker images:

-   https://hub.docker.com/r/yanwk/comfyui-boot
-   https://hub.docker.com/r/universonic/stable-diffusion-webui
-   https://hub.docker.com/r/ashleykza/stable-diffusion-webui
-   https://github.com/ai-dock/comfyui

### Docker Build Arguments

`--build-arg HEADLESS_MODE=true`: when building in headless-mode, ComfyTS will not copy over any of its UI-components, making for a smaller docker-image. This is useful when running Comfy as purely a back-end API or in a cloud-environment.

### Docker To Do:

-   Make sure filesystem cache and sym-links are working.
-   Do we really need the extra_model_paths?
-   We probably won't need sym-links and extra-model paths anymore to be honest; we can build those into comfy-ts directly.
-   Stop custom nodes from downloading external files and doing pip-install at runtime (on startup). We should ensure that's all done at build-time.
-   NFS sym-links: all of ComfyUI's folders (/input, /output, /temp, /models) should be symlinked to an NFS drive, so that they can be shared amongst workers.

### General To Do:

-   Add ComfyUI manager into this repo by default
-   Add a startup flag to turn off the ComfyUI manager and other settings. (This is for when running ComfyTS in a cloud environment, where users downloading custom-nodes would be inappropriate.)
-   Add a startup flag to switch between using ComfyUI-local or using the void-tech API

### Future

-   We'll need to update the ui-tests, so that they work with importing Litegraph as a library rather than assuming it exists already in its execution context.

### Building the UI

`/web` is now a build-folder, built from `/web-src`. To recreate it, `cd` into /web-src, then run `yarn` to install dependencies, followed by `yarn build`.

You normally shouldn't commit build-folders to your repo, but in this case it makes it easier for end-users to just git-clone this repo and start working, without the need to install any javascript-package mangers on their machine.
