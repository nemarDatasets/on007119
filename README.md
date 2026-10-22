[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on007119-blue)](https://doi.org/10.82901/nemar.on007119)

This dataset contains intracranial EEG data recorded during non-REM sleep and used in Hatano et al. (in press).

Authors: 
Keisuke Hatano, Naoto Kuroda, Hiroshi Uda, Kazuki Sakakura, Michael J. Cools, Aimee F. Luat, Shin-Ichiro Osawa, Hitoshi Nemoto, Kazushi Ukishiro, Hidenori Endo, Nobukazu Nakasato, Yutaro Takayama, Keiya Iijima, Masaki Iwasaki, Eishi Asano

Funding: 
National Institutes of Health (NIH; NS064033 to E.A.); 
Uehara Memorial Foundation Postdoctoral Fellowship (202441017 to K.H.; 20210301 to H.U.);
Japan Society for the Promotion of Science (JP22J23281, JP22KJ0323, and 202560576 to N.K.; 202560628 to H.U.; JP19K09494 and 22K09296 to M.I.)

## Channel names follow the data headers (18 Oct 2026)

In 105 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count), so `mne_bids.read_raw_bids` stopped with 'Channel mismatch' (or 'Duplicate channel names'); EEGDash renamed the data channels by position to the channels.tsv names, which gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 10668 rows took the header name (the header labels carry '-AV' suffix).

8 header channels had no channels.tsv row (4 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-175_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: A63-AV; `sub-186_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: C63-AV; `sub-216_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: A5-AV, A6-AV.

144 channels.tsv rows had no channel in the data file (14 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-151_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: A57, A58, A59, A60, A61; `sub-162_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: C1, C2, C3, C4, C5; `sub-185_ses-01_task-sleep_acq-A_run-01_ieeg.edf`: C1, C2, C3, C4, C5.

Four removed rows of `sub-222_ses-01_task-sleep_acq-A_run-01_ieeg.edf` (N1-N4) carried a free-text clinical annotation in their channel name. In the copy kept in sourcedata the annotation is redacted (names `N1(redacted)` to `N4(redacted)`); the rows are otherwise unchanged.

1 files listed the channels in another order than the header; rows now follow the header order.

EEGDash's positional renaming had given wrong labels to 839 channels in 14 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.
