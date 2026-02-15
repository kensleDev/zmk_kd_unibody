# Unibody 36 - ZMK Wireless Keyboard

A 36-key unibody wireless keyboard powered by ZMK firmware and nice!nano controller.

## 🎯 Features

- **Wireless** via Bluetooth Low Energy (BLE)
- **Home row mods** (GACS: GUI, ALT, CTRL, SHIFT)
- **4 layers**: Main (QWERTY), NUM (numbers + F-keys), SYM (symbols + nav), BLE (Bluetooth controls)
- **300ms tapping term** with tap-preferred behavior
- **Battery monitoring** built-in
- **Power saving** with sleep mode after 30 minutes
- **Key repeat** on thumb for convenience

## 📦 Hardware

- **Controller**: nice!nano v2
- **Battery**: 3.7V 500mAh LiPo (JST connector)
- **Switches**: 36× mechanical switches
- **Diodes**: 36× 1N4148 (COL2ROW)
- **Expected battery life**: 2-4 weeks per charge

## 🗺️ Layout

```
┌───┬───┬───┬───┬───┐       ┌───┬───┬───┬───┬───┐
│ Q │ W │ E │ R │ T │       │ Y │ U │ I │ O │ P │
├───┼───┼───┼───┼───┤       ├───┼───┼───┼───┼───┤
│GUI│ALT│ D │CTL│NUM│       │NUM│CTL│ K │ALT│GUI│
│ A │ S │   │ F │ G │       │ H │ J │   │ L │ ; │
├───┼───┼───┼───┼───┤       ├───┼───┼───┼───┼───┤
│ Z │ X │ C │ V │ B │       │ N │ M │ , │ . │ / │
└───┴───┴───┴───┴───┘       └───┴───┴───┴───┴───┘
          ┌───┬───┬───┐   ┌───┬───┬───┐
          │REP│BKS│SFT│   │SFT│SYM│TAB│
          │   │   │ESC│   │RET│SPC│   │
          └───┴───┴───┘   └───┴───┴───┘
```

### Home Row Mods

**Left hand (GACS)**:
- `A` → GUI when held, A when tapped
- `S` → ALT when held, S when tapped
- `D` → Pure D (no modifier)
- `F` → CTRL when held, F when tapped

**Right hand (mirrored)**:
- `J` → CTRL when held, J when tapped
- `K` → Pure K (no modifier)
- `L` → ALT when held, L when tapped
- `;` → GUI when held, ; when tapped

### Layers

**Access**:
- Hold `G` or `H` → NUM layer
- Hold `Space` → SYM layer
- Hold `ESC` on SYM layer → BLE layer

**NUM Layer**: Numbers 0-9, F1-F12, media controls
**SYM Layer**: Symbols, brackets, navigation (arrows, home/end/pgup/pgdn)
**BLE Layer**: Bluetooth device switching (BT0-BT4), clear bonds

## 🚀 Setup & Build

### Option 1: GitHub Actions (Recommended)

1. **Fork this repo** or create a new repo with these files

2. **Push to GitHub**:
   ```bash
   cd zmk
   git init
   git add .
   git commit -m "Initial ZMK config for Unibody 36"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/zmk-unibody36.git
   git push -u origin main
   ```

3. **Download firmware**:
   - Go to **Actions** tab in your repo
   - Click on the latest workflow run
   - Download the `firmware` artifact
   - Extract `unibody36-nice_nano_v2-zmk.uf2`

### Option 2: Local Build

```bash
# Clone ZMK
git clone https://github.com/zmkfirmware/zmk.git
cd zmk

# Setup west
west init -l app/
west update

# Build firmware
west build -p -b nice_nano_v2 -- -DSHIELD=unibody36 -DZMK_CONFIG="/path/to/this/repo/config"

# Firmware will be at: build/zephyr/zmk.uf2
```

## 📲 Flashing

1. **Plug in nice!nano** via USB

