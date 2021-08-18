# HSM 2 Distribution building

This document briefly describes how to (re)build an HSM 2 distribution that will serve the purpose of setting up (or upgrading from HSM 1.x) an HSM 2 for a federate node environment.

## Prerequisites

You will need:

- Docker
- A local clone of [the HSM2 repository](https://gitlab.rsklabs.io/hsm/hsm2/-/tree/master), checked out at the tag (branch, commit ref) that you wish to build from. Without loss of generality, let's assume the clone is located at `/path/to/the/clone`.
- The "middleware" and "ledger" docker images of the HSM 2 repository already built. In short, you need to execute the scripts `/path/to/the/clone/docker/mware/build` and `/path/to/the/clone/docker/ledger/build`. For more details on this, have a look at [the middleware readme](https://gitlab.rsklabs.io/hsm/hsm2/-/blob/master/middleware/README.md) and [the ledger readme](https://gitlab.rsklabs.io/hsm/hsm2/-/blob/master/ledger/README.md).

## Building

Once all your prerequisites are met, and assuming the local clone of *this repository* is at `/path/to/repo`, just issue:

```
> /path/to/repo/build-dist /path/to/the/clone
```

and follow the steps.

## Signing the ledger apps

Once the build is done, there is one more step for a distribution to be complete: the ledger apps need to be certified with a signature. For this task, you can use the `/path/to/repo/hsm/tools/signapp` script, which in turn uses the `/path/to/repo/hsm/tools/bin/signapp` middleware built during the previous step. Both the UI and the Signer must be certified. For this process, with the certifying ledger plugged in and its signing app open, just issue:

For the Signer:

```
/path/to/repo/hsm/tools/signapp ledger -a /path/to/repo/hsm/dist/firmware/signer.hex
```

For the UI:

```
/path/to/repo/hsm/tools/signapp ledger -a /path/to/repo/hsm/dist/firmware/ui.hex
```

The output will be in `/path/to/repo/hsm/dist/firmware/signer.hex.sig` and `/path/to/repo/hsm/dist/firmware/ui.hex.sig`, respectively.

## Distributing

Once the full process is complete, the `/path/to/repo/hsm/dist` directory will contain all the necessary artifacts for an HSM 2 setup (or upgrade from HSM 1.x). There's no need to distribute the rest of the contents of the `/path/to/repo/hsm` directory. With respect to running a full federate node, the distribution building process will also have copied the HSM 2 manager binary into `/path/to/repo/federation_node/server/hsm2/bin/manager`, so from an HSM 2 perspective, the `/path/to/repo/federation_node` directory is also ready for distribution.
