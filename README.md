# luci-app-run

`luci-app-run` 是一个轻量级的 OpenWrt LuCI 应用，用于上传并运行由 makeself 生成的 `.run` 安装程序。

它仅接受用户上传的 `.run` 文件，将其保存到 `/tmp/luci-app-run` 目录，赋予可执行权限，在后台运行该安装程序，并将执行日志实时返回到 LuCI 页面。

对于较大文件的上传，采用独立的 CGI 接口 `/cgi-bin/luci-app-run-upload` 进行一次性令牌验证的二进制上传，避免使用 base64-over-ubus 分块传输，从而获得接近原生 HTTP 的上传速度。

## 使用 OpenWrt SDK 编译

将本目录复制或软链接到 OpenWrt SDK 的 package feed 中，例如：

```sh
mkdir -p package/luci-app-run
cp -a /path/to/luci-app-run/* package/luci-app-run/
./scripts/feeds update -a
./scripts/feeds install -a
make package/luci-app-run/compile V=s
```

## 安装方法
### Luci 25.12.x

```sh
wget -O luci-app-run.apk <替换为apk地址>
apk update
apk add --allow-untrusted luci-app-run.apk
```

### Luci 21——24.10 

```sh
wget -O luci-app-run.ipk <替换为ipk地址>
opkg update
opkg install luci-app-run.ipk
```
- 或者在软件包 上传安装ipk