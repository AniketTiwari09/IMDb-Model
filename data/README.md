# Dataset schemas

Dataset files are not bundled. Supply the original datasets under the paths expected by the notebooks.

## Single-label data

`Data/train_data.txt` and `Data/test_data_solution.txt` contain one record per line with four fields separated by `:::`:

```text
ID ::: TITLE ::: GENRE ::: DESCRIPTION
```

`Data/test_data.txt` omits the label:

```text
ID ::: TITLE ::: DESCRIPTION
```

The provided source archive has 54,214 training records, 54,200 test records, and 27 genre labels. `Data/description.txt` in the original archive identifies `ftp://ftp.fu-berlin.de/pub/misc/movies/database/` as the data source; availability of that historical endpoint has not been verified.

## Multi-label data

`Data_Multilabel/movies.csv` must include `MOVIES`, `GENRE`, and `ONE-LINE`. `GENRE` is a comma-separated list of genres. The source CSV also contains `YEAR`, `RATING`, `STARS`, `VOTES`, `RunTime`, and `Gross`; these are not model inputs.

The saved notebook reports 8,662 cleaned rows. The currently supplied CSV does not reproduce that count using the recorded preprocessing, so retain and identify the dataset version when repeating experiments.
