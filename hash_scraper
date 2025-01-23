import os
import re
import csv
import fitz  # PyMuPDF
import PyPDF2
import argparse

def read_pdf(file_path):
    text = ''
    try:
        with fitz.open(file_path) as pdf:
            for page in pdf:
                text += page.get_text()
    except:
        try:
            with open(file_path, 'rb') as file:
                pdf = PyPDF2.PdfFileReader(file)
                for i in range(pdf.getNumPages()):
                    text += pdf.getPage(i).extractText()
        except:
            print(f"Error reading file: {file_path}")
    return text

def find_md5_hashes(text):
    md5_pattern = r'\b[a-fA-F0-9]{32}\b'
    return re.findall(md5_pattern, text)

def find_sha1_hashes(text):
    sha1_pattern = r'\b[a-fA-F0-9]{40}\b'
    return re.findall(sha1_pattern, text)

def write_to_csv(data, csv_file):
    with open(csv_file, 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(['Source File', 'MD5 Hash', 'SHA-1 Hash'])
        for row in data:
            writer.writerow(row)

def main():
    parser = argparse.ArgumentParser(description="Hash Scraper CLI")
    parser.add_argument('folder', type=str, help="Folder containing PDFs")
    parser.add_argument('csv_file', type=str, help="Path to output CSV file (must end in .csv)")
    args = parser.parse_args()

    folder_path = args.folder
    csv_file = args.csv_file
    data = []

    print(f"Processing files in folder: {folder_path}")

    for root, dirs, files in os.walk(folder_path):
        for filename in files:
            if filename.endswith('.pdf'):
                file_path = os.path.join(root, filename)
                print(f"Processing file: {file_path}")
                text = read_pdf(file_path)
                md5_hashes = find_md5_hashes(text)
                sha1_hashes = find_sha1_hashes(text)
                
                # Ensure all hashes are captured
                max_len = max(len(md5_hashes), len(sha1_hashes))
                for i in range(max_len):
                    md5 = md5_hashes[i] if i < len(md5_hashes) else ''
                    sha1 = sha1_hashes[i] if i < len(sha1_hashes) else ''
                    data.append([filename, md5, sha1])
                    print(f"Found MD5: {md5} and SHA-1: {sha1} in file: {filename}")

    write_to_csv(data, csv_file)
    print(f"Data written to CSV file: {csv_file}")

if __name__ == '__main__':
    main()
