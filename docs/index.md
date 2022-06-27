# SagerNet

The universal proxy toolchain for Android, written in Kotlin.

[![](https://count.getloli.com/get/@nekohasekai?theme=rule34)](https://github.com/SagerNet/SagerNet)

### Changelog

#### 0.8.1-beta02

* Bug fixes

#### 0.8.1-beta01

* Add support for [Mieru](https://github.com/enfein/mieru)
* Add support for [TUIC](https://github.com/EAimTY/tuic)

#### 0.8-rc05

* Bug fixes

#### 0.8-beta06

* Shadowsocks 2022 TCP protocol breaking changes **1**

**1**:

This change prevents a possible detection method (also present in old shadowsocks).

Need to update sing ss-server or shadowsocks-rust >= v1.15.0-alpha.4 (not released yet, use master branch).

#### 0.8-beta05

* Fix shadowsocks 2022-blake3-aes-128-gcm implementation **1**
* Bug fixes

**1**: Due to an implementation detail bug, users of 2022-blake3-aes-128-gcm will need to update both the server and
client.

#### 0.8-beta03

* Performance improvements
* Shadowsocks 2022 Extensible Identity Headers support
* Android 13 themed icon support
* Bug fixes

Note: we have created `Clash` / `Clash For Android` fork with Shadowsocks 2022 support for Clash users:

https://github.com/SagerNet/clash
https://github.com/SagerNet/ClashForAndroid

#### 0.8-beta02

* SUoT **1** support for shadowsocks and naiveproxy
* Bug fixes

**1**:

[SagerNet UDP Over TCP Protocol](https://github.com/SagerNet/UoT)

For shadowsocks, requires one of the following servers:

* [SagerNet/outline-ss-server](https://github.com/SagerNet/outline-ss-server)
* [SagerNet/v2ray-core](https://github.com/SagerNet/v2ray-core)

For naiveproxy, replace official server with [SagerNet/forwardproxy](https://github.com/SagerNet/forwardproxy).

#### 0.8-beta01

* Disabled global http proxy by default
* Add option to close active connections (by app)
* Increase buffer size to support MTU lager than 8192 **1**
* Reset connections when network changed
* Improved china apps scanner
* Shadowsocks 2022 protocol support **2**
* Added XTLS protocol compatibility for VLESS and Trojan **3**
* Added gRPC multi/raw mode
* Updated brook support
* Bug fixes

**1**:

Using an MTU larger than the upstream interface will only improve speed in some scenarios and may result in UDP packet
loss.

**2**:

Added support for the following new `Shadowsocks 2022 Edition` ciphers:

```
2022-blake3-aes-128-gcm
2022-blake3-aes-256-gcm
2022-blake3-chacha20-poly1305

```

The new protocol is designed to improve UDP performance and provide full replay protection.

Although the protocol is still in draft, professionals can try to deploy it
with [Shadowsocks-NET/outline-ss-server](https://github.com/Shadowsocks-NET/outline-ss-server) (recommend)
or [SagerNet/v2ray-core](https://github.com/SagerNet/v2ray-core) (main branch).

**3**:

This protocol does not guarantee concealment.

#### 0.7-rc11

* Bug fixes

#### 0.7-rc10

* Bug fixes

#### 0.7-rc09

* Added back hysteria disableMtuDiscovery option
* Bug fixes

#### 0.7-rc08

* Optimized memory usage for System Tun
* Optimized performance for 64 bit devices
* DNS replies with original TTL
* Bug fixes

#### 0.7-rc06

* Always show stop button in notification
* Added profile switch dialog to notification
* Added option to hold cpu wakelock
* Added cpu wakelock switcher to notification
* Enabled dns routing by default
* Removed hysteria disableMtuDiscovery option
* Bug fixes

#### 0.7-rc05

* Bug fixes
* Updated android gradle plugin
* Updated dependencies
* Refined database
* Add android 12 dataExtractionRules

#### 0.7-rc04

* Bug fixes

#### 0.7-rc02

* Bug fixes
* Handling non-IP DNS query support **1**

**1**:

For local DNS, Android 10+ is required for raw query.

#### 0.7-rc01

* Bug fixes
* Ping proxy now supports returning ICMP Unreachable **1**

**1**:

Requires [SagerNet/v2ray-core](https://github.com/SagerNet/v2ray-core) v5.0.4+ or routed to a freedom outbound (Bypass
Route)

#### 0.7-beta06

* Bug fixes

#### 0.7-beta05

* Bug fixes

#### 0.7-beta04

* Rewritten dns part of v2ray-core **1**
* Auto mtu option **2**
* Shadowsocks experiment ReducedIvHeadEntropy option **3**
* Fixes and Improvements

**1**:

Added DNS Over TLS and QUIC support, example:

```
tls://dns.google
quic://dns.adguard.com
```

All available DNS schemes:

```
tcp
tcp+local
udp
udp+local
tls
tls+local
https
https+local
quic
quic+local
```

Multiple DNS filled in now will query concurrently and share the cache.

Not yet tested on a large scale, but you can help.

**2**:

Use the MTU of the upstream network interface, enabled by default.

The VPN will automatically reload if the upstream MTU changes.

**3**:

This is a pull request for the experiment on GFWReport's proposal for a countermeasure for the random stream like
protocol blocking behaviour observed on GFW.

According to the gfw.report et al's research, when connecting to the impacted VPSs, GFW is likely to block binary
protocols unless it is an HTTP, TLS, or SSH connection, or the first 6 bytes of data sent from client to server can be
interpreted as printable characters. This suggests in addition to TLS encryption, HTTP Header, this kind of censorship
can also be temporarily evaded by only sending printable characters in the first 6 bytes of data.

This option request V2Ray to remap the first 6 bytes of IV to printable characters.

Enabling it have security implications. It is possible for anyone on the privileged network path to identify the
protocol when this experiment is enabled.

See also:

- https://github.com/v2fly/v2ray-core/pull/1552
- https://github.com/shadowsocks/shadowsocks-rust/commit/53aab484f8daba6f5cee6896b034af943cc3d406

#### 0.7-beta03

* Add MTU settings for VPN and WireGuard
* Ping proxy support **1**

**1**:

In addition to WireGuard, requires a server of `SagerNet/v2ray-core v5.0.3+`, and a protocol that supports UDP. (can be
used as a front proxy to support other protocols)

To enable ping support on the server, add the `ping` JSON item:

```json
{
  "ping": {
    // For more settings, check out the README
  }
}
```

Client behavior is controlled by routing; if there are no results and the default outbound is not WireGuard, ping will
return directly.

To route all ping requests, use `ping` as the `protocol` value.

Pay attention to the routing order, if `outbound` is `Bypass`, the ping will be sent directly.

#### 0.7-beta02

* Fix plugin error

#### 0.7-beta01

* Merge v2ray-core v5.0.2
* Add packetEncoding **1** option to vmess/vless
* Add option **2** to switch root CA certificates provider
* Add tasker support
* Fix NAT test not handling incorrect source address error

**1.** Endpoint independent mapping NAT (aka full cone NAT) support:

- `None`: disabled, uses less traffic.
- `Packet`: packetaddr protocol, need `v2fly/v2ray-core v5.0.2+` or `SagerNet/v2ray-core`.
- `XUDP`: xudp protocol, need `XTLS/Xray-core` or `SagerNet/v2ray-core`.

**2.** The built-in [Mozilla Included](https://wiki.mozilla.org/CA/Included_Certificates) root CA certificate trust list
with Chinese certificates removed is now used by default, or you can switch back to the system list.

Note: [SagerNet/v2ray-core](https://github.com/SagerNet/v2ray-core) is a generic v2ray-core fork that includes useful
features and optimizations, and is now officially released.

#### 0.6-rc04

* Add backup function
* Add hysteria wechat-video mode
* Add option to use local dns as direct dns
* Fixes

#### 0.6-rc03

* Fixes and Improvements

#### 0.6-rc02

* Fixed nat test not going through proxy under gVisor and proxy only mode
* Removed alterId

#### 0.6-beta20

* Fixes

#### 0.6-beta19

* Add network type and wifi ssid to routing rule
* Fixes and Improvements

#### 0.6-beta15

* Improved system TUN
* New option to disable DNS record expire (TTL)
* New default DNS and hosts
* DNS behavior change: continue to the next DNS server when an error is received
* Hysteria faketcp protocol support (root required)
* Fixes and Improvements

#### 0.6-beta14

* Replace LwIP TUN with new system stack implementation
* Fixes and Improvements

#### 0.6-beta13

* Fixes

#### 0.6-beta12

* Added stun NAT checker
* Added support for hysteria sharing link invented by Shadowrocket
* Fixes and Improvements

#### 0.6-beta11

* Fixes and Improvements
* Updated v2ray-core, gVisor and WireGuard

#### 0.6-beta10

* Removed foreground detector due to play policy
* Fixes and Improvements

#### 0.6-beta09

* Added hysteria ALPN option
* Updated v2ray-core, gVisor and WireGuard

#### 0.6-beta08

* Fixes and Improvements

#### 0.6-beta06

* New default routing rules
* FakeDNS removed
* Fixed compatibility with ssr none cipher
* Fixes and Improvements

For technical reasons, removing FakeDNS fixes a upstream memory leak bug and improves performance (
when it is not enabled).  
As well, we think FakeDNS doesn't make much sense at all.

#### 0.6-beta05

* Fixes and Improvements

#### 0.6-beta04

* Added QUIC sniffer
* Fixes and Improvements

#### 0.6-beta03

* Fixes

#### 0.6-beta01

* Based on v2ray-core v5 (mainline)
* Multi-observation support: allows customization of test link and time interval for each balancer
* Performance improvements
* Snell removed
* NaiveProxy's `--insecure-concurrency` option support

Note: This release contains numerous unpublished and unexplained upstream and internal changes, and some features may
not work as expected. If possible, thanks for your testing.

#### 0.5-rc25

* Fixes and Improvements

#### 0.5-rc24

* Fixes
* Support specifying port in UDP DNS

#### 0.5-rc23

* Performance improvements
* Option to disable profile traffic statistics
* Filter support for OOCv1 owner field
* Fixes

* Updated v2ray-core to v4.43.0
* Updated shadowsocks-rust to v1.12.0-alpha

#### 0.5-rc21

* Fixes

#### 0.5-rc20

* Optimized performance for gVisor mode
* Parsing clash's websocket early data configuration
* Sending shadowsocks handshake with payload if available [#1292](https://github.com/v2fly/v2ray-core/pull/1292)
* Fixes

#### 0.5-rc19

* Fix queryStrategy ignored [#1285](https://github.com/v2fly/v2ray-core/pull/1285)
* Minor fixes

#### 0.5-rc18

* Removed read subscription timeout
* Mux settings will be applied to naiveproxy
* Fixes

#### 0.5-rc17

* Fixed a serious bug in the previous version caused by updating to AGP7

#### 0.5-rc16

* Performance improvements
* Updated v2ray-core to v4.42.2

#### 0.5-rc15

`This release contains many v2ray-core changes, some of which have been submitted upstream for review.`

**Override destination option**

Disabled by default.

When disabled, `AsIs` can get the same accuracy as `IPIfNonMatch` and do not require the server to resolve the domain
name again before connecting.

**Resolve destination option**

Resolve the domain (if sniffed), and overwrite the destination address based on the IPv6 Mode selection result.

Note: There is no fallback to other ips when the connection fails.

**Concurrent DNS queries**

If you fill in more than one DNS, core will query concurrently.

**Endpoint independent mapping**

Fixed UDP address mapping behavior.

This does not work for v*ess and mux.cool; for plugins, it depends on the plugin itself.

**Fixes**

* Fixed Local DNS queries getting fallback to Remote DNS
* Fixed LwIP not work
* Other fixes

Note: you should always use gVisor if available.

#### 0.5-rc14

* Fixes
* Performance improvements

#### 0.5-rc13

* Fixes

#### 0.5-rc12

* Tun2ray implementation based on LwIP is available (try if gVisor fails)
* Fixes and Improvements

#### 0.5-rc11

* CloudFlare Warp WireGuard configuration generator
* Fixes

#### 0.5-rc10

* Fixes

#### 0.5-rc09

* WireGuard: multiple local address support (split by line)
* WireGuard: parse multiple local address from config file
* WireGuard: ipv6 support
* Fixes

#### 0.5-rc07

* WireGuard support (needs wireguard-plugin)
* Improved balancer
* Fixes and Improvements

#### 0.5-rc06

* Added shadowsocks-libev (for full shadowsocks support on android L)
* DNS items starts with # will be ignored
* Fixes and Improvements

#### 0.5-rc05

* Log viewer
* Simple crash reporter
* Fixes

#### 0.5-rc04

* Fixes

#### 0.5-rc03

* SSH support
* Fixes and Improvements

#### 0.5-rc02

* Refined route assets manager (download compressed resources)
* Removed uTLS because it caused bugs and made no sense
* Fixes and Improvements

#### 0.5-rc01

* Fixes and Improvements

#### 0.5-beta04

* Option to enable traffic statistics (disabled by default)
* Fixes

Note: According to user reports, this traffic statistics feature adds some power consumption, so an option is provided
to choose from.

#### 0.5-beta03

* Included shadowsocks xchacha20-ietf-poly1305 cipher in core
* Included uTLS in core
* Fixes and Improvements

#### 0.5-beta02

* Fixes

#### 0.5-beta01

* Migrated to a [custom core](https://github.com/SagerNet/v2ray-core) to support improved routing
* Traffic statistics support, including real-time display and cumulative statistics
* Foreground status based routing support
* Fixes and Improvements

#### 0.4-rc07

* Updated Shadowsocks(R) protocol list
* Added plugin download assistant
* Added group order setting (origin/by name/by delay)
* Fixes and Improvements

#### 0.4-rc06

* Fixes
* Full hysteria 0.8.5 support

#### 0.4-rc05

* Fixes

#### 0.4-rc04

* Fixes

#### 0.4-rc03

* Refined tun implementation (tun direct to v2ray-core)
* Refined icmp ping [implementation](https://github.com/SagerNet/libping)
* Refined url test implementation
* Fixes

#### 0.4-rc02

* Refined tun implementation (gvisor based uid routing support)
* [SagerConnect](https://github.com/SagerNet/SagerConnect) support

#### 0.4-rc01

* Fixes and Improvements

#### 0.4-beta03

* Fixes

#### 0.4-beta02

* Added internal shadowsocks simple-obfs / v2ray plugin
* Added snell support
* Replaced shadowsocksr-libev with clash implementation

#### 0.4-beta01

* New core library
* Added golang based lwip tun2socks implementation
* Added gvisor based tun2socks implementation

#### 0.3-rc14

* Fixes

#### 0.3-rc12

* SOCKS 4/4a support
* VMess experiments support
* Fixes

#### 0.3-rc09

* Fixes
* Target API 31 (Android 12)

#### 0.3-rc08

* Fixes

#### 0.3-rc07

* Fixes and Improvements

#### 0.3-rc06

* Added [hysteria](https://github.com/HyNetwork/hysteria) support
* Fixes
* Updated translations

#### 0.3-rc05

* Fixes
* Updated translations

#### 0.3-rc04

* Epoll support for Native Forwarding
* Fixes
* New zh_CN [issue template](https://github.com/SagerNet/SagerNet/issues/new/choose)

#### 0.3-rc02

* Fixes

#### 0.3-rc01

Warning: It's an incompatible update, backup your profiles and subscriptions (Clearing data is recommended).

* Experimental traffic forwarder (alternative to tun2socks, with better performance and more options)
* Added ICMP Ping / URL Test
* Option to reorder profiles by delay
* Option to switch method for bypass LAN
* Chain support for almost all protocols
* Protocol provider setting for Trojan and Shadowsocks AEAD profiles
* Option to append HTTP Proxy to VPN (Android 10+)
* Use 1.0.0.1 as default dns [#180](https://github.com/SagerNet/SagerNet/issues/180)
* Fix routing rule not applied for TCP DNS (v2ray-core master)
* New group structure and settings (read [Group](https://sagernet.org/group/))
* Import/Export subscription from/to link (external or qrcode)
* Import profile from external link
* Added conscrypt to provides consistent TLS 1.3 compatibility
* Custom config support for routing
* Application-based routing support
* Improved dns settings
* Updated shadowsocks-rust to v1.11.2
* Fixes and Improvements

#### 0.2-rc05

* Bug fixes

#### 0.2-rc04

* TCPing connection test
* Option to always show profile address
* Fixes and Improvements

#### 0.2-rc03

* Bug fixes

#### 0.2-rc02

* Configuration export
* Balancer support
* More options when exporting links
* Optimized proxy start and stop
* Fixed PingTunnel plugin (now you need ROOT permission to start it)
* Other fixes

#### 0.2-rc01

* Bug fixes

#### 0.1-rc27

* V2Ray rule assets manager
* Option to switch rule assets provider
* Option to enable log for debug
* Updated to v2ray-core 4.41.0
* Bug fixes

#### 0.1-rc26

* Refined IPv6 settings
* Updated to v2ray-core 4.40.1
* Bug fixes

#### 0.1-rc25

* Bug fixes

#### 0.1-rc24

* Fixed compatibility with Lineage's VPN Hotspot
* Allow insecure option for v2ray and trojan profiles
* More alert for insecure profiles
* Custom config now available with Trojan-Go
* Bug fixes

#### 0.1-rc23

* Night mode support
* Tcp keep alive interval option now applied for shadowsocks-rust (v1.11.1)
* Bug fixes

#### 0.1-rc22

* Custom config support, read [Configuration](https://sagernet.org/configuration/) for usage
* `Early Data Header Name` option for V2Ray WebSocket settings
* Moved Xray related parts to the new fork with xray-core: [AnXray](https://t.me/AnXray)
* Bug fixes

#### 0.1-rc21

* Fixed UDP connection in ShadowsocksR profiles
* Local response to ICMP PING requests (thanks [@rprx](https://github.com/rprx))
* Improved chain settings

#### 0.1-rc20

* Bug fixes

#### 0.1-rc19

* Reverse proxy support
* Certificate options for V2Ray TLS profiles
* TCP DNS support (v2ray-core 4.40.0)
* Bug fixes

#### 0.1-rc17

* [New Brook link](https://github.com/txthinking/brook/issues/811) support
* Fixed connection test on Android L-M devices
* Minor fixes

#### 0.1-rc16

* Transparent proxy inbound support
* Option to change connection test URL
* UI Improves
* Minor fixes

#### 0.1-rc15

* Minor fixes
* Added SHA256 checksum in CI result page and GitHub release

#### 0.1-rc14

* Brook ( [txthinking/brook](https://github.com/txthinking/brook) ) support
* Minor fixes

#### 0.1-rc13

* Bug fixes

#### 0.1-rc12

* PingTunnel ( [esrrhs/pingtunnel](https://github.com/esrrhs/pingtunnel) ) support
* RelayBaton ( [iyouport-org/relaybaton](https://github.com/iyouport-org/relaybaton) ) support
* Bug fixes

#### 0.1-rc11

* NaïveProxy ( [klzgrad/naiveproxy](https://github.com/klzgrad/naiveproxy) ) support
* Bug fixes