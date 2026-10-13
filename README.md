# RDNA Forge

Estudio de imágenes **local**, con interfaz parecida a Stable Diffusion WebUI.
Pensado para AMD Radeon (RX 9070 y similares) vía **DirectML**.

No usa Grok. No llama a ninguna API de imágenes. Con el modelo ya descargado funciona **sin internet**.

## Windows (lo más simple)

1. Clona este repositorio.
2. Doble clic en **Launcher.exe** si lo compilaste, o ejecuta el instalador:

```powershell
git clone https://github.com/BakaNekoNyaa-Pixel/rdna-forge.git
cd rdna-forge
powershell -ExecutionPolicy Bypass -File .\windows\install.ps1
.\windows\Star.bat
```

`install.ps1` descarga Python embebido, PyTorch DirectML y Stable Diffusion 1.5.
Hace falta internet **solo esa vez**.

3. **Star.bat** arranca el motor y abre http://127.0.0.1:7860/

No cierres la ventana negra: es el motor.

## A mano

```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r engine\requirements-windows.txt
python engine\download_model.py
python engine\server.py
```

Luego abre http://127.0.0.1:7860/

## Qué protege tu PC

- Una sola generación a la vez. Interrumpir la cancela.
- En CPU el lado largo se queda en 512 px y los pasos en 20.
- En GPU (DirectML) el lado largo no pasa de 2048. 4K es un segundo pase (hires), no un lienzo de 3840 en el UNet: así no se llena la VRAM ni se reinicia Windows.
- Atención en rodajas y VAE teselado van activos.

## Samplers

Euler, Euler a, Heun, LMS, DDIM, DDPM, PNDM, DPM2, DPM2 a, DPM++ 2M, DPM++ 2M Karras, DPM++ SDE, DPM++ SDE Karras, DPM++ 2M SDE, DPM++ 3M SDE, UniPC, LCM, Restart.

## Compilar Launcher.exe

```
cd windows/launcher
GOOS=windows GOARCH=amd64 go build -ldflags "-s -w" -o ../../Launcher.exe .
```

El zip listo para Windows incluye Launcher.exe (icono de chica anime), Star.bat e install.ps1.
