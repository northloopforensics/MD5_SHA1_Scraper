The tool is designed to be fed a folder containing PDFs.  It will then open each PDF, read the contents, locate any data that appears to be a MD5 or SHA1 hash value, and then add those values to a CSV file.

It is a command line tool that will ask for:
1.	The folder containing PDFs you want to scrape
2.	The CSV file where you want to send results

To run it, open a command prompt at the folder containing the executable and provide:

hash_scraper_CLI.exe “path to folder” “path to csv” 


