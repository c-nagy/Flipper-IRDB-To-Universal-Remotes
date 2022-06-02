# What's new

The universal TV remote funtion in the Flipper Zero firmwares Vanilla and Unleashed contains hundreds of POWER commands, but only three unique CH+, CH-, VOL+, VOL-, and MUTE commands.

Parsing and adding the remotes from the Flipper-IRDB project (https://github.com/UberGuidoZ/Flipper-IRDB/) would greatly increase compatibility, which is what I'm attempting to do here. 

# Verification

1. Git clone the Flipper-IRDB repository and cd into the TVs directory:

```
git clone https://github.com/UberGuidoZ/Flipper-IRDB/
cd Flipper-IRDB-main/TVs
```

2. Run the commands to populate `~/tv.ir`:

```
grep -RiEh 'channel\+|channel_up|chan_up|chanup|ch_up' -A 5 | grep -v '\--' | sed 's/name.*/name: CH+/' >> ~/tv.ir

grep -RiEh 'chan_down|chandown|chan_dwn|channel\-|channel_dn|channel_down|ch_down|ch_dwn' -A 5 | grep -v '\--' | sed 's/name.*/name: CH-/' >> ~/tv.ir

grep -RiEh 'mute' -A 5 | grep -v '\--' | sed 's/name.*/name: MUTE/' >> ~/tv.ir

grep -RiEh 'v\+|volume\+|volume_up|volumeup|vol_up|volup|v_up' -A 5 | grep -v '\--' | sed 's/name.*/name: VOL+/' >> ~/tv.ir

grep -RiEh 'v\-|v_down|v_dwn|vol_dn|vol_down|voldown|vol_dwn|voldwn|volume\-|volume_down|volumedown' -A 5 | grep -v '\--' | sed 's/name.*/name: VOL-/' >> ~/tv.ir
```

3. Manually go through the `~/tv.ir` output an ensure a '#' character is between each command and remove any duplicates. Remove any entries that have "protocol: RCA" or else the remote will freeze or stop before sending all the commands. TODO: Automate this step.

4. Copy the contents of the output file `~/tv.ir` and paste it into your existing universal TV remote file on your Flipper SD card at `/infrared/assets/tv.ir`. Paste it near the middle of the file between existing entries because the file is read from the top down and can be somewhat slow especially on slower SD cards. If pasted at the very top, the POWER button commands will take a bit longer to start. If pasted at the bottom, it can take a while to get past all the POWER entries to start. Double check that there aren't any duplicates introduced and the formatting is consistent.
