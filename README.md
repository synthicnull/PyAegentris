# PyAegentris

PyAegentris is a build-time tool for protecting Python applications on Windows. It turns your source into an encrypted runtime package and can optionally produce a standalone EXE for distribution.

![PyAegentris Protector GUI](https://raw.githubusercontent.com/synthenull/PyAegentris/refs/heads/main/screenshots/pyaegentris_gui.png)

---

## Features

- **No source in the release** - end users receive a protected runtime package, not your original `.py` files.
- **Strong encryption** - application logic is protected at build time and loaded through a native runtime module.
- **Optional EXE packaging** - bundle a single-file or folder-based executable with PyInstaller.
- **Only-EXE deliverable** - pack and keep a single `.exe` under `output/<project>/` with no intermediate package files left behind.
- **Extra protector support (VMProtect/Themida)** - pause the build to apply a third-party protector before packaging, when you need a stronger release.
- **Machine binding** - optional HWID lock and Windows-specific key binding for licensed deployments.
- **GUI and CLI** - protect from the desktop app or the command line with the same engine.

---

## Supported environment

- **OS:** Windows 10/11 (64-bit)
- **Python:** 3.12, 3.13, or 3.14

Use the same Python feature release for building and running protected output when possible.

---

## Quick start

1. **Get the project**

```bat
git clone https://github.com/synthenull/PyAegentris.git
cd PyAegentris
py -3.12 -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

2. **Protect a script**

```bat
pyaegentris create examples\example_1.py
```

Protected output is written under `output/<project>/` (for example `output\example_1\main.py` plus the native runtime).

3. **Run the protected app**

```bat
python output\example_1\main.py
```

**Optional - build an EXE** (keeps the protected package; EXE under `dist\`)

```bat
pyaegentris create examples\example_1.py --pack
```

**Optional - single EXE only** (no `main.py` / `.pyd` leftovers)

```bat
pyaegentris create examples\example_1.py --only-exe -w --uac-admin --no-icon
```

**Optional - harden before EXE**

```bat
pyaegentris create examples\example_1.py --pack --wait-protect
```

Follow the on-screen prompt, apply your external protector, then continue.

**GUI**

```bat
run_gui.bat
```

**CLI help**

```bat
pyaegentris -h
pyaegentris create -h
pyaegentris -v
```

---

## Common options

```text
pyaegentris create <source.py> [options]
```

| Option | Purpose |
|--------|---------|
| `--pack` | Build an EXE after protect (`output/<project>/dist/<name>.exe`) |
| `--only-exe` | Pack to a single EXE and remove package leftovers (`output/<project>/<name>.exe`) |
| `--wait-protect` | Pause for external hardening before pack |
| `--open-folder` | Open `output/<project>/` when finished |
| `-p`, `--project` | Output folder name |
| `--name` | EXE base name (default: project name) |
| `-w`, `--noconsole` | Hide the console window |
| `--uac-admin` | Request administrator elevation for the EXE |
| `--no-icon` / `-i` | Default icon, or custom `.ico` |
| `--bind-hwid` | Lock execution to the build machine fingerprint |
| `--dpapi-bind` | Bind protection to this Windows installation |
| `--force` | Force rebuild of the native runtime |

You can also run `pyaegentris your_app.py` as shorthand for `create`.

For the full flag list: `pyaegentris create -h`.

---

## Suggested release flow

1. Protect your application  
2. Apply external hardening if your threat model requires it (`--wait-protect`)  
3. Pack with `--pack`, or ship a single file with `--only-exe`  
4. Ship only the release artifact - not your source or builder keys  

Builder keys and local metadata stay on your development machine; they are not part of the customer package.

---

## License

Proprietary - see [LICENSE](LICENSE). Redistribution of the tool is not permitted unless your agreement allows it.

---

## Links

- **Repository:** [github.com/synthenull/PyAegentris](https://github.com/synthenull/PyAegentris)
- **Changelog:** [docs/RELEASES.md](docs/RELEASES.md)

---

## Getting help

Open an issue on GitHub with your Python version and the command or GUI steps you used. Include log output when reporting a build failure.
