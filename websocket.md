# library - websockets

websocket reconnect mechanism

```python
async for websocket in websockets.connect(f"ws://192.168.29.18:8000/ws/serial_communication/producer/"):
                try:
                    async for message in websocket:
                        print(message)
                except websockets.ConnectionClosed:
                    continue
```
