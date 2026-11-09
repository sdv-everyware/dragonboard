# dragonboard

Linaro's board support for the Dragonboards in AOSP, `device/linaro/dragonboard`, as SDV
EveryWare builds it for the Qualcomm RB3 (db845c). The EveryWare product that uses it is in
the `device-rb3` repository; this one carries the AOSP code at the tag `android-17.0.0_r1`
and the one change the product needs.

Branches:

| Branch | What it holds |
|---|---|
| `everyware-17.0.0_r1` | the AOSP import and our commit; the EveryWare manifest checks this out at `device/linaro/dragonboard` |
| `aosp-17.0.0_r1` | the import alone: the AOSP tree at the tag `android-17.0.0_r1` |

A new AOSP release gets the same pair of branches (`aosp-<release>`, `everyware-<release>`), our
commit rebased onto the new import.

Our commit, sepolicy: the platform side of the policy (the `tinymix` domain, a coredomain)
moves from the vendor policy to `sepolicy/system_ext`, listed in
`SYSTEM_EXT_PRIVATE_SEPOLICY_DIRS`, because the Treble labeling test of the public tree rejects
a coredomain in vendor policy; `libaconfig_storage_read_api_cc.so` gets the
`same_process_hal_file` label: `libui.so` needs it, and without it a process that loads Linaro's
prebuilt mesa through the EGL loader finds no OpenGL ES; the libdrm line leaves
minigbm's file_contexts. Nothing else of Linaro's is changed; the prebuilt vendor package is
fetched as in AOSP (`fetch-vendor-package.sh`).

The code is Apache-2.0, as in AOSP (LICENSE).
