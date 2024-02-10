"""
Script Name: log_parser.py
Version: 1.0
Description: This script is designed to parse log files exported from Fortinet-FortiAnalyzer. It filters entries based on source and/or destination IP addresses, employing multiprocessing for enhanced efficiency with large files. The script facilitates the preparation of logs for insertion into any SIEM platform, aiding in use case creation or threat hunting activities.
License: MIT License
Created by: Marion Renaldo Rotinsulu

Usage:
    python log_parser.py <path_to_log_file> [--srcip <source_ip>] [--dstip <destination_ip>] [--debug]

Requirements:
    Python 3.x
"""

import argparse
import logging
from multiprocessing import Pool, cpu_count
import os
import time

# Setup logging configuration
logging.basicConfig(filename='log_parser_activity.log', level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s', datefmt='%Y-%m-%d %H:%M:%S')

def process_lines(args):
    filepath, start_line, end_line, srcip, dstip = args
    results = []
    try:
        with open(filepath, 'r') as file:
            for i, line in enumerate(file):
                if i < start_line or i >= end_line:
                    continue
                if srcip and srcip not in line:
                    continue
                if dstip and dstip not in line:
                    continue
                results.append(line.strip())
    except Exception as e:
        logging.error(f"Error processing lines {start_line} to {end_line}: {e}")
    return results

def line_count(filepath):
    try:
        with open(filepath, 'r') as file:
            return sum(1 for _ in file)
    except Exception as e:
        logging.error(f"Error counting lines: {e}")
        return 0

def main(filepath, srcip, dstip, debug):
    if srcip is None and dstip is None:
        logging.error("Error: At least one of --srcip or --dstip must be provided.")
        print("Error: At least one of --srcip or --dstip must be provided.")
        return

    start_time = time.time()

    logging.info('Starting script')
    total_lines = line_count(filepath)

    if total_lines > 500:
        logging.info('File exceeds 500 lines, utilizing multiprocessing')
        num_workers = cpu_count()
        lines_per_worker = total_lines // num_workers
        args = [(filepath, i * lines_per_worker, (i + 1) * lines_per_worker if i < num_workers - 1 else total_lines, srcip, dstip) for i in range(num_workers)]
        
        with Pool(num_workers) as pool:
            results = pool.map(process_lines, args)
    else:
        logging.info('File does not exceed 500 lines, processing in a single thread')
        results = [process_lines((filepath, 0, total_lines, srcip, dstip))]

    combined_results = [item for sublist in results for item in sublist]
    logging.info('Processing complete')

    if debug:
        debug_filename = f"DEBUG-{os.path.basename(filepath)}"
        with open(debug_filename, 'w') as debug_file:
            for line in combined_results:
                debug_file.write(line + '\n')
        logging.info(f'Results saved to {debug_filename}')
    
    for line in combined_results:
        print(line)

    end_time = time.time()
    duration = (end_time - start_time) * 1000
    logging.info(f"Script execution time: {duration:.2f} ms")

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description="Log File Parser with srcip, dstip filtering, and debug mode")
    parser.add_argument('logfile', help="Path to the log file")
    parser.add_argument('--srcip', help="Source IP to filter by", default=None)
    parser.add_argument('--dstip', help="Destination IP to filter by", default=None)
    parser.add_argument('--debug', help="Enable debug mode to print and save results", action='store_true')
    args = parser.parse_args()

    main(args.logfile, args.srcip, args.dstip, args.debug)
