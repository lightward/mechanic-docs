# FTP

The **FTP** action uploads files to server via [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol), or [FTPS](https://en.wikipedia.org/wiki/FTPS). The files to be uploaded are evaluated using [**file generators**](file-generators/).

A connecting service like [Couchdrop](https://couchdrop.io/) can be used to relay these uploads on to other cloud locations, like Dropbox, Google Drive, and Amazon S3.

## Options

| Option | Description |
| :--- | :--- |
| `host` | Required; the hostname of the destination server |
| `user` | Required; a string |
| `uploads` | Required; an object whose keys are file paths \(relative or absolute\), and whose values are [file generators](file-generators/) |
| `mode` | Optional; only available for FTP; can be set to `"ascii"` |
| `password` | Optional; a string |
| `port` | Optional; an integer specifying the server port on which FTP or SFTP is available |
| `private_key_pem` | Optional; only available for SFTP; a PEM-formatted certificate for authentication |
| `protocol` | Optional; can be `"ftp"`, `"sftp"`, or `"ftps"` |

### Authentication

The `user` option is always required.

When connecting to an FTP or FTPS server, authenticate with the `password` option.

When connecting to an SFTP server, authenticate using either `password` or `private_key_pem`. PEM certificates may be given directly in the task code:

```javascript
{% capture private_key_pem %}
-----BEGIN OPENSSH PRIVATE KEY-----
l0UGrDQWWbOpUsLENHwD5ya478pmRXarmDj5Wh31B54nmuq7be4ZKD5eh9nEV42JCl4mX6
...
pZ/WFoT82brhooSfJDue14C0Y=
-----END OPENSSH PRIVATE KEY-----
{% endcapture %}

{% action "ftp" %}
  {
    "host": "example.com",
    "port": 22,
    "user": "sftp_user",
    "private_key_pem": {{ private_key_pem | json }}
    "uploads": {
      "success.txt": "hooray!"
    }
  }
{% endaction %}
```

### File paths

The `uploads` option is an object whose keys are file paths. If only the filename is given \(e.g. `"sample.pdf"`\), the generated file will be uploaded to the home directory of the user. If a relative path \(e.g. `"subdirectory/sample.pdf"`\) or absolute path \(e.g. `"/tmp/sample.pdf"`\) is given, it will be respected.

#### Example

This example action results in \(a\) an upload to an absolute path, starting from the server root, \(b\) an upload to a nested directory within the user's home folder, and \(c\) an upload to a nested directory in another user's home folder \(which may fail, depending on filesystem permissions\).

```javascript
{% action "ftp" %}
  {
    ...
    "uploads": {
      "/absolute/path/to/success.txt": "hooray!",
      "relative/path/to/success.txt": "hooray!",
      "../another/relative/path/to/success.txt": "hooray!"
    }
  }
{% endaction %}
```

## Testing

If a server is unavailable for testing, consider using [Couchdrop](https://couchdrop.io/), with [their hosted storage service](https://couchdrop.io/features/hosted-storage). This is a \(nearly\) configuration-free avenue for testing, using the hosts ftp.couchdrop.io or sftp.couchdrop.io.

Alternatively, [ngrok](https://ngrok.com/) can be used to create a public tunnel to a local FTP or SSH server. By running `ngrok tcp 22` \(adjusting for the appropriate local port\), ngrok will generate a temporary public host and port that's appropriate for use while testing.

## Example

This task compiles all SKUs with their titles and prices, and uploads it as a CSV every night or on demand.

{% tabs %}
{% tab title="Subscriptions" %}
```text
mechanic/scheduler/daily
mechanic/user/trigger
```
{% endtab %}

{% tab title="Code" %}
```javascript
{% assign csv_rows = array %}

{% assign header = "SKU,Title,Price" | split: "," %}
{% assign csv_rows[0] = header %}

{% for product in shop.products %}
  {% for variant in product.variants %}
    {% assign title = variant.title %}
    {% if title == "Default Title" %}
      {% assign title = product.title %}
    {% endif %}

    {% assign row = array %}
    {% assign row[row.size] = variant.sku %}
    {% assign row[row.size] = title %}
    {% assign row[row.size] = variant.price %}

    {% assign csv_rows[csv_rows.size] = row %}
  {% endfor %}
{% endfor %}

{% capture filename %}product-export-{{ "now" | date: "%Y-%m-%d" }}.csv{% endcapture %}

{% action "ftp" %}
  {
    "host": "example.com",
    "port": 21,
    "user": "anonymous",
    "password": null,
    "uploads": {
      {{ filename | json }}: {{ csv_rows | csv | json }}
    }
  }
{% endaction %}
```
{% endtab %}
{% endtabs %}