2. **Enter bootloader mode**:
   - Double-press the reset button on nice!nano
   - A drive named `NICENANO` should appear

3. **Copy firmware**:
   - Drag and drop `unibody36-nice_nano_v2-zmk.uf2` to the `NICENANO` drive
   - The drive will disconnect automatically
   - nice!nano will reboot with new firmware

4. **Test**:
   - Open Bluetooth settings on your computer
   - Look for "Unibody 36"
   - Pair and start typing!

## 🔋 Battery Setup

1. **Connect LiPo battery** to nice!nano's JST connector
   - Red wire → + (positive)
   - Black wire → - (ground/negative)

2. **Battery monitoring**:
   - ZMK reports battery level over Bluetooth
   - Check your OS's Bluetooth battery indicator

3. **Charging**:
   - Plug in USB-C cable to nice!nano
   - Battery charges automatically
   - Orange LED indicates charging
   - Green LED indicates fully charged

## 🎛️ Customization

### Editing Your Keymap

1. Edit `config/unibody36.keymap`
2. Commit and push changes
3. GitHub Actions automatically builds new firmware
4. Download and flash

### Common Tweaks

**Change tapping term**:
```c
&mt {
    tapping-term-ms = <250>;  // Adjust this value
    flavor = "tap-preferred";
};
```

**Add combos**:
```c
/ {
    combos {
        compatible = "zmk,combos";
        combo_esc {
            timeout-ms = <50>;
            key-positions = <0 1>;  // Q + W
            bindings = <&kp ESC>;
        };
    };
};
```

**Modify layers**: Edit the `bindings` arrays in each layer

## 🔧 Troubleshooting

**Keyboard not pairing**:
- Make sure you flashed the firmware successfully
- Try clearing Bluetooth bonds (BLE layer, press `CLR`)
- Restart your computer's Bluetooth

**Keys not registering**:
- Check your matrix wiring matches the overlay file
- Verify diode direction (cathode towards switches)
- Test continuity with multimeter

**Battery not charging**:
- Check polarity (red = +, black = -)
- Try different USB cable
- Ensure nice!nano's charging circuit is working

**Weird behavior with home row mods**:
- Adjust `tapping-term-ms` in keymap (try 200-350ms)
- Try changing `flavor` to "hold-preferred"
- Consider using D and K as pure keys (already done!)

## 📊 Pin Configuration

**Row pins (nice!nano)**:
- Row 0 → Pin 4 (D4)
- Row 1 → Pin 5 (C6)
- Row 2 → Pin 6 (D7)
- Row 3 → Pin 7 (E6)

**Column pins (nice!nano)**:
- Col 0 → Pin 21 (F4)
- Col 1 → Pin 20 (F5)
- Col 2 → Pin 19 (F6)
- Col 3 → Pin 18 (F7)
- Col 4 → Pin 15 (B1)
- Col 5 → Pin 14 (B3)
- Col 6 → Pin 16 (B2)
- Col 7 → Pin 10 (B6)
- Col 8 → Pin 9 (B5)
- Col 9 → Pin 8 (B4)

## 🎨 The Analogy

**ZMK config repo** = Tailwind config file
- Edit files, commit, rebuild, see changes
- Much faster than traditional firmware builds
- Configuration as code

**GitHub Actions** = CI/CD pipeline
- Automatic builds on every commit
- Download pre-compiled firmware
- No local build environment needed

**nice!nano flashing** = Dragging a file to deploy
- Simple UF2 bootloader
- No special tools required
- Instant updates

## 📚 Resources

- [ZMK Documentation](https://zmk.dev/)
- [nice!nano Pinout](https://nicekeyboards.com/docs/nice-nano/pinout-schematic)
- [ZMK Discord](https://zmk.dev/community/discord/invite)
- [Home Row Mods Guide](https://precondition.github.io/home-row-mods)

## 📄 License

MIT

---

**Happy wireless typing!** 📡⌨️✨
