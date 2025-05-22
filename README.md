# homekit-button-onvis

A lightweight Flask-based webhook server triggered by HomeKit-compatible Onvis buttons via Homebridge. Each button event is routed to an HTTP API call or local shell script, defined in a `config.yml` file.

---

## 🚀 Features

- HomeKit support via Eve or Homebridge dummy switches
- Button events POST to local Flask API
- YAML-based routing to:
  - External REST APIs
  - Local shell scripts
- Runs as a persistent macOS Launch Agent

---

## 🔧 Installation

### 1. Clone the repo & create virtualenv

```bash
git clone https://github.com/chrisbergeron/homekit-button-onvis.git
cd homekit-button-onvis
python3 -m venv cb
source cb/bin/activate
pip install -r requirements.txt
```

### 2. Create a config file

```yaml
# config.yml
button1:
  type: api
  url: http://localhost:7000/hello
  method: POST
  headers:
    Content-Type: application/json
  body: '{"event": "button1"}'

button2:
  type: shell
  command: "/usr/local/bin/script.sh arg1"
```

### 3. Test it locally

```bash
python onvis_webhook.py --config config.yml
```

---

## 🔄 Enable on Boot (macOS Launch Agent)

1. Copy or symlink the plist:

```bash
ln -s "$(pwd)/com.chrisbergeron.homekitbutton.plist" ~/Library/LaunchAgents/
```

2. Load it:

```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.chrisbergeron.homekitbutton.plist
```

3. Restart later with:

```bash
launchctl kickstart -k gui/$(id -u)/com.chrisbergeron.homekitbutton
```

---

## 📡 API Endpoint

Trigger a button manually:

```bash
curl -X POST http://localhost:5055/api/onvis/button1
```

---

## 🧪 Logs

```bash
tail -F ~/Library/Logs/onvis_webhook.log
tail -F ~/Library/Logs/onvis_webhook.err
```

---

## 🧰 Development Tips

- Edit `config.yml` to add buttons or change behavior
- Restart the process after config changes:

```bash
launchctl kickstart -k gui/$(id -u)/com.chrisbergeron.homekitbutton
```

- To test the Python entry manually:

```bash
cb/bin/python3 onvis_webhook.py --config config.yml
```

MIT — © 2025 Chris Bergeron

74debcf5-c182-4d92-9f1e-42f0799c9bff
