Nginx Log Analyzer

A simple Bash tool to analyze Nginx access logs from the command line. It parses a standard access log file and provides key insights such as the top IP addresses, requested paths, status codes, and user agents2•4.

Features

Top 5 IP addresses with the most requests: Identifies the most active visitors.
Top 5 most requested paths: Shows the most popular endpoints or pages on your server.
Top 5 response status codes: Gives a quick overview of the types of responses your server is serving (e.g., success, errors).
Top 5 user agents: Reveals the most common browsers or bots accessing your site.
Requirements

A Unix-like environment (Linux, macOS, WSL on Windows).
A standard Nginx access log file.
The following command-line utilities must be available: awk, sort, uniq, head, cut.
Installation

Save the script code above as analyze-logs.sh.
Make the script executable:
bash
chmod +x analyze-logs.sh
(Optional) Move the script to a directory in your PATH for easier access, e.g., /usr/local/bin/:
bash
sudo mv analyze-logs.sh /usr/local/bin/analyze-logs
Usage

Run the script and provide the path to your Nginx access log file as an argument.

Basic Syntax

bash
./analyze-logs.sh <path-to-your-log-file>
Example

First, download the sample log file as suggested in the project description:

bash
wget https://gist.githubusercontent.com/kamranahmedse/e66c3b9ea89a1a030d3b739eeeef22d0/raw/77fb3ac837a73c4f0206e78a236d885590b7ae35/nginx-access.log -O access.log
Then, run the analyzer:

bash
./analyze-logs.sh access.log
Example Output

Analyzing log file: access.log
========================================
Top 5 IP addresses with the most requests:
45.76.135.253 - 1000 requests
142.93.143.8 - 600 requests
178.128.94.113 - 50 requests
43.224.43.187 - 30 requests
178.128.94.113 - 20 requests

Top 5 most requested paths:
/api/v1/users - 1000 requests
/api/v1/products - 600 requests
/api/v1/orders - 50 requests
/api/v1/payments - 30 requests
/api/v1/reviews - 20 requests

Top 5 response status codes:
200 - 1000 requests
404 - 600 requests
500 - 50 requests
401 - 30 requests
304 - 20 requests

Top 5 user agents:
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36" - 800 requests
"curl/7.58.0" - 600 requests
"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36" - 150 requests
"Python-urllib/3.6" - 50 requests
"-" - 30 requests

========================================
Analysis complete.
(Note: The output above is illustrative and will vary based on the content of your log file.)

How It Works

The script chains together standard Unix commands to process the log file efficiently:

awk '{print $N}': Extracts the Nth column from each line of the log file. For example, $1 is the IP address, $7 is the path, and $9 is the status code1.
sort: Sorts the extracted lines alphabetically. This is required for uniq to work correctly.
uniq -c: Collapses adjacent identical lines and prefixes them with a count of occurrences.
sort -rn: Sorts the lines numerically (-n) and in reverse (-r) order, putting the lines with the highest count at the top.
head -5: Truncates the output to only the top 5 lines.
cut -d '"' -f 12: Used specifically for the user agent, this command cuts the line using a double-quote as a delimiter and extracts the 12th field, which contains the full user agent string5•6.
Stretch Goals & Advanced Ideas

Multiple Solutions: As suggested, you can achieve similar results using other tools. For example, grep and sed could be used for filtering and extraction instead of awk.
Time-based Filtering: Use grep to filter the log for a specific date range before piping it to the analysis commands3.
Error Analysis: Create a dedicated report for 404 "Not Found" errors, showing the top paths that are returning them5.
Bandwidth Analysis: Calculate the total bandwidth served by summing the response size field (typically $10) in the log6.
