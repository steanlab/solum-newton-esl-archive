# Required pyOCD pack

Required CMSIS Device Family Pack:

```text
SiliconLabs.GeckoPlatform_EFR32BG22_DFP.2025.12.1.pack
```

The pack must contain target:

```text
EFR32BG22C224F512IM40
```

Check it:

```bash
pyocd list --targets \
  --pack /path/to/SiliconLabs.GeckoPlatform_EFR32BG22_DFP.2025.12.1.pack \
  | grep -i efr32bg22c224f512im40
```
