## How to generate wallet?
### Install dependencies
```
sudo apt update && sudo apt list --upgradable && sudo apt upgrade -y
```
```
sudo apt install libfuse2
```
### Download wallet installer
```
wget https://github.com/OwshenNetwork/owshen/releases/download/v0.1.0/Owshen_v0.1.0_x86_64.AppImage
```
```
chmod +x Owshen_v0.1.0_x86_64.AppImage
```

untuk membuat wallet baru `jangan lupa simpen pashparsenya`
```
./Owshen_v0.1.0_x86_64.AppImage init
```
jika ingin recovery wallet yang sudah punya
```
./Owshen_v0.1.0_x86_64.AppImage init --mnemnonic "seed parsemu"
```

untuk melihat informasi wallet
```
./Owshen_v0.1.0_x86_64.AppImage info
```
