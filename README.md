# Blanket Exporter

Blanket Exporter is a command-line tool (CLI tool) that’s available to all user. You can use the Blanket Exporter to
export your data, such as list templates, list entries, users, and locations, to multiple formats that can be used for
business intelligence tools or record keeping.

This guide shows you how to configure and run the Blanket Exporter to bulk export your organization's data.

<!-- TOC -->
* [Before you begin](#before-you-begin)
* [Ready to get started?](#ready-to-get-started)
  * [Configure](#configure)
  * [Configure Options](#configure-options)
  * [Export data](#export-data)
    * [Export To CSV](#export-to-csv)
    * [Export Data on a Recurring Basis](#export-data-on-a-recurring-basis)
      * [Schedule exports on Windows](#schedule-exports-on-windows)
      * [Schedule exports on Mac OS/Linux](#schedule-exports-on-mac-oslinux)
<!-- TOC -->
## Before you begin

Please note that you must have an `API key` provided by the Blanket administrator to configure and run the Blanket
Exporter. Please contact your administrator to obtain an API key before proceeding.

As the Blanket Exporter runs on a command-line interface (CLI), we recommend that you have some basic knowledge of
running command lines before configuration. The Blanket Exporter allows you to export data from Blanket to CSV format.
It supports Windows, macOS and Linux platforms.

## Ready to get started?

The Blanket Exporter needs to be configured on first use by creating a config file, but this configuration step is only
required once - subsequent runs will use the existing config.

Notes: Each time you run an export, remember to navigate to the directory where you stored the Blanket Exporter. Learn
how to change directory or folder using command lines
on [macOS](https://github.com/0nn0/terminal-mac-cheatsheet#core-commands)
and [Windows](https://docs.microsoft.com/windows-server/administration/windows-commands/cd).

### Configure

1. Open your command-line application. This can be “PowerShell” for Windows and “Terminal” for macOS.
2. In your command line window, navigate to the location where you stored the Blanket Exporter. Learn how to change
   directory or folder using command lines on [macOS](https://github.com/0nn0/terminal-mac-cheatsheet#core-commands)
   and [Windows](https://docs.microsoft.com/windows-server/administration/windows-commands/cd).
3. Run the following command line:
   ```
   ./blanket-exporter configure --api-key <your_api_key>
   
   # Example: ./blanket-exporter configure --api-key abc123
   # <your_api_key> is the API key provided by your Blanket administrator.
   ```
   P/S: If you're using Windows Command Prompt rather than PowerShell, the command above may return an error. If you get
   an error around "." not being recognized, run .\blanket-exporter configure orblanket-exporter configure.

4. After run command line, you will be got a file "`config.yaml`". If you want to change the configuration, you can
   edit this file.

### Configure Options

Blanket Exporter relies on configuration files to determine what and where to export data and from which account's
perspective.

When you download and configure the Blanket Exporter for the first time, you will come across the following
configuration options in the default "`config.yaml`" file:

```config
api_key: <your_api_key>
api_url: <blanket_api_url>
log_mode: <log_mode>
after_date: <timestamps_milliseconds>
csv:
    max_rows_per_file: 100000
export:
    path: export
    type: csv
    limit: 100
dataset:
    tables: []
    issue:
        limit: 100
    list_entry:
        limit: 100
    list_entry_report:
        limit: 100
    list_template:
        limit: 100
    photo_task_entry:
        limit: 100
```

The following table outlines each configuration field's purpose and accepted values:

| Field                 | Description                                                                                                                                                                                                                                          | Example        |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|
| api_key               | Your API Key (contact your administrator to obtain an API key)                                                                                                                                                                                       | 189...MJm2zv50 |
| api_url               | The Blanket API URL. The default value should be used at all times.                                                                                                                                                                                  |                |
| log_mode              | The exporter provides a log mode to show the progress of the data export. `processing`, `info` and `none`                                                                                                                                            | processing     |
| after_date            | Specify the timestamp to start exports from, based on the date list_entries, task_entries, and issues that are modified, in Coordinated Universal Time (UTC). ***This start timestamp option is only applicable when scheduling recurring exports*** | 1704168205754  |
| csv.max_rows_per_file | Each exported CSV file is limited to a maximum of 100,000 records. If your data export exceeds this limit, it will be split into multiple files.                                                                                                     | 100000         |
| export.path           | The absolute or relative path to save exported data.                                                                                                                                                                                                 | export         |
| export.limit          | The limit for the number of data that gets processed per batch.                                                                                                                                                                                      | 100            |
| dataset.tables        | The tables to export. By default, the Blanket Exporter exports all data set tables, unless specified: `["user"]` - *only export user data*.                                                                                                          | []             |
| dataset.[...].limit   | The limit for the number of data (`issue`, `list_entry`, `list_entry_report`, `list_template`, `photo_task_entry`) that gets processed per batch.                                                                                                    | 100            |


### Export data

#### Export To CSV
Run the following command line to export data to CSV for recurring:
```
./blanket-exporter export-to-csv

# Example: ./blanket-exporter export-to-csv --from-date 1701059980337 --tables list_entries --log-mode processing
# Params:
      --api-key string                API Key
      --api-url string                API URL
  -p, --export-path string            File Export Path (default "export")
  -e, --export-type string            Export type. Currently supported: csv (default "csv")
  -f, --from-date int                 Milliseconds - the start date for data extraction (default 0 - the time of available data)
  -h, --help                          help for export-to-csv
      --issue-limit int               Number of issues fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
      --list-entry-limit int          Number of list_entries fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
      --list-entry-report-limit int   Number of list_entry_reports fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
      --list-template-limit int       Number of list_templates fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
      --log-mode string               Log Mode (info, processing, none) (default "processing")
  -r, --max-rows-per-file int         Maximum number of rows in a csv file. New files will be created when reaching this limit. (default 100000)
      --photo-task-entry-limit int    Number of photo_task_entries fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
  -l, --query-limit int               Number of data fetched at once. Lower this number if the app_exporter fails to load the data (default 100)
      --tables strings                Export tables to export (default all)
  -t, --to-date int                   Milliseconds - the end date for data extraction (default 0 - the current time when data extracting)

```
---------------------------------------

#### Export Data on a Recurring Basis
Run the following command line to export data to CSV on a Recurring Basis:
```
./blanket-exporter export

# Example: ./blanket-exporter export
# Params:
      --api-key string          API Key
      --api-url string          API URL
  -h, --help                    help for export
      --log-mode string         Log Mode (info, processing, none) (default "processing")
  -r, --max-rows-per-file int   Maximum number of rows in a csv file. New files will be created when reaching this limit. (default 100000)
```

##### Schedule exports on Windows
On Windows, the easiest way to schedule your exports is to use the built-in Windows Scheduler. These instructions are based on Windows 10 but you should be able to adapt them to other versions. These instructions are assuming you've already configured the tool and have a properly set up config file in a folder alongside the Blanket Exporter. If you haven't done so yet, please go back and do that first.

1. Click Start and search for "Scheduler". Then, select "Task Scheduler".
2. Click Create Basic Task over on the right-hand side.
3. Give your task a useful name. For example, "Blanket Exporter Schedule".
4. Click Next and set your trigger. Either "Daily" or any other interval that you prefer.
5. Click Next. Windows Scheduler may give you additional options around when the task should run. Configure it as you need. 
6. Click Next and select Start a program.
7. Click Browse and navigate to the folder where you set up the Blanket Exporter. Then, select the "blanket-exporter.exe" file. 
8. In "Add Arguments", enter "export" for the command to run blanket-exporter.exe for a recurring. If you haven't configured a YAML file, you can also enter additional parameters here. However, we recommend that you use the YAML file as it's much easier. 
9. In "Start In", paste in the full path to the folder containing "blanket-exporter.exe". 
10. Click Next and review your settings. 
11. Click Finish. 
12. You can test the schedule by clicking Run on the lower-right.

Notes: ***This application is built with Golang, which may result in false positive alerts from Windows Defender. If you receive any warnings, they can safely be ignored - the exporter is secure and harmless.***

##### Schedule exports on Mac OS/Linux
We recommend using cron to schedule on these systems.
