# luci-app-run

`luci-app-run` is a small OpenWrt LuCI application for uploading and running
makeself-generated `.run` installers.

The package intentionally does not implement an app store or software catalog.
It only accepts a `.run` file, uploads it to `/tmp/luci-app-run`, marks it
executable, runs it in the background, and streams the execution log back to the
LuCI page.

Large uploads use a one-time-token CGI endpoint at
`/cgi-bin/luci-app-run-upload` instead of base64-over-ubus chunking. This keeps
large `.run` uploads close to raw HTTP upload speed.



## Build with OpenWrt SDK

Copy or symlink this directory into your OpenWrt SDK package feed, for example:

```sh
mkdir -p package/luci-app-run
cp -a /path/to/luci-app-run/* package/luci-app-run/
./scripts/feeds update -a
./scripts/feeds install -a
make package/luci-app-run/compile V=s
```

The resulting package will be under:

```text
bin/packages/<target>/base/luci-app-run_1.0.0-1_all.ipk
```

On apk-based OpenWrt 25.12 builds, build the `.apk` with the matching target
SDK, for example the x86/64 SDK for x86_64 systems. Do not use the old manual
ipk repack approach for apk packages; apk v2/v3 package metadata and repository
format should be produced by the OpenWrt SDK tooling.

## Notes

- Only files ending in `.run` are accepted.
- Uploads larger than 256 MiB are rejected by the backend.
- Only one installer can run at a time.
- Uploaded files and logs are temporary and live below `/tmp/luci-app-run`.
