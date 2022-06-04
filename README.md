Adding remotes from the [Flipper-IRDB project](https://github.com/UberGuidoZ/Flipper-IRDB/tree/main/TVs) into the Flipper Zero's Universal Remote functionality.

# Files in this repo

- `tv.ir` - Full Universal Remote containing [Flipper-IRDB](https://github.com/UberGuidoZ/Flipper-IRDB/tree/main/TVs) commands merged with the [`tv.ir` file from the Flipper Zero Unleashed firmware](https://github.com/Eng1n33r/flipperzero-firmware/blob/dev/assets/resources/infrared/assets/tv.ir).
- `tv.ir.append` - Only the parsed remotes from Flipper-IRDB. Recommend merging with the `tv.ir` file from the Unleashed firmware. Doesn't include any power button controls because those are well covered already. See note at bottom of the Verification section below on the best way to merge the files.

# Verification

1. Git clone the Flipper-IRDB repository and cd into the TVs directory:

```
git clone https://github.com/UberGuidoZ/Flipper-IRDB/
cd Flipper-IRDB-main/TVs
```

2. Run the commands to populate `~/tv.ir.append`:

```
grep -RiEh 'channel\+|channel_up|chan_up|chanup|ch_up' -A 5 | grep -v '\--' | sed 's/name.*/name: CH+/' >> ~/tv.ir.append

grep -RiEh 'chan_down|chandown|chan_dwn|channel\-|channel_dn|channel_down|ch_down|ch_dwn' -A 5 | grep -v '\--' | sed 's/name.*/name: CH-/' >> ~/tv.ir.append

grep -RiEh 'mute' -A 5 | grep -v '\--' | sed 's/name.*/name: MUTE/' >> ~/tv.ir.append

grep -RiEh 'v\+|volume\+|volume_up|volumeup|vol_up|volup|v_up' -A 5 | grep -v '\--' | sed 's/name.*/name: VOL+/' >> ~/tv.ir.append

grep -RiEh 'v\-|v_down|v_dwn|vol_dn|vol_down|voldown|vol_dwn|voldwn|volume\-|volume_down|volumedown' -A 5 | grep -v '\--' | sed 's/name.*/name: VOL-/' >> ~/tv.ir.append
```

3. Manually go through the `~/tv.ir.append` output an ensure a '#' character is between each command and remove any duplicates. Remove any entries that have "protocol: RCA" or else the remote will freeze or stop before sending all the commands. TODO: Automate this step.

4. Copy the contents of the output file `~/tv.ir.append` and paste it into your existing universal TV remote file on your Flipper SD card at `/infrared/assets/tv.ir`.

Note: When merging, paste near the middle of the existing `tv.ir` file between existing entries and a bit past the large block of only POWER controls. This is because the file is read from the top to the bottom by the Flipper somewhat slowly. If pasted at the very top, the POWER button commands will take a bit longer to start. If pasted at the bottom, it can take a while to get past all the POWER entries to start. Double check that there aren't any duplicates introduced and the formatting is consistent.
