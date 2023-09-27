# Steam

On running steam in Linux using `Flatpak`

## Run exe in same Proton env

Ex running `toggle_anti_cheat` for elden ring in `Proton 8.0` :

```bash
export STEAM_COMPAT_DATA_PATH=~/.local/share/Steam/steamapps/compatdata
export STEAM_COMPAT_CLIENT_INSTALL_PATH=~/.local/share/Steam
cd /media/Games/flatpak-steam-library/SteamLibrary/steamapps/common/ELDEN\ RING/Game
~/.local/share/Steam/steamapps/common/Proton\ 8.0/proton run toggle_anti_cheat.exe
```
