# OS検出
## 1.テクニック

- **-Oオプションを使用する**
  - これにより、指定したターゲットのOSを特定する
```bash
nmap -O <target>
```

- **アグレッシブスキャン**
  - OS検出ともに、サービス/バージョン検出も行う
```bash
nmap -A <target>
```
