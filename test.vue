def process_html_file(input_file, output_file):
    """Reads HTML from input file, processes it, and writes to output file"""
    try:
        with open(input_file, 'r', encoding='utf-8') as f:
            html = f.read()
        
        processed_html = merge_empty_leading_tds(html)
        
        with open(output_file, 'w', encoding='utf-8') as f:
            f.write(processed_html)
        
        print(f"Successfully processed HTML. Output saved to {output_file}")
    except Exception as e:
        print(f"Error processing file: {e}")

# Example usage
if __name__ == "__main__":
    input_filename = "input.html"  # Change to your input file
    output_filename = "output.html"  # Change to your output file
    
    process_html_file(input_filename, output_filename)
