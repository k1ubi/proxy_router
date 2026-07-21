<div align="center">

```
____________ _______   ____   __ ______ _____ _   _ _____ ___________ 
| ___ \ ___ \  _  \ \ / /\ \ / / | ___ \  _  | | | |_   _|  ___| ___ \
| |_/ / |_/ / | | |\ V /  \ V /  | |_/ / | | | | | | | | | |__ | |_/ /
|  __/|    /| | | |/   \   \ /   |    /| | | | | | | | | |  __||    / 
| |   | |\ \\ \_/ / /^\ \  | |   | |\ \\ \_/ / |_| | | | | |___| |\ \ 
\_|   \_| \_|\___/\/   \/  \_/   \_| \_|\___/ \___/  \_/ \____/\_| \_|
```

`[ neo-regeorg tunnel orchestration :: socks5 injection :: proxychains autopilot ]`

![python](https://img.shields.io/badge/PYTHON-3.x-ff00c8?style=for-the-badge&logo=python&logoColor=00fff9&labelColor=0a0014)
![license](https://img.shields.io/badge/LICENSE-GPLv3-00fff9?style=for-the-badge&labelColor=0a0014)
![status](https://img.shields.io/badge/STATUS-OPERATIONAL-ff00c8?style=for-the-badge&labelColor=0a0014)

</div>

<br>

```
▓▒░ 0x00 // SITREP ░▒▓
```

Web shells die the moment someone rotates a backdoor URL or a tunnel gets killed mid-engagement.
`proxy_router` collapses every [Neo-reGeorg](https://github.com/L-codes/Neo-reGeorg) backdoor you're
running into **one JSON manifest**, then gives you a single CLI to spin a tunnel up, shove it into
`proxychains`, and tear it back down — no more hunting through terminal history for the right
`neoreg.py` invocation.

<br>

```
▓▒░ 0x01 // TOPOLOGY ░▒▓
```

```
   [ operator ]
        │
        │  proxychains4  (socks5 :: 127.0.0.1:<PORT>)
        ▼
 ┌────────────────┐              reads               ┌───────────────────────┐
 │   router.py    │ ─────────────────────────────────▶│  Proxies/proxy.json   │
 │  (this repo)   │                                    │  id · country · url  │
 └───────┬────────┘                                    └───────────────────────┘
         │ spawns
         ▼
 ┌────────────────┐        encrypted tunnel        ┌──────────────────────────┐
 │  Neo-reGeorg   │ ═══════════════════════════════▶│   target webshell        │
 │  (neoreg.py)   │◀═══════════════════════════════ │   e.g. /tunnel.php       │
 └────────────────┘                                  └──────────────────────────┘
```

<br>

```
▓▒░ 0x02 // LOADOUT ░▒▓
```

| requirement | purpose |
|---|---|
| `python3 -m pip install requests psutil` | HTTP reachability checks + process control |
| `proxychains` (`proxychains4.conf`) | routes arbitrary tools through the spawned tunnel |
| [`Neo-reGeorg`](https://github.com/L-codes/Neo-reGeorg) | dropped in the project root — this *is* the tunnel engine |

<br>

```
▓▒░ 0x03 // CAPABILITIES ░▒▓
```

- `►` list every proxy registered in the manifest
- `►` select a proxy by ID and bind it to a local port
- `►` auto-check the target is alive before wasting a port on it
- `►` push the tunnel straight into your `proxychains4.conf` routing table
- `►` strip `#router_tunnel`-tagged entries back out when you're done
- `►` kill every running `neoreg.py` process in one shot

<br>

```
▓▒░ 0x04 // USAGE ░▒▓
```

```console
root@node:~/proxy_router# python3 router.py --list
ID: 1, Type: Neo-ReGeorg, Country: Local

root@node:~/proxy_router# python3 router.py --proxy-id 1 --port 8080
Executing: python3 Neo-reGeorg/neoreg.py -k examplepsw -u http://127.0.0.1/tunnel.php -p 8080 --skip

root@node:~/proxy_router# python3 router.py --proxy-id 1 --port 8080 --proxychain
[SUCCESS] Added proxy to /etc/proxychains4.conf: socks5 127.0.0.1 8080 #router_tunnel

root@node:~/proxy_router# python3 router.py --killall
[INFO] Terminated 1 Neo-reGeorg process(es).

root@node:~/proxy_router# python3 router.py --clean
[SUCCESS] Removed all #router_tunnel entries from proxychains config.
```

<br>

```
▓▒░ 0x05 // MANIFEST FORMAT ░▒▓
```

Every backdoor you're holding lives in `Proxies/proxy.json` as one entry:

```json
{
    "id": "1",
    "country": "Local",
    "url": "http://127.0.0.1/tunnel.php",
    "psw": "examplepsw",
    "type": "Neo-ReGeorg"
}
```

<br>

```
▓▒░ 0x06 // RULES OF ENGAGEMENT ░▒▓
```

> Built for authorized penetration tests, CTFs, and red-team engagements you have explicit
> permission to run. Point this at anything you don't own or aren't contracted to test and
> that's on you, not the tool.

<br>

<div align="center">

`GNU GPLv3` — see [`LICENSE`](./LICENSE)

</div>
