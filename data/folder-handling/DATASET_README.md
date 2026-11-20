# Folder-handling Beispieldaten

Diese Struktur ist für das Kapitel zu `os`, Pfaden, CSV-Dateien und Fehlerbehandlung gedacht.

- `input/team_alpha.csv`: gültige UTF-8-CSV
- `input/team_beta.CSV`: gültige UTF-8-CSV mit großgeschriebener Endung
- `input/archive/2025/team_old.csv`: gültige CSV im Unterordner
- `input/archive/2026/team_new.csv`: gültige CSV im Unterordner
- `input/empty.csv`: leer → `pandas.errors.EmptyDataError`
- `input/broken_parser.csv`: absichtlich kaputt → `pandas.errors.ParserError`
- `input/bad_number.csv`: CSV ist lesbar, aber `score` enthält Text → `ValueError` bei numerischer Konvertierung
- `input/latin1.csv`: Latin-1 statt UTF-8 → `UnicodeDecodeError` bei `encoding="utf-8"`
- `input/photo.jpg`: wird nur benutzt, um Dateiendung und tatsächlichen Inhalt zu unterscheiden; nicht als Bild einlesen
- `input_invalid_only/`: nur fehlerhafte CSV-Dateien, um den Null-gültige-Dateien-Fall zu testen
- `output/combined_scores.csv`: absichtlich bereits vorhanden, um Überschreiben zu demonstrieren
