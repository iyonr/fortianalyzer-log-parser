# fortianalyzer-log-parser
## Description

This Python script is tailored for parsing log files exported from Fortinet-FortiAnalyzer. It's specifically designed to assist in filtering log entries based on source and/or destination IP addresses, making it an invaluable tool for preparing logs for insertion into any SIEM platform. This facilitates the creation of use cases or aids in threat hunting activities by providing a streamlined way to analyze Fortinet logs outside of FortiAnalyzer.

## Features

- **IP Address Filtering**: Enables filtering by source IP (`--srcip`) and destination IP (`--dstip`) to isolate relevant log entries.
- **Multiprocessing Support**: Utilizes multiprocessing for large log files (over 500 lines), significantly improving processing efficiency.
- **Debug Mode**: Includes a debug mode (`--debug`) for printing detailed information and saving filtered results, enhancing analysis and troubleshooting.
- **Performance Insights**: Logs the script's execution time in milliseconds, offering performance insights.

## Requirements

- Python 3.x: Ensure Python 3 is installed on your system.

## Installation

No specific installation required. Simply clone or download the script to your local machine:

```bash
git clone <repository-url>
```

Or copy the script into a `.py` file if you're not using Git.

## Usage

Navigate to the directory containing `log_parser.py` and execute it with the following syntax:

```bash
python log_parser.py <path_to_log_file> [--srcip <source_ip>] [--dstip <destination_ip>] [--debug]
```

### Examples

- **Filtering by Source IP**:
  ```bash
  python log_parser.py my_log_file.log --srcip 192.168.1.1
  ```

- **Filtering by Destination IP**:
  ```bash
  python log_parser.py my_log_file.log --dstip 10.1.1.1
  ```

- **Using Debug Mode for Detailed Analysis**:
  ```bash
  python log_parser.py my_log_file.log --srcip 192.168.1.1 --debug
  ```

Results are saved to a file prefixed with `DEBUG-` and detailed processing information is printed to the console.

## Licensing

Released under the MIT License. See the `LICENSE` file for more details.

## Note

This tool is developed to extend the utility of Fortinet-FortiAnalyzer log files for SIEM integration or threat hunting without relying on FortiAnalyzer's built-in capabilities. It is designed for security analysts and IT professionals looking to enhance their threat intelligence and use case development.
