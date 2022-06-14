# HSM 2 Distribution building

This document briefly describes how to (re)build an HSM 2 distribution that will serve the purpose of setting up (or upgrading from HSM 1.x) an HSM 2 for a federate node environment.

## Prerequisites

You will need:

- Docker
- A local clone of [the powHSM repository](https://github.com/rsksmart/rsk-powhsm), checked out at the tag (branch, commit ref) that you wish to build from. Without loss of generality, let's assume the clone is located at `/path/to/the/clone`.
- The "middleware", "ledger" and "packer" docker images of the powHSM repository already built. In short, you need to execute the scripts `/path/to/the/clone/docker/mware/build`, `/path/to/the/clone/docker/ledger/build` and `/path/to/the/clone/docker/packer/build`. For more details on this, take a look at [the quickstart guide](https://github.com/rsksmart/rsk-powhsm/blob/master/QUICKSTART.md).

## Building

Once all your prerequisites are met, and assuming the local clone of *this repository* is at `/path/to/repo`, just issue:

```
> /path/to/repo/build-dist /path/to/the/clone
```

and follow the steps.

## Signing the ledger apps (versions up to 2.3 only)

Once the build is done, there is one more step for a distribution to be complete: the ledger apps need to be certified with a signature. For this task, you can use the `/path/to/repo/hsm-builder/tools/signapp` script, which in turn uses the `/path/to/repo/hsm-builder/tools/bin/signapp` middleware built during the previous step. Both the UI and the Signer must be certified. For this process, with the certifying ledger plugged in and its signing app open, just issue:

For the Signer:

```
/path/to/repo/hsm-builder/tools/signapp ledger -a /path/to/repo/hsm-builder/dist/firmware/signer.hex
```

For the UI:

```
/path/to/repo/hsm-builder/tools/signapp ledger -a /path/to/repo/hsm-builder/dist/firmware/ui.hex
```

The output will be in `/path/to/repo/hsm-builder/dist/firmware/signer.hex.sig` and `/path/to/repo/hsm-builder/dist/firmware/ui.hex.sig`, respectively.

## Authorising a signer upgrade (versions 3 and up only)

Once the build is done, if the distribution is intended for a signer upgrade to an existing powHSM setup, a fully signed `/path/to/repo/hsm-builder/dist/firmware/signer_auth.json` file must be present, authorising the signer version to which the device is to be upgraded. The `signapp.py` middleware tool aids in the generation of the signatures needed (the file is generated when the distribution is built, but with no signatures). The minimum number of signatures needed will be the threshold (N/2+1) for the total number of authorizers (N) in the UI authorizers headers file specified at build time.

## Distributing

Once the full process is complete, the `/path/to/repo/hsm-builder/dist` directory will contain all the necessary artifacts for a powHSM setup (or upgrade from HSM 1.x in versions up to 2.3). There's no need to distribute the rest of the contents of the `/path/to/repo/hsm-builder` directory. With respect to running a full federate node, the distribution building process will also have copied the powHSM manager binary into `/path/to/repo/federation_node/server/hsm2/bin/manager`, so from a powHSM perspective, the `/path/to/repo/federation_node` directory is also ready for distribution.
