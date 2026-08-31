# ⌨️ Remote_Keyboard — Low-Latency TCP Socket Input Bridge

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![TCP Sockets](https://img.shields.io/badge/Network-Raw_TCP_Sockets-008080?style=for-the-badge)](https://docs.python.org/3/library/socket.html)
[![pynput](https://img.shields.io/badge/Input-pynput_Controller-orange?style=for-the-badge)](https://pynput.readthedocs.io/)
[![Multi-Monitor](https://img.shields.io/badge/Display-screeninfo-blueviolet?style=for-the-badge)](https://github.com/rr-/screeninfo)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

A high-speed, lightweight TCP socket bridge for remote hardware input injection. It enables controlling a host machine's mouse and keyboard seamlessly across local networks with sub-millisecond packet latency, multi-monitor coordinate scaling, and full keycode translation.

---

## 🔄 Network & Input Architecture

```mermaid
sequenceDiagram
    participant Client as 📱 Remote Client (GUI / Mobile)
    participant Socket as 🌐 TCP Socket Server (Port 8000)
    participant Dispatcher as ⚙️ Threaded Event Dispatcher
    participant OS as 🖥️ OS Input Subsystem (pynput)

    Client->>Socket: Connect over LAN (192.168.x.x:8000)
    Socket-->>Client: Handshake & Screen Dimension Sync (WxH)
    loop Input Streaming
        Client->>Socket: Byte Packet [e.g. 'click,left', 'key,ctrl+c', 'move,500,300']
        Socket->>Dispatcher: Decode & Normalize Coordinates
        Dispatcher->>OS: Inject Hardware Keypress / Cursor Position
    end
```

---

## ⚡ Key Capabilities

- **🚀 Sub-Millisecond Input Latency:** Direct raw TCP socket streaming without heavyweight HTTP/WebSocket wrappers.
- **🖱️ Multi-Monitor Resolution Scaling:** Integrates with `screeninfo` to map relative touch coordinates to absolute pixel boundaries across multiple displays.
- **⌨️ Comprehensive Keycode Mapping:** Full support for standard alphanumeric keystrokes, modifier combinations (Ctrl, Shift, Alt, Cmd), and special function keys.
- **🧵 Thread-Safe Connection Pooling:** Multi-threaded server daemon handles disconnections, reconnects, and concurrent event packets gracefully.

---

## 📂 File Structure

```
Remote_Keyboard/
├── remote_server.py      # Multithreaded TCP server receiving packets & injecting inputs
├── remote_keyboard.py    # Client interface / packet generator
└── README.md             # Technical documentation
```

---

## 🚀 Getting Started

### Installation

```bash
# 1. Clone repository
git clone https://github.com/abhay2008/Remote_Keyboard.git
cd Remote_Keyboard

# 2. Install dependencies
pip install pynput screeninfo
```

### Running the Server (Host Machine to be Controlled)

```bash
# Start host listener daemon
python remote_server.py
```

### Running the Client (Controller Device)

```bash
# Connect and stream input events
python remote_keyboard.py
```

---

## 👨‍💻 Author

**Abhay Kashyap**
- GitHub: [@abhay2008](https://github.com/abhay2008)
