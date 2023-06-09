# LogMan.io Export beta

<br>

- [List of Exports](#list-of-exports)
- [Export Detail](#export-detail)
- [Start Export](#start-export)
- [Custom Export (Tutorial)](#custom-export)
- [Advanced](#advanced)

<br>

## List of Exports

<span style="color:lightblue"> The new Export funtionality is now in _beta_. </span>  
<br>

|                                                                                                               |                                                                    |
| :------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------- |
| <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background-color:#a9f0b8;"></span> | <span style="color:#a9f0b8;">Successfully completed exports</span> |
| <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background-color:#EDF49C;"></span> | <span style="color:#EDF49C;">In progress exports</span>            |
| <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background-color:#ff3f5d;"></span> | <span style="color:#ff3f5d;">Failed exports</span>                 |
| <span style="display:inline-block;width:10px;height:10px;border-radius:50%;background-color:#788ecb;"></span> | <span style="color:#788ecb;">Scheduled exports</span>              |

<br>
<h4 style="color:lightblue;"> You can... </h4>

- **Create** a new export by using the <button style="background-color:#325ee3; color:white; padding:3px;">New</button> button

- **View** the details of an export by clicking on its name <span style="color:#788ecb;">export_elastic01</span>

- **Download** an export with <img style="width:20px; height:20px; vertical-align:middle;" src="download.svg"/>

- **Delete** any export with <img style="width:20px; height:20px; vertical-align:middle;" src="delete.svg"/>  
  <br>

The list of exports is **sorted** from most recent to oldest. You can **search** the list by name using the search bar <img style="width:20px; height:20px; vertical-align:middle;" src="search.svg"/>.  
<br>

<h2 style="color:#325ee3" id="export-detail">Export Detail</h2>

<h4 style="color:lightblue;"> You can... </h4>

- see the **complete** information about a specific export on the detail screen
- also download <img style="width:20px; height:20px; vertical-align:middle;" src="download.svg"/> and delete <img style="width:20px; height:20px; vertical-align:middle;" src="delete.svg"/>  
  <br>

The <span style="color:#325ee3">Restart</span> runs the export again as is or with your custom changes. Deleting a scheduled export does not delete the exports that were already generated from it.

<br>

<h2 style="color:#325ee3" id="start-export"> Start Export</h2>

<h4 style="color:lightblue;"> You can... </h4>

- see pre-made exports from the library
- **run** these directly or
- **modify** and run with these with custom changes

The <button style="color:#325ee3, padding:3px;">Custom</button> button takes you to a blank form where you can create a **self-made export**.  
<br>

<h2 style="color:#325ee3" id="custom-export">Custom Export</h2>
<h4 style="color:lightblue;">Customize your export by selecting:</h4>

- `Data Source`
  - selection will determine other export options
- `Output`
  - when selecting CSV, you must enter the column names
- `Target`
  - depends on the configuration of the export function
  - can include "e-mail" and "jupyter" targets
  - jupyter exports the files directly to jupyter notebook integrated within LogMan.io.
  - choose a predefined e-mail template from the Library or create your own
- `Schedule`
  - allows you to plan your export
  - fill in date and time for one-off exports
  - strictly follow YYYY-MM-DD HH:mm format - your date might look like this: _2023-01-01 12:00_.
  - the second variant handes periodicaly scheduled exports. Use _cron_ syntax for this option
- <button style="color:#325ee3; padding:3px">Add new header</button>
  - adds more columns
  - change the order of the columns by dragging
  - the same order of columns will be in the resulting table
  - if you do not fill in the columns or their names do not match the search terms in the database, your table will be empty.

You can refer to http://en.wikipedia.org/wiki/Cron for more details:

<img style="width:15px; height:15px; vertical-align:middle;" src="checkmark.svg"/>
random “R” definition keywords are supported

<img style="width:15px; height:15px; vertical-align:middle;" src="checkmark.svg"/>
Vixie cron-style “@” keyword expressions are supported.

<br>

Query: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html

<span style="color:#325ee3">Advanced</span> button lead you to YAML editor. Please see <span style="color:#325ee3">Advanced</span> section for more infromation.

<br>

<h2 style="color:#325ee3" id="advanced">Advanced</h2>

Exports defined in the `Exports` section of the Library and Advanced Exports share common YAML format.
Export files (or advanced exports) may contain following sections:  
<br>

_Advanced export example - define_

```
define:
  name: Export e-mail
  datasource: elasticsearch
  output: csv (options: raw / csv / xlsx)
  header: ["Column1", "Column2", "Column3"]
  schedule: 2023-01-01 00:00 (options: datetime / timestamp / cron)

```

### _define_

The define section includes the following parameters:

- `name`: The name of the export.
- `datasource`: The name of the DataSource declaration in the Library, specified as a specified as an absolute path to the Library.
- `output`: The output format for the export. Available options are "raw", "csv", and "xlsx" for ES DataSources, and "raw" for Kafka DataSources.
- `header`: When using "csv" or "xlsx" output, you must specify the header of the resulting table as an array of column names. These will appear in the same order as they are listed.
- `schedule`- There are three options how to schedule an export

  - **datetime** in a format YYYY-MM-DD HH:mm (e.g. 2023-01-01 00:00)
  - **timestamp** as integer (e.g. 1674482460)
  - **cron** - you can refer to http://en.wikipedia.org/wiki/Cron for more details, random “R” definition keywords are supported, and remain consistent only within their croniter() instance, Vixie cron-style “@” keyword expressions are supported.

  <br>

_Advanced export example - target_

```
target:
  type: download (options: download / email / jupyter)
  email: to (other optional: cc / bcc / from / subject / body)

```

### _target_

The target section includes the following parameters:

- `type`: An array of target types for the export. Possible options are "download", "email", and "jupyter". "download" is always selected if the target section is missing.
- `email`: For email target type, **you must specify at least the `to` field**, which is an array of recipient email addresses. Other optional fields include:
  - `cc`: an array of CC recipients
  - `bcc`: an array of BCC recipients
  - `from`: the sender's email address (string)
  - `subject`: the subject of the email (string)
  - `body`: a file name (with suffix) stored in the Template folder of the library, used as the email body template. You can also add special `parameters` to be used in the template.  
    Otherwise, use any keyword from the define section of your export as a template parameter:
    - `name`, `datasource`, `output` (for any export)
    - `compression`, `header`, `schedule`, `timezone`, `tenant` (for a specific export)

#### query

The query field must be a string containing an ElasticSearch query object. Please refer to ES documentation: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html
